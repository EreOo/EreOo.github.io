---
layout: post
title: "Запуск тестов в несколько потоков с помощью TestNG"
tags: TestNG Junior
---

## TestNG.xml
Запуск тестов в несколько потоков значительно сокращает время прогона.
Связано это с тем, что  на каждый поток/thread создается свой WebDriver и поднимается браузер.
TestNG позволяет создать test suite, указать тесты и количество потоков.
<!--more-->

Хранить testng.xml нужно в package resources.
{:.info}
{% highlight XML%}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="implemented" verbose="1" parallel="tests" thread-count="3">

    <test name="AboutMainPageText">
        <classes>
            <class name="com.qaforpeople.functionality.tests.AboutMainPageText"/>
        </classes>
    </test>
    <test name="InputFieldsText">
        <classes>
            <class name="com.qaforpeople.functionality.tests.InputFieldsText"/>
        </classes>
    </test>
    <test name="AboutSecondPageText">
        <classes>
            <class name="com.qaforpeople.functionality.tests.AboutSecondPageText"/>
        </classes>
    </test>
</suite>
{% endhighlight %}

 thread-count="3" - количество потоков не должно быть больше, чем тестов  (это не имеет смысла) и так же большое количество может перегрузить машину.
 {:.info}

Если проект в __IntelliJ IDEA__ можно нажать на файл xml правой кнопкой мыши и запустить локально. Поднимется три браузера.

Исходный код можно найти тут: [GitHub project: WEB-QA.][TEASY] 
{:.info}

[TEASY]:https://github.com/EreOo/WEB-QA "WEB-QA project"
