---
layout: post
title: "Достаем Properties из Maven profiles в Java код."
tags: Maven Java Middle
---
## Создаем Maven profile
Profile используется для сборки проекта с разными настройками. Напишем профайлы, которые помогу нам запускать тесты в
разщных браузерах. Добавим этот код в pom.xml.

{% highlight XML%}
<profiles>
     <profile>
         <id>Chrome</id>
         <properties>
             <browser>chrome</browser>
         </properties>
     </profile>
 </profiles>
{% endhighlight %}
<!--more-->
#### Рассмотрим профйл:
- \<profiles\> - между этими тегами будут храниться все наши профайлы.
- \<profile\> - сам профайл.
- \<id\> - имя профайла.
- \<properties\> - значения которые хранит профайл.
- \<browser\> - наше значенгие, которое мы сами создаем.

На примере этого профайла можно создать для других браузеров: IE, FireFox, Opera и т.д

### Maven plugin

{% highlight XML%}
<!--Plugin set custom properties-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.16</version>
                <configuration>
                    <systemPropertyVariables>
                        <browser>${browser}</browser>              
                        <buildDirectory>${project.build.directory}</buildDirectory>
                    </systemPropertyVariables>
                </configuration>
            </plugin>
        </plugins>
    </build>
{% endhighlight %}

Этот плагин поможет вытащить значение properties из профайла в Java код.

  \<browser\>${browser}\</browser\>
  <br> Эта строчка позволит обратиться из кода к данному значению.
  {:.info}

## Get properties from pom.xml
{% highlight Java%}
private void selectBrowser() {
    Configuration.browser = System.getProperty("browser");
}
{% endhighlight %}
Рассмотрим код:
- System.getProperty("browser"); - вернет нам chrome.

Теперь есть только один метод, вместо selectChrome, selectIE и т.д, который выбирает браузер в зависимости от maven profile.


### Default properties.
Если не выбрать ни один из профилей, тесты не запустятся, так как не получится создать WebDriver.

Решение:
{% highlight XML%}
<!--Default properties-->
<properties>
    <browser>firefox</browser>
</properties>
{% endhighlight %}
Теперь если не будет выбран ни один профайл, тесты запустятся на FireFox.

Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY]
{:.info}

[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
