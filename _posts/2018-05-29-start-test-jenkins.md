---
layout: post
title: "Запуск тестов с помощью Jenkins"
tags: Jenkins grid Selenide
---
![jenkins](\assets\images\jenkins-1.png)
## Настройка Jenkins.

Запуск jenkins в docker описан в [этой статье][article]
{:.info}
В настройках дженкинса переходим в Global Tool Configuration.
- Добавляем Maven.

### Настройка Jоb'a.
1. Создаем новый item.
2. Разрешить параллельный запуск задачи.
3. Выбираем  "параметризованная сборка" и добавляем два String параметра: BROWSER, DEVICE, HUB, THREAD. (Значение по дефолту chrome, pc, http://192.168.1.155:4444/wd/hub, 3).
<!--more-->
4. Выбираем "Управление исходным кодом" - git. Пишем ссылку на наш репозиторий и указываем бранч.
5. Выбираем Сборка, Вызвать цели Maven верхнего уровня. Указать версию мавена (которую мы указали в настройках дженкинса).
6. Цели: clean integration-test -PChrome (-Р профайл)
7. Расширенные: указываем пом и проперти.
<br>browser = ${BROWSER}
<br>platform.env = ${DEVICE}
<br>hub=${HUB}
<br>thread =${THREAD}
<br> проперти из pom.xml = параметр из дженкинса.
8. Inject build variables - включить.
9. Сохранить настройки.

http://192.168.1.155:4444/wd/hub - урл хаба. Как поднять хаб и ноды [читать тут][article2].

## Настройка проекта.
За основу берем проект, который писали в преидущих статьях.

В BaseTest допишем настройку remote. С ней селенид создаст remoteWebDriver. Значение возьмем из maven профайла (локально)
или из дженкинса (selenium grid). Подробнее о проперти [тут][article3], все параметры которые указали в дженкинсе - указываем в поме.
{% highlight JAVA%}
private void selectBrowser() {
    Configuration.browser = System.getProperty("browser");
    Configuration.remote = System.getProperty("hub");
}
{% endhighlight %}


### Создаем listner для jenkins.
Листнер нужен для создания testng.xml. Он автоматически добавит в xml все тесты и создаст test suite.

{% highlight JAVA%}
public class CreateXmlForRunInParallel implements IAlterSuiteListener {
    private static Set<Method> allTestsInProject;
    private static final String PATH_WITH_TESTS = "com.qaforpeople.functionality.tests";

    @Override
    public void alter(List<XmlSuite> suites) {
        if (!suites.isEmpty()) {
            XmlSuite modifySuite = suites.get(0);
            List<XmlTest> xmlTestList = new ArrayList<>();
            Map<String, String> allTestClasses;

                allTestClasses = getAllTestClasses(getAllTests());

            for (Map.Entry<String, String> entry : allTestClasses.entrySet()) {
                XmlTest xmlTest = new XmlTest(modifySuite);
                XmlClass xmlClass = new XmlClass(entry.getValue());
                xmlTest.setClasses(Collections.singletonList(xmlClass));
                xmlTest.setName(entry.getKey());
                xmlTestList.add(xmlTest);
            }
            modifySuite.setTests(xmlTestList);

            modifySuite.setThreadCount(Integer.parseInt(System.getProperty("thread")));

            System.out.println(modifySuite.toXml());
        }
    }

    private static Set<Method> getAllTests() {
        if (allTestsInProject == null) {
            Reflections reflections = new Reflections(PATH_WITH_TESTS, new MethodAnnotationsScanner());
            allTestsInProject = reflections.getMethodsAnnotatedWith(Test.class);
        }
        return allTestsInProject;
    }

    private static Map<String, String> getAllTestClasses(Set<Method> methodSet) {
        Map<String, String> classNameAndPathMap = new HashMap<>();
        for (Method testMethod : methodSet) {
            List<String> groups = Arrays.asList(testMethod.getAnnotation(Test.class).groups());

                String classPath = testMethod.getDeclaringClass().getName();
                if (!classNameAndPathMap.containsValue(classPath)) {
                    classNameAndPathMap.put(testMethod.getName(), classPath);
                }
            }
        return classNameAndPathMap;
    }
}
{% endhighlight %}

Теперь необходимо указать дженкинсу, что он должен запустить этот листенер.
Создадим xml файл.

{% highlight XML%}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="implemented" verbose="1" parallel="tests" thread-count="2">
    <listeners>
        <listener class-name="com.qaforpeople.listeners.CreateXmlForRunInParallel"/>
    </listeners>
</suite>
{% endhighlight %}

### И дополним pom.xml.
 Пример профайла.
{% highlight XML%}
<profiles>
     <profile>
         <id>Chrome</id>
         <properties>
             <browser>chrome</browser>
             <testNGPath>${basedir}/src/main/resources/testng/jenkins.xml</testNGPath>
             <platform.env>pc</platform.env>
             <hub>http://192.168.1.155:4444/wd/hub</hub>
             <thread>3</thread>
         </properties>
     </profile>
 </profiles>
{% endhighlight %}

Добавим плагин.
{% highlight XML%}
<!--Plugin for create XML-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-failsafe-plugin</artifactId>
                <version>2.10</version>
                <configuration>
                    <argLine>-XX:PermSize=256m -XX:MaxPermSize=1024m</argLine>
                    <skip>false</skip>
                    <suiteXmlFiles>
                        <suiteXmlFile>${testNGPath}</suiteXmlFile>
                    </suiteXmlFiles>
                </configuration>
                <executions>
                    <execution>
                        <id>integration-test</id>
                        <goals>
                            <goal>integration-test</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>verify</id>
                        <goals>
                            <goal>verify</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            {% endhighlight %}

И еще добавим новую зависимость.
            {% highlight XML%}
            <!--Need for create XML listener-->
                    <dependency>
                    <groupId>org.reflections</groupId>
                    <artifactId>reflections</artifactId>
                    <version>0.9.11</version>
                    <exclusions>
                        <exclusion>
                            <artifactId>guava</artifactId>
                            <groupId>com.google.guava</groupId>
                        </exclusion>
                    </exclusions>
                </dependency>
              {% endhighlight %}

Теперь все готово к запуску проекта на jenkins с использование selenium grid.
Сейчас запуск производится в ручную. Но можно поставить на любые тригеры: время или коммиты. 


Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY]
{:.info}
[article3]:https://ereoo.github.io/2018/05/04/maven-profiles.html "maven article"
[article2]:https://ereoo.github.io/2018/05/24/grid.html "grid article"
[article]:https://ereoo.github.io/2018/05/14/jenkins-docker.html "jenkins article"
[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
