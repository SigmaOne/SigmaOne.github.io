---
title: "Java web-stack"
ref: java-web-stack
layout: post
lang: en
---

#### Article is not inished yet! Because the topic is so huge.

Hello there, today i do want to summarize my whole JavaEE Course called "Programming of internet Applications"
in my lovely university, i want to tell where the Java is used and how it works.

### What is Java
Java - general purpose explicit language which can everything :)
I always wanted to describe a whole language by one piece of code. Alright, let's try.

Java is:

```Java
class Chart {
  private List<Point> points = new ArrayList<>();

  public void addPoint(Point point) {
    points.add(0, point);

    // free lst if it' filled
    if (points.size() > 10)
        points.remove(points.size() - 1);
  }
  public List<Point> getPoints() {
    return points;
  }
}

public class Program {
  public static void main(String[] args) {
    Chart chart1 = new Chart();
    Chart chart2 = new Chart();

    // do some logic here
  }
}
```

Java platform is divided from Java Standard Edition and Java Enterprise Edition.

### Java Standard Edition
Java standard edition(JavaSE further) - It's kind of language itself plus standard class library.
JavaSE defines wide general purpose API's.  

JavaSE includes:

* Java Language Specification - Language description, so you can write your own compiler.
* Java Class Library - Library you can use, there are ArrayList, Pair, Map classes and more.
* Java Virtual Machine Specification - Bytecode runtime environment description, so you can write your own Virtual machine.

Everyone familiar with Java knows the Oracle's Java Development Kit - canonical JavaSE realization.

### Java Enterprise Edition
Java Enterprise edition(JavaEE further) - It's JavaSE extension, huge web-development platform.
JavaEE includes API(different tier frameworks) and runtime environments(java-application-servers) for development and executing enterpirse-applications.
Enterprise application in general is an applications that helps company's business. Enterprise application can be a
complex site or mobile(JavaEE is only web) application.

Developers of JavaEE tried to achieve next goals:

* Huge applications - well, it's obvious
* Multi-tier applications - so different developers can work on their own piece(frontend, backend).
* Extensibility - so app can grow with company.
* Security - so no secrets can be exposed because of platform vulnerability.
* Reliability - so application can do everything.


## JavaEE history
После того, как бизнес увидел в сайтах для их продуктов прок, компаниям-программистам стали заказывать сайты.
А затем сайты посложенее, а затем ещё сложнее. Пояилась потребность программно писать динамические сайты с большой логикой. Как всё это развивалось:

1. CGI-скрипты, из-за них появились следующие
2. Рождение Java-application-серверов и с ними Сервлетов
3. Java Server Pages(JSP)
4. Java Server Faces(JSF)

Все далее идующие пункты опишут каждый из пунктов истории. Итак, как всё это было.

### Что такое CGI-скрипт
Вначале ребята просто подумали, а давайте просто писать сайты динамически своим любимым языком. Такие программы называются CGI-скрипты
CGI-скрипт это просто программа на любом языке программирования, которая пишет руками в ответ сервера html-разметку. Веб-сервер при наличии нового запроса
просто завязывает стандартные дескрипторы ввода-вывода(stdin в HTTPRequest.getReader(), stdout в HttpResponse.getWriter(), stderr в ...) и запускает её:
Каждый CGI-скрипт - это самостоятельный процесс, отпускаемый системой. Поэтому если вас попробуют заддосить, то оперативки не хватит :)

Пример CGI-скрипта:

```C++
#include <iostream>
#include <stdlib.h>
using namespace std;

int main () {
   cout << "Content-type:text/html\r\n\r\n";
   cout << "<html>\n";

   cout << "<head>\n";
   cout << "<title>CGI-скрипт</title>\n";
   cout << "</head>\n";

   cout << "<body>\n";
   cout << "<h2>Hello World!</h2>\n";
   cout << "<h2>Random number for today is: " << rand() << "</h2>\n";
   cout << "</body>\n";

   cout << "</html>\n";

   return 0;
}
```

Минусы CGI-скриптов - код, который как-никак, но будут писать дизайнеры-верстальщики, и затратность этого подхода из-за того, что
каждый CGI-скрипт работает в своём процессе.

### Java-application-server и Сервлеты
Ребята из oracle начали думать над тем, как же решить эту проблемы CGI, и придумали java-web-сервера

В чём идея: Будет у нас веб-контейнер, который будет следить за всем,от безопасности до многопоточности, его можно будет конфигурировать.
У веб-сервера будут различные контейнеры(web, ejb типов), в которых будут жить классы, реализующие логику.
Контейнер следит за безопасностью, многопоточностью и иньекциями классов друг в друга. Он сам создаёт пул объектов и управляет им.
Так вот Сервлеты - просто java-классы, умеющие отвечать на http-запросы, живущие в веб-контейнере.
Сервлет по смыслу - CGI-скрипт, только все недостатки CGI-скриптов устраняет контейнер.
Также, добавляется некоторая функциональность, например, можно передавать информацию между сервлетами, хранить её в контексте приложений,
Добавления сессий и прочее.

Пример сервлета:

```java
public class SimpleServlet extends HttpServlet {
    @Override
    public void init() {
      // do the dance, i mean, all the preparations
    }

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) {
        response.setCharacterEncoding("UTF-8");

        try(PrintWriter writer = response.getWriter()) {
            writeStaticPageContent(writer, x, y, r, isInArea);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public void doHead(HttpServletRequest request, HttpServletResponse response) {
      // Head logic
    }

    @Override
    public void doPost(HttpServletRequest request, HttpServletResponse response) {
      // Post logic
    }

    private void writeStaticPageContent(PrintWriter responseWriter) {
        responseWriter.write("" +
        "<!DOCTYPE html>  \n" +
        "<html lang=\"en\"> \n" +
        "<head> \n" +
        "    <meta charset=\"UTF-8\"> \n" +
        "    <title>Simple servlet</title> \n" +
        "</head> \n" +
        " \n" +
        "<body>\n" +
        "    <h2>Hello World!</h2>\n");

        for (double i = 1; i <= 3; i += 0.5) {
            writer.write("" +
            "         <label>\n" +
            "            <input type=\"checkbox\" name=\"r\" value=\"" + i + "\" />\n" +
            "            " + i + "\n" +
            "         </label>\n");
        }

        writer.write("" +
        "</body> \n" +
        "</html>");
    }
}
```

Недостакти сервлетов - всё ещё на очень низком уровне. Дизайнеру тяжело.

### Java Server Pages
Дизайнеры просто вывихнули бы себе мозг, верстая Java-кодом, поэтому для них решили сделть спасение
Суть Java Server Pages(Далее - JSP) - Html-страницы с java-вставками. Транслируются Jasper'ом(JSP-транслятор) в сервлеты.
В JSP можно давать инструкции транслятору, определять методы, классы, вставлять Java-код, и иметь доступ к созданным экземплярам-бинов.
Java Bean - просто Java-класс, все названия методов которого унифицированы. Bean обязан иметь конструктор без параметров, и все его методы-аксессоры
начинаться с get, set, или is: getProperty, setProperty, isBoolean

Пример JSP

```html
<html xmlns="http://www.w3.org/1999/xhtml" lang="en" xml:lang="en">
<head>
    <meta charset="UTF-8">
    <title>Lab 7 - Servlets</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
</head>

<body>
  <jsp:useBean id="dbc" class="DataBaseConnector"  scope = *application*/>
  <h3>
      Старался: <%= dbc.getAuthorName() =%> <br/>
      Группа: <%= dbc.getAuthorGroup() =%> <br/>
      Вариант: <%= dbc.getAuthorOption() =%> <br/>
  </h3>

  <% for (int i = -4; i <= 4; i++) { %>
    <label>
        <input type="button" name="x" value="<%= i %>"/>
    </label>
  <% } %>
</body>
```

### Java Server Faces
Преддверие к Java Server Faces(Далее - JSF) - паттерн Model-View-Controller(Далее - MVC)

* Model - Логика приложения, данные и методы их изменения. То, что не видит пользователь.
* View - Отображение приложения, его вид. Всякие таблицы, кнопки, поля. То, что видит пользователь.  
* Controller - Элемент, которые связывает действия(нажатия кнопок) View с Model-логикой

Релизация MVC в JSF
Model - ORM framework, MBeans
View - Facelets
Controller - Faces controller(Стндартный сервлет для JSF)

View - разметка JSF-страницы


Модель - логика, подкрепляющая страницу

```Java
@ManagedBean(name="mainManager", eager=true)
@ApplicationScoped
public class MainPageManager {
    List<Point> pointsCache = new ArrayList();
    DataBaseConnector dbc = new DataBaseConnector();

    public void addPoint(float x, float y) {
        pointsLog.add(0, new Point(x, y));

        dbc.persist(pointsLog);
    }

    public List<Point2D.Float> getPointLog() {
      if (!dbc.isCacheValid(pointsCache))
        pointsCache = dbc.retrieve("Select * from points");

      return pointsLog;
    }
}
```

Контроллер - конфигурация web-контейнера

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>Faces Servlet</servlet-name>
        <servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>Faces Servlet</servlet-name>
        <url-pattern>*.xhtml</url-pattern>
    </servlet-mapping>
</web-app>
```

JSF - Конфетка, если сравнить со всем прошлым. Буду говорить о каждом компоненте MVC по отдельности

### View
JSF-страницы состоят из JSF - разметки. JSF-разметка - просто расширение Html разметки, транслируется FacesServlet'ом в Html-разметку.
JSF-компонеты можно писать самому, поэтому существуют различные библиотеки тэгов, предоставляющие куски функциональность.
Синтаксис #{backingBean.property} - это EL-language, язык доступа к backing бинам. Супер-удобный Ajax
Примеры JSF-Библиотек:

* Primefaces
* Icefaces
* RichFaces

Примеры jsf-страницы:

```Html
<!-- Определения пространств-имён тегов -->
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:p="http://primefaces.org/ui"
      xmlns:f="http://java.sun.com/jsf/core">

<!-- p:{something} - указатель на библиотеку primefaces -->
<!-- р:{something} - указатель на стандартную библиотеку -->

<h:head>
    <title>Main page</title>
    <h:outputStylesheet name="style.css" library="css"/>
</h:head>

<h:body>
    <h:form id="form">
        <p:commandButton value="Add" process="@form" update="chart pointsLog" actionListener="#{mainManager.addCurrentPoint}" />
    </h:form>
    <p:dataList id="pointsLog" value="#{mainManager.pointLog}" var="point" type="ordered" emptyMessage="No points inserted yet!">
          #{point.x}, #{point.y}
    </p:dataList>
</h:body>
</html>
```

### Model
Model в JSF - ManagedBeans и ORM framework.

#### ManagerBeans
ManagedBeans - просто Java-классы, подкрепляющие страницу. Страница будет тянуть данные из них.

```Java
@ManagedBean(name="mainManager", eager=true)
@ApplicationScoped
public class MainPageManager {
    List<Point> pointsCache = new ArrayList();
    DataBaseConnector dbc = new DataBaseConnector();

    public void addPoint(float x, float y) {
        pointsLog.add(0, new Point(x, y));

        dbc.persist(pointsLog);
    }
    public List<Point2D.Float> getPointLog() {
      if (!dbc.isCacheValid(pointsCache))
        pointsCache = dbc.retrieve("Select * from points");

      return pointsLog;
    }
}
```

#### ORM framework
Рассказать о Hibernate
Оборачивает табличную базу данных в объекты

### Controller
Различные аннотации над ManagedBean'ами, EJB-бинами.

@RequestScoped - Stores the bean for the duration of the HTTP request.
@ViewScoped	- Stores the bean until the ID for the current view changes.
@SessionScoped - Stores the bean for the duration of the session
@ApplicationScoped - Stores the bean for the duration of the application.
@NoneScoped - The bean is instantiated each time it is referenced, and is not saved in any scope.
@CustomScoped - An EL expression that points to a map which controls the visibility and lifetime of beans.

Пример конфигурации:

```Java
@ManagedBean(name="mainManager", eager=true) // Для EL-language mainManager - объект этого класса. eager=true - Создай объект заранее.
@SessionScoped  // Для каждоый сессии будет свой объект
public class MainPageManager {
  // ...
}
```

### Вывод по JavaEE
JSF - венец JavaEE, но мне не понравился, т.к. слишком много неявных багов. Я как-бы не пишу логику,
а пытюсь исправить то, что не работет так, как мне нативно кажется.

Я люблю Джаву, но рабоать в сфере JavaEE я бы не хотел. Сейчас висит проект с практики, за который я очень не хоче браться,т.к.
устал фиксить баги