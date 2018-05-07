---
layout: post
title: "Page object pattern и BaseTest"
tags: Java Selenide Junior Web
---

## Зачем нужны Page и BaseTest классы?
Рассмотрим простой тест:

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
<!--more-->

Давайте разберем проблемы этого теста:
1. Сложно создать новый тест, так как нам придется:
  * Заново указывать браузер, где будет запущен тест.
  * Заново писать локаторы элементов, которы уже описывали.
  * Опять закрывать драйвер вручную.
  * Указывать сайт.
2. Сложно вносить изминения (refactoring):
  * Если у нас будет 100+ тестов, а поменяется только один локатор, надо исправлять в 100+ местах.
3. Сложно запустить тест на других браузерах.
  * В каждом тесте придется менять значение Configuration.browser .


## Решение проблемы.
### Page object.
Page класс описывает страницу сайта. В проекте их столько же сколько и страниц на сайте, который тестируем.
В данном примере создадим два класса MainPage и SearchResultPage.

{% highlight Java %}
public class MainPage {
      private static final By SEARCH_FIELD = By.cssSelector("#header-search > div > form > div > input");
      private static final By SEARCH_BUTTON = By.cssSelector("#header-search > div > form > div > span.input-group-btn > button");

      public MainPage inputSearch(String query) {
          $(SEARCH_FIELD).setValue(query);
          return this;
      }

      public SearchResultPage clickSearchButton() {
          $(SEARCH_BUTTON).click();
          return new SearchResultPage();
      }
}
{% endhighlight %}

{% highlight Java %}
public class SearchResultPage {
    private static final By EMPTY_RESULT = By.id("empty-search-results");

    public SearchResultPage checkNoSearchResultsMessage(String query) {
        String noResultText = String.format("К сожалению, по запросу «%s» мы ничего не смогли найти.", query);
        $(EMPTY_RESULT).shouldHave(text(noResultText));
        return this;
    }
}
{% endhighlight %}

Рассмотрим наши классы:
<br>Локаторы элементов и взаимодействия с ними из теста мы перенесли в методы с human readable названиями.
Локаторы мы вынесли в константы, которые хранятся в начале Page класса.

На Page должны находится только простые действия, которые мы сможем использовать в других тестах.
{:.info}

Возникает вопрос:
 * Почему мы в методах возвращаем ее или другую?

 Ответ:
  * Благодаря этому мы сможем получать все методы для данной страницы и не сможем обратиться к методу и элементу, которые находят на другой странице сайта. Если метод переводит нас на другую страницу, он возвращает новый объект, как пример - метод clickSearchButton().

  Среда разработки будет помогать, показывая после '.' (точки) возможные методы для нашей Page. Написание тестов становится похожим на конструктор,
  где инженер из методов собирает готовый тест.
  {:.info}

Написав два класса мы решили две проблемы:
1. Заново писать локаторы элементов, которы уже описывали.
- Теперь можно вызвать нужный нам метод или написать новый, который будет брать нужный локатор из константы.
2. Сложно вносить изминения (refactoring):
- Теперь достаточно изменить изменить констану в Page классе и все тесты будут использовать новый локатор.

### BaseTest всему голова.
Создадим BaseTest class:

{% highlight Java %}
public class BaseTest {

    private static final String SHOP_SITE = "https://www.dns-shop.ru/";

    private void selectChrome() {
        Configuration.browser = "chrome";
    }

    public MainPage openSite() {
        selectChrome();
        open(SHOP_SITE);
        return new MainPage();
    }

    @AfterTest
    public void close() {
        getWebDriver().close();
    }
}
{% endhighlight %}

Рассмотрим класс:
- SHOP_SITE - константа хронящяя url нашего сайта.
- selectChrome() - метод устанавливающий браузер для запуска
- openSite() - вызывает метод selectChrome, после открывает сайт и возвращает MainPage. Что бы мы смогли сразу работать с методами главной страницы.
- close() - будет закрывать драйвер после каждого теста.

Решили проблемы:
1. Заново указывать браузер, где будет запущен тест.
- Указываем в одном месте и все тесты будут идти на данном браузере.
2. Закрывать драйвер вручную.
- Драйвер будет закрываться автоматически после каждого теста.
3. Заново казывать сайт.
- Указываем сайт один раз в константе.

##Тест после доработок.
{% highlight Java %}
public class NoSearchResults extends BaseTest {
    private static final String NO_RESULTS_QUERY = "qweqwe";

    @Test
    public void noSearchResults() {
        openSite()
                .inputSearch(NO_RESULTS_QUERY)
                .clickSearchButton()
                .checkNoSearchResultsMessage(NO_RESULTS_QUERY);
    }
}
{% endhighlight %}

Рассмотрим тест:
- extends BaseTest - каждый новый тест нужно наследовать от BaseTest. Мы наследуем метод openSite и close (который будет вызываться автоматически благодаря аннотации).
-   openSite()
          .inputSearch(NO_RESULTS_QUERY) - открываем сайт и получаем после '.' метод MainPage и дальше строим наш тест.


### Итоги.
Мы написали хороший автоматизированный тест, который можно легко изменять. Локаторы теперь хранятся в одном месте и разделены логически по страницам. Плюс мы можем сразу задавать браузер для всех тестов в одном месте и не переживать о закрытии драйвера.

Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY] <br>Branch: *page-object-pattern*
{:.info}
[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
