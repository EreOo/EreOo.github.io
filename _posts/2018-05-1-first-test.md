---
layout: post
title: "Первый автоматизированный тест"
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
    public void checkNoSearchResult() {
        open("https://www.dns-shop.ru/");
        $(By.cssSelector("#header-search > div > form > div > input")).setValue("qweqwe");
        $(By.cssSelector("#header-search > div > form > div > span.input-group-btn > button")).click();
        $(By.id("empty-search-results")).shouldHave(text("К сожалению, по запросу «qweqwe» мы ничего не смогли найти."));
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


Без getWebDriver().close() на тестовой машине (Window 10, i7 7700k, 16gb) после прогона тестов была замечена загрузка CPU на 100%!
macOS отреагировал на отсутсвие методf закрытия более лояльно. Но считаю, что лучше прописывать driver.close().
{:.warning}

Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY] <br>Branch: *bad_simple_test*
{:.info}

[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
