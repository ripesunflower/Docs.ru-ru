---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 настраиваемых фильтров действий | Документация Майкрософт
author: rick-anderson
description: ASP.NET MVC предоставляет фильтры действий для выполнения логики фильтрации, до или после вызова метода действия. Фильтры действий представляют собой настраиваемые атрибуты tha...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807939"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Фильтры настраиваемых действий в ASP.NET MVC 4

по [Web Слышатся Team](https://twitter.com/webcamps)

[Скачайте комплект учебных материалов по лагеря Web](https://aka.ms/webcamps-training-kit)

ASP.NET MVC предоставляет фильтры действий для выполнения логики фильтрации, до или после вызова метода действия. Фильтры действий представляют собой настраиваемые атрибуты, которые предоставляют декларативный способ для добавления поведения перед действием и после действия к методам действий контроллера.

В этой лаборатории вы создадите атрибут фильтра настраиваемое действие в решение MvcMusicStore для перехвата запросов контроллера и записи в журнал действия узла в таблицу базы данных. Можно добавить фильтр ведения журнала с помощью внедрения любого контроллера или действия. Наконец вы увидите представление журнала, в которой отображается список посетителей.

Это Практическое лабораторное занятие предполагается, что у вас есть базовые знания о **ASP.NET MVC**. Если вы не использовали **ASP.NET MVC** раньше, мы рекомендуем к превышению **ASP.NET MVC 4 Fundamentals** Практическое лабораторное занятие.

> [!NOTE]
> Все примеры кода и фрагменты кода включены в Web лагеря комплект обучающих материалов, доступных из в [выпуски Microsoft Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Проект, относящиеся к этой лаборатории доступен в [действие настраиваемые фильтры для ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Цели

В этом Практическое лабораторное занятие, вы узнаете, как:

- Создайте атрибут фильтра настраиваемого действия для расширения возможностей фильтрации
- Применение пользовательского атрибута фильтра с помощью внедрения до определенного уровня
- Глобально зарегистрировать настраиваемых фильтров действий

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Предварительные требования

Необходимо иметь следующие элементы во укомплектовать данную лабораторию:

- [Microsoft Visual Studio Express 2012 для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) или руководителя (чтение [приложение А](#AppendixA) инструкции по его установке).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Установка

**Установка фрагменты кода**

Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступен как фрагменты кода Visual Studio. Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.

Если вы не знакомы с фрагменты кода Visual Studio и хотите научиться использовать их, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Упражнения

Это Практическое лабораторное занятие включает по следующие упражнения:

1. [Упражнение 1: Ведение журнала действий](#Exercise1)
2. [Упражнение 2: Управление нескольких фильтров](#Exercise2)

Предполагаемое время для выполнения этого лабораторного: **30 минут**.

> [!NOTE]
> Каждого упражнения сопровождается **окончания** папку, содержащую полученное решение, должен быть получен после завершения упражнения. Это решение можно использовать как руководство, если вам нужна дополнительная помощь при работе с примерами.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Упражнение 1: Ведение журнала действий

В этом упражнении вы узнаете, как создать настраиваемого журнала фильтра действий с помощью поставщиков фильтров ASP.NET MVC 4. Для этой цели будет применен фильтр ведения журнала к сайту MusicStore, который будет записывать все действия в выбранных контроллеров.

Расширить фильтр **ActionFilterAttributeClass** и переопределить **OnActionExecuting** метод перехват каждого запроса, а затем выполнять ведение журнала действий. Контекстные сведения об HTTP-запросы, выполнение методов, результаты и параметров будет предоставляться по ASP.NET MVC **ActionExecutingContext** класс **.**

> [!NOTE]
> ASP.NET MVC 4 также имеет поставщиков фильтров по умолчанию, которые можно использовать без создания пользовательского фильтра. ASP.NET MVC 4 предоставляет следующие типы фильтров:
> 
> - **Авторизация** фильтрации, который принимает решения безопасности, необходимость выполнения метода действия, такие как выполнение проверки подлинности или проверки свойств запроса.
> - **Действие** фильтр, который служит оболочкой для выполнения метода действия. Этот фильтр может выполнять дополнительную обработку, например предоставлять дополнительные данные методу действий, инспектировать возвращаемое значение или отменять выполнение метода действия
> - **Результат** фильтра, который служит оболочкой для выполнения объекта ActionResult. Этот фильтр может выполнять дополнительную обработку результата, например изменить HTTP-ответа.
> - **Исключение** фильтр, который выполняется при наличии необработанного исключения в методе действия, начиная с фильтрами авторизации и заканчивая выполнения результата. Фильтры исключений можно использовать для ведения журнала или отображения страниц об ошибках.
> 
> Дополнительные сведения о поставщиках фильтров см. на этой ссылке MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>О возможности ведения журнала в MVC-приложении Music Store

Это решение Music Store имеет новую таблицу модели данных для ведения журнала узла, **ActionLog**, со следующими полями: имя контроллера, получивший запрос, вызывается действие, IP-адрес клиента и отметку времени.

![Модель данных. Таблица ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Модели данных. Таблица ActionLog.")

*Модель данных — ActionLog таблицы*

Решение обеспечивает представление MVC ASP.NET в журнал действий, который можно найти в **MvcMusicStores/представления/ActionLog**:

![Представления журнала действий](aspnet-mvc-4-custom-action-filters/_static/image2.png "представление журнала действий")

*Представления журнала действий*

Это структура вся работа будет фокусироваться на прерывание запроса контроллера и выполнять запись в журнал, используя специальную фильтрацию.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Задача 1 - Создание пользовательского фильтра для перехвата запроса контроллера

В этой задаче вы создадите класс атрибут пользовательского фильтра, который будет содержать логику занесения данных. Для этой цели вы расширите возможности ASP.NET MVC **ActionFilterAttribute** класса и реализовать этот интерфейс **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** является базовым классом для всех атрибутов фильтров. Он предоставляет следующие методы для выполнения определенной логики после и перед выполнением действия контроллера:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): непосредственно перед действие вызывается метод.
> - **OnActionExecuted**(ActionExecutedContext filterContext): после вызова метода действия и до выполнения результата (до отображения представления).
> - **OnResultExecuting**(ResultExecutingContext filterContext): только в том случае, результат которого выполняется (до отображения представления).
> - **OnResultExecuted**(ResultExecutedContext filterContext): после выполнения результата (после визуализации представления).
> 
> Путем переопределения любого из этих методов в производном классе, можно выполнить собственный код фильтрации.


1. Откройте **начать** решений, расположенный **\Source\Ex01-LoggingActions\Begin** папки.

   1. Необходимо будет загрузить некоторые отсутствующие пакеты NuGet, прежде чем продолжить. Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.
   2. В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.
   3. Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.

      > [!NOTE]
      > Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта. С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта. Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.
      > 
      > Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Добавление нового класса C# в **фильтры** папки и назовите его *CustomActionFilter.cs*. Эта папка будет хранить все настраиваемые фильтры.
3. Откройте **CustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространств имен:

    (Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Наследовать **CustomActionFilter** класса из **ActionFilterAttribute** и внесите **CustomActionFilter** Реализуйте класс **IActionFilter** интерфейс.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Сделать **CustomActionFilter** класс переопределить метод **OnActionExecuting** и добавить необходимую логику в журнал выполнения фильтра. Чтобы сделать это, добавьте следующий выделенный код в **CustomActionFilter** класса.

    (Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - сервера Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** использует метод **Entity Framework** для добавления нового ActionLog регистра. Он создает и заполняет контекстную информацию из нового экземпляра сущности **filterContext**.
    > 
    > Дополнительные сведения о **параметром ControllerContext** класса в [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Задача 2 - Включение перехватчик код в класс контроллера Store

В этой задаче вы добавите пользовательский фильтр, внедряя его для всех классов контроллеров и действий контроллера, которые регистрируются. В целях практики класс контроллера Store будет иметь журнала.

Метод **OnActionExecuting** из **ActionLogFilterAttribute** пользовательского фильтра выполняется при вызове вставлен элемент.

Можно также перехватывать метод конкретному контроллеру.

1. Откройте **StoreController** в **MvcMusicStore\Controllers** и добавьте ссылку на **фильтры** пространство имен:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Внедрить пользовательский фильтр **CustomActionFilter** в **StoreController** класса путем добавления **[CustomActionFilter]** атрибут перед объявлением класса.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Когда фильтр внедряется в класс контроллера, также добавляются все его действия. Если вы хотите применить фильтр только для набора действий, пришлось бы внедрить **[CustomActionFilter]** для каждой из них:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Задача 3 - запуск приложения

В этой задаче вы проверите, что работает фильтр записи в журнал. Вы запустите приложение и посетить магазин, и затем производится проверка журнала действий.

1. Нажмите клавишу **F5** для запуска приложения.
2. Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе:

    ![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image3.png "tracker состояние перед действием страницы журнала")

    *Состояние регистрации журнала до действия "страницы"*

   > [!NOTE]
   > По умолчанию он всегда будет отображаться один элемент, который создается при получении существующих жанров для меню.
   > 
   > В целях упрощения мы удаляем **ActionLog** таблицы при каждом запуске приложения, он отображает только журналы проверки каждой конкретной задачи.
   > 
   > Может потребоваться удалить следующий код из **сеанса\_запустить** метод (в **Global.asax** класс), чтобы сохранить журнал для всех действий, выполненных в Store Контроллер.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.
4. Перейдите к **/ActionLog** и если журнал пустой press **F5** для обновления страницы. Проверьте, что были отслежены посещенных:

    ![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image4.png "журнал действий с действием в журнал")

    *Журнал действий с действием в журнал*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Упражнение 2: Управление нескольких фильтров

В этом упражнении будет добавить второй настраиваемый фильтр действий в класс StoreController и определить определенном порядке, в котором будет выполняться оба фильтра. Затем вы обновите код, чтобы зарегистрировать глобальный фильтр.

Существуют различные параметры, которые необходимо учитывать при определении порядка выполнения фильтров. Например свойство Order и область фильтры:

Можно определить **область** для каждого из фильтров, например, можно задать область все фильтры действий для выполнения в рамках **область контроллера**и все фильтры авторизации для запуска в **глобальной области видимости** . Области имеют порядок определения выполнение.

Кроме того для каждого фильтра действия есть свойство Order, который используется для определения порядка выполнения в области фильтра.

Дополнительные сведения о порядке выполнения пользовательских фильтров действий см. на этой статье MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Упражнение 1: Создание нового настраиваемого фильтра действий

В этой задаче вы создадите новый настраиваемый фильтр действий для добавления в класс StoreController научиться управлять порядок выполнения фильтров.

1. Откройте **начать** решений, расположенный **\Source\Ex02-ManagingMultipleActionFilters\Begin** папки. В противном случае можно продолжить использование **окончания** решение получен путем выполнения предыдущего упражнения.

    1. Если вы открыли предоставленный **начать** решение, необходимо будет загрузить некоторые отсутствующие пакеты NuGet прежде чем продолжить. Чтобы сделать это, нажмите кнопку **проекта** меню и выберите **управление пакетами NuGet**.
    2. В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.
    3. Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.

        > [!NOTE]
        > Одним из преимуществ использования NuGet является отсутствие поставлять все библиотеки в проекте, уменьшив размер проекта. С помощью NuGet Power Tools путем указания версий пакета в файле Packages.config можно для скачивания всех необходимых библиотек при первом запуске проекта. Вот почему необходимо выполните описанные выше действия, после открытия существующего решения из этой лаборатории.
        > 
        > Дополнительные сведения см. в этой статье: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Добавление нового класса C# в **фильтры** папки и назовите его *MyNewCustomActionFilter.cs*
3. Откройте **MyNewCustomActionFilter.cs** и добавьте ссылку на **System.Web.Mvc** и **MvcMusicStore.Models** пространство имен:

    (Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Замените объявление класса по умолчанию следующим кодом.

    (Code Snippet - *ASP.NET MVC 4 настраиваемых фильтров действий - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Этот настраиваемый фильтр действий практически не отличается от той, которую вы создали в предыдущем упражнении. Основное различие заключается в наличии *&quot;войти по&quot;* атрибута обновляется с именем этого нового класса, чтобы определить фильтр, который зарегистрирован в журнале.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Задача 2: Включение новый перехватчик код в класс StoreController

В этой задаче будет добавить новый пользовательский фильтр в класс StoreController и запустить решение, чтобы убедиться в том, как оба фильтра совместной работы.

1. Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и внедрять новый настраиваемый фильтр **MyNewCustomActionFilter** в  **StoreController** показан класс, как в следующем коде.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Теперь запустите приложение, чтобы показать, как работают эти две настраиваемые фильтры действий. Чтобы сделать это, нажмите клавишу **F5** и подождите, пока при запуске приложения.
3. Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе.

    ![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image5.png "tracker состояние перед действием страницы журнала")

    *Состояние регистрации журнала до действия "страницы"*
4. Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.
5. Убедитесь, что это время; посещенных отслеживались дважды: после добавления в пользовательские фильтры действий во всех **StorageController** класса.

    ![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image6.png "журнал действий с действием в журнал")

    *Журнал действий с действием в журнал*
6. Закройте браузер.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Задача 3: Управление упорядочение фильтра

В этой задаче вы узнаете, как управлять порядок выполнения фильтров с помощью свойства заказа.

1. Откройте **StoreController** класса, расположенный **MvcMusicStore\Controllers** и укажите **порядок** свойство в обоих фильтров, такие как показано ниже.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Убедитесь том, как фильтры выполняются в зависимости от значения свойства Order. Вы увидите, что фильтр с наименьшим значением порядка (**CustomActionFilter**) является тот, который выполняется. Нажмите клавишу **F5** и подождите, пока при запуске приложения.
3. Перейдите к **/ActionLog** в начальное состояние представления журнала см. в разделе.

    ![Состояние регистрации перед действием страницы журнала](aspnet-mvc-4-custom-action-filters/_static/image7.png "tracker состояние перед действием страницы журнала")

    *Состояние регистрации журнала до действия "страницы"*
4. Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.
5. Проверьте, что на этот раз посещенных отслеживались упорядоченные по фильтрации порядковый номер: **CustomActionFilter** журналы первого.

    ![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image8.png "журнал действий с действием в журнал")

    *Журнал действий с действием в журнал*
6. Теперь обновления значение порядка фильтров и проверьте, как изменяется порядок ведения журнала. В **StoreController** класса, обновите значение порядка фильтров, как показано ниже.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Снова запустите приложение, нажав клавишу **F5**.
8. Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.
9. Проверьте, что на этот раз журналов, созданных **MyNewCustomActionFilter** фильтра отображается первым.

    ![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image9.png "журнал действий с действием в журнал")

    *Журнал действий с действием в журнал*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Задача 4: Регистрация фильтрует глобально

В этой задаче вы обновите решение, чтобы зарегистрировать новый фильтр (**MyNewCustomActionFilter**) как глобальный фильтр. Таким образом, он будет запускаться все perfomed действия в приложении, а не только StoreController из них как в предыдущей задаче.

1. В **StoreController** класса, удалите **[MyNewCustomActionFilter]** атрибутов и свойств заказа из **[CustomActionFilter]**. Он должен выглядеть следующим образом:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Откройте **Global.asax** файл и найдите **приложения\_запустить** метод. Обратите внимание на то, что при каждом запуске приложения оно регистрация глобальных фильтров путем вызова **RegisterGlobalFilters** метода в **FilterConfig** класса.

    ![Регистрация глобальных фильтров в файле Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "регистрация глобальных фильтров в файле Global.asax")

    *Регистрация глобальных фильтров в файле Global.asax*
3. Откройте **FilterConfig.cs** файл включен в **приложения\_запустить** папки.
4. Добавьте ссылку на использование System.Web.Mvc; с помощью MvcMusicStore.Filters; пространство имен.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Обновление **RegisterGlobalFilters** метод добавления пользовательского фильтра. Чтобы сделать это, добавьте выделенный код:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Запустите приложение, нажав клавишу **F5**.
7. Щелкните одну из **жанров** в меню и выполнения ряда действий, таких как просмотр доступных альбома.
8. Проверьте, что теперь **[MyNewCustomActionFilter]** является, введенного в HomeController и ActionLogController слишком.

    ![Журнал действий с действием в журнал](aspnet-mvc-4-custom-action-filters/_static/image11.png "журнал действий с действием в журнал")

    *Журнал действий с помощью глобального действия в журнал*

> [!NOTE]
> Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложении B: публикации приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Сводка

Выполнив этот практический семинар вы узнали, как расширить фильтр действий для выполнения пользовательских действий. Вы также узнали, как внедрить любой фильтр к контроллерам страницы. Использовались следующие понятия:

- Создание фильтров настраиваемое действие в классе ASP.NET MVC ActionFilterAttribute
- Как внедрить фильтры в контроллерах ASP.NET MVC
- Как управлять фильтра упорядочение с помощью свойства Order
- Как зарегистрировать фильтры глобально

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Приложение а. Установка Visual Studio Express 2012 для Web

Вы можете установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию с помощью **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Приведенные ниже инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.

1. Перейдите к [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; <em>Visual Studio Express 2012 для Web с пакетом Windows Azure SDK</em>&quot;.
2. Щелкните **установить сейчас**. Если у вас нет **установщика веб-платформы** вы будете перенаправлены к сначала загрузить и установить его.
3. Один раз **установщика веб-платформы** открыт, нажмите кнопку **установить** для запуска программы установки.

    ![Установка Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "установка Visual Studio Express")

    *Установка Visual Studio Express*
4. Прочтите лицензии и условия все продукты и нажмите кнопку **я принимаю** для продолжения.

    ![Принятие условий лицензии](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Принятие условий лицензии*
5. Подождите, пока не завершится процесс загрузки и установки.

    ![Ход установки](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Ход выполнения установки*
6. После завершения установки нажмите кнопку **Готово**.

    ![Установка завершена](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Установка завершена*
7. Нажмите кнопку **выхода** закрыть установщик веб-платформы.
8. Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и Займитесь написанием &quot; **VS Express**&quot;, нажмите кнопку на **VS Express для Web** Плитка.

    ![VS Express для Web плитки](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express для Web плитки*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания

В этом приложении показано, как создать новый веб-сайт на портале управления Windows Azure и опубликовать приложение, полученный после лаборатории, используя преимущества компонентов публикации веб-развертывания, предоставляемые Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Задача 1 - Создание нового веб-сайта с Windows Azure Portal

1. Перейдите к [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.

    > [!NOTE]
    > С помощью Windows Azure можно бесплатное размещение 10 веб-сайтов ASP.NET и затем масштабировать по мере увеличения объема трафика. Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).

    ![Войдите на портал Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Войдите на портал Windows Azure")

    *Войдите на портал управления Windows Azure*
2. Нажмите кнопку **New** на панели команд.

    ![Создание нового веб-сайта](aspnet-mvc-4-custom-action-filters/_static/image18.png "создания нового веб-сайта")

    *Создание нового веб-сайта*
3. Нажмите кнопку **вычислений** | **веб-сайт**. Затем выберите **быстрое создание** параметр. Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **создать веб-сайт**.

    > [!NOTE]
    > Веб-сайта Windows Azure является узлом для веб-приложение, работающее в облаке, вы можете управлять. Быстрое создание позволяет развернуть завершенное веб-приложения для Windows Azure веб-сайта из вне портала. Он не включает действия по настройке базы данных.

    ![Создание нового веб-сайта, с помощью быстрого создания](aspnet-mvc-4-custom-action-filters/_static/image19.png "создания нового веб-сайта, с помощью быстрого создания")

    *Создание нового веб-сайта, с помощью быстрого создания*
4. Подождите, пока новый **веб-сайт** создается.
5. После создания веб-сайт, щелкните ссылку под **URL-адрес** столбца. Проверьте, что новый веб-сайт работает.

    ![Выбрав новый веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image20.png "просмотра на новый веб-сайт")

    *Выбрав новый веб-сайт*

    ![Веб-сайт работал](aspnet-mvc-4-custom-action-filters/_static/image21.png "веб-узлом")

    *Веб-сайт под управлением*
6. Вернитесь на портал и щелкните имя веб-сайт в **имя** столбец для отображения страницы управления.

    ![Открытие страницы управления веб-сайт](aspnet-mvc-4-custom-action-filters/_static/image22.png "Открытие страницы управления веб-сайта")

    *Открытие страницы управления веб-сайта*
7. В **панели мониторинга** раздела **быстрый обзор** щелкните **загрузить профиль публикации** ссылку.

    > [!NOTE]
    > *Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайт Windows Azure для каждого разрешенного метода публикации. Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверку подлинности в каждой из конечных точек, для которых включена метода публикации. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки из этих программ для Публикация веб-приложений на веб-сайтах Windows Azure.

    ![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-custom-action-filters/_static/image23.png "профиль публикации веб-сайта загрузки")

    *Профиль публикации веб-сайта загрузки*
8. Скачайте файл профиля публикации в известном месте. Далее в этом упражнении показано, как использовать этот файл для публикации веб-приложения на веб-сайтах, Windows Azure из Visual Studio.

    ![Сохранение файла профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image24.png "Сохранение профиля публикации")

    *Сохранение файла профиля публикации*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Задача 2 - Настройка сервера базы данных

Если приложение использует SQL Server, баз данных, необходимо будет создать сервер базы данных SQL. Эту задачу может пропустить, если вы хотите развернуть простое приложение, которое не использует SQL Server.

1. Вам потребуется сервер базы данных SQL для хранения базы данных приложения. Серверы баз данных SQL можно просмотреть в своей подписке на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**. Если у вас на сервер, созданный, можно создать его, используя **добавить** кнопки на панели команд. Запишите **имя сервера и URL-адрес, имя входа администратора и пароль**, как их использование в следующие задачи. Не создавайте базы данных, так как он будет создан в более позднем этапе.

    ![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "панель мониторинга базы данных SQL Server")

    *Панель мониторинга базы данных SQL Server*
2. В следующей задаче вы проверите подключения к базе данных из Visual Studio по этой причине необходимо включить IP-адрес локального сервера списка **разрешенные IP-адреса**. Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) кнопки.

    ![Добавление IP-адрес клиента](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Добавление IP-адрес клиента*
3. Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.

    ![Подтверждение изменений](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Подтверждение изменений*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Задача 3 - публикация приложения ASP.NET MVC 4 с помощью веб-развертывания

1. Вернитесь к решению для ASP.NET MVC 4. В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.

    ![Публикация приложения](aspnet-mvc-4-custom-action-filters/_static/image29.png "публикации приложения")

    *Публикация веб-сайта*
2. Импортируйте профиль публикации, сохраненный в первой задаче.

    ![Импорт профиля публикации](aspnet-mvc-4-custom-action-filters/_static/image30.png "Импорт профиля публикации")

    *Импорт профиля публикации*
3. Нажмите кнопку **Проверка подключения**. После завершения проверки нажмите кнопку **Далее**.

    > [!NOTE]
    > Проверка будет завершена, как только вы увидите зеленый флажок отображаться рядом с кнопкой проверить подключение.

    ![Проверка подключения](aspnet-mvc-4-custom-action-filters/_static/image31.png "Проверка подключения")

    *Проверка подключения*
4. В **параметры** раздела **баз данных** нажмите кнопку рядом с текстовое поле подключения к базе данных (т. е. **DefaultConnection**).

    ![Конфигурация веб-развертывания](aspnet-mvc-4-custom-action-filters/_static/image32.png "конфигурация веб-развертывания")

    *Конфигурация веб-развертывания*
5. Настройте подключение к базе данных следующим образом:

   - В **имя_сервера** введите URL-адреса серверу базы данных SQL с помощью *tcp:* префикс.
   - В **имя пользователя** введите имя входа администратора сервера.
   - В **пароль** введите пароль входа администратора сервера.
   - Введите новое имя базы данных.

     ![Настройка строки подключения назначения](aspnet-mvc-4-custom-action-filters/_static/image33.png "Настройка строка соединения с назначением")

     *Настройка строки подключения назначения*
6. Затем нажмите кнопку **ОК**. При появлении запроса на создание базы данных нажмите кнопку **Да**.

    ![Создание базы данных](aspnet-mvc-4-custom-action-filters/_static/image34.png "Создание строки базы данных")

    *Создание базы данных*
7. В текстовое поле подключение по умолчанию отображается строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure. Затем нажмите кнопку **Далее**.

    ![Строка подключения, указывающий на базу данных SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "строку подключения, указывающий на базу данных SQL")

    *Строка подключения, указывающий на базу данных SQL*
8. В **предварительной версии** щелкните **публикации**.

    ![Публикация веб-приложения](aspnet-mvc-4-custom-action-filters/_static/image36.png "публикации веб-приложения")

    *Публикация веб-приложения*
9. После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-сайта.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Приложение c. фрагменты кода

С помощью фрагментов кода у вас есть весь код, который требуется в вашем распоряжении. Лаборатории документ поможет определить точно при их использовании, как показано на рисунке ниже.

![Фрагменты кода Visual Studio, чтобы вставить код в проект](aspnet-mvc-4-custom-action-filters/_static/image37.png "фрагменты кода с помощью Visual Studio, чтобы вставить код в проект")

*Фрагменты кода Visual Studio, чтобы вставить код в проект*

***Чтобы добавить фрагмент кода, с помощью клавиатуры (только C#)***

1. Поместите курсор в место вставки кода.
2. Начните вводить имя фрагмента (без пробелов и дефисов).
3. Посмотрите, как IntelliSense отображает, совпадающие с именами фрагменты.
4. Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).
5. Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.

![Начните вводить имя фрагмента](aspnet-mvc-4-custom-action-filters/_static/image38.png "начните вводить имя фрагмента")

*Начните вводить имя фрагмента*

![Нажмите клавишу Tab, чтобы выделить фрагмент в выделенный](aspnet-mvc-4-custom-action-filters/_static/image39.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")

*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*

![Снова нажмите клавишу Tab и фрагмент будет расширяться](aspnet-mvc-4-custom-action-filters/_static/image40.png "еще раз нажмите клавишу Tab и фрагмент будет расширяться")

*Снова нажмите клавишу Tab и фрагмент будет расширяться*

***Чтобы добавить фрагмент кода, с помощью мыши (C#, Visual Basic и XML)*** 1. Щелкните правой кнопкой мыши место для вставки фрагмента кода.

1. Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.
2. Выберите соответствующий фрагмент из списка, щелкнув его.

![Щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент](aspnet-mvc-4-custom-action-filters/_static/image41.png "щелкните правой кнопкой мыши, где необходимо вставить фрагмент кода и выберите Вставить фрагмент")

*Щелкните правой кнопкой мыши место для вставки фрагмента кода и выберите Вставить фрагмент*

![Выберите соответствующий фрагмент из списка, щелкнув по ней](aspnet-mvc-4-custom-action-filters/_static/image42.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")

*Выберите соответствующий фрагмент из списка, щелкнув по ней*
