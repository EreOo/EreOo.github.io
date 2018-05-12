---
layout: post
title: "Автоматизированный тест с Selenide и TestNG"
tags: Java Selenide Junior Web
---

## Подключение TestNG и Selenide.
Создаем обычный Maven проект. В pom.xml добавляем dependencies.

{% highlight XML%}
<dependencies>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>6.14.2</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>com.codeborne</groupId>
        <artifactId>selenide</artifactId>
        <version>4.9.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
{% endhighlight %}
<!--more-->
## Код теста.

В src → test → java создаем class OurFirstTest. Это и будет наш тест, который проверяет ответ сайта на невалидный запрос поиска.

{% highlight Java %}
public class OurFirstTest {

  @BeforeTest
  private void settings() {
      Configuration.browser = "chrome";
  }

  @Test
  public void checkSecondPageAboutText() {
      open("https://ereoo.github.io/main-page");
      $(By.id("go_second")).click();
      $(By.id("about")).shouldHave(text("This is second test page \"SecondPage\". First test page is \"MainPage\". This is a test page filled with common HTML elements. Feel free to practice create your auto-tests."));
  }

  @AfterTest
  private void close() {
      getWebDriver().close();
  }
}
{% endhighlight %}

### Рассмотрим аннотации TestNG:
- @BeforeTest - метод будет выполнен перед тестом.
- @Test - метод является тестом.
- @AfterTest - метод выполнятся после теста.

### Разберем методы под аннотациями:
-  private void settings()  - указываем, что тест будет запущен на Google Chrome (по умолчанию FireFox).
-  public void checkNoSearchResult() - открываем сайт, вводим в поле поиска qweqwe, нажимаем найти, проверяем ответ сайта.
-  private void close() - закрываем драйвер.

В Selenide есть shutdown driver метод, но на тестовой машине (Windows 10, i7 7700k, 16gb) было замечено, что
без getWebDriver().close()  после прогона тестов загрузка CPU 100% из за незакрытых браузеров.
macOS отреагировал на отсутсвие метода закрытия более лояльно, но считаю, что лучше прописать driver.close().
{:.warning}

Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY] <br>Branch: *bad_simple_test*
{:.info}

[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
