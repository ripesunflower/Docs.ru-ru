---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Многократное использование пользовательского интерфейса с помощью главных страниц и частичных представлений | Документация Майкрософт
author: microsoft
description: Шаг 7 выглядит как "не повторяйся"принцип можно применить в наши шаблоны представлений, чтобы исключить дублирование кода, с помощью шаблонов частичного представления и главные страницы.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839598"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Многократное использование пользовательского интерфейса с помощью главных страниц и частичных представлений
====================
по [Microsoft](https://github.com/microsoft)

[Загрузить PDF-файл](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Это 7 из бесплатной [руководство по использованию приложения «NerdDinner»](introducing-the-nerddinner-tutorial.md) , пошаговое рассмотрение как создать небольшой, но завершить, веб-приложения с помощью ASP.NET MVC 1.
> 
> Шаг 7 рассматривает способами, мы можете использование принципа «"не повторяйся"» в наши шаблоны представлений, чтобы исключить дублирование кода, с помощью шаблонов частичного представления и главных страниц.
> 
> Если вы используете ASP.NET MVC 3, рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner шаг 7: Частичные и главные страницы

Одним из принципов проектирования, которые охватывает ASP.NET MVC — «Сделать не Repeat Yourself» принцип (которые называют «DRY»). "Не повторяйся" разработки помогает исключить дублирование кода и логику, которая в конечном счете делает приложения для построения быстрее и проще было обслуживать.

Мы уже видели принципа "не повторяйся" применяется в нескольких NerdDinner сценариев. Несколько примеров: Наша логика проверки реализуется в наш слой модели, который включает его для всех режимах редактирования и создания сценариев в своем контроллере; повторно используется шаблон представления «NotFound» через методы действий изменения "," Сведения "и" Delete; Мы используем шаблон именования соглашение - с нашей шаблоны представления, которые устраняет необходимость явно задавать имя при вызове вспомогательного метода View(); и мы повторно при использовании класса DinnerFormViewModel для режимах редактирования и создания действий сценариев.

Теперь взглянем на способы «Повторяйся» можно применить в наши шаблоны представления также исключить дублирование кода, существует.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Повторно посетите наши изменения и создавать шаблоны представлений

В настоящее время мы используем два шаблона другое представление — «Edit.aspx» и «Create.aspx» — для отображения нашей компании Dinner формы пользовательского интерфейса. Быстрое сравнение visual их выделяет, насколько они похожи. Ниже приведен, как выглядит Создание формы.

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

И вот как выглядит нашу форму Edit (изменить):

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Не так влияет существует? Отличное от текста заголовка и заголовок элементы управления формы макет и входные данные идентичны.

При открытии «Edit.aspx» и «Create.aspx» шаблоны представлений, которые очень скоро найдется, которые они содержат идентичные формы макета и входных данных код элемента управления. Это дублирование означает, что нам приходится вносить изменения в два раза каждый раз, когда мы ввести или изменить новое свойство Dinner — он не подходит.

### <a name="using-partial-view-templates"></a>С помощью шаблонов частичного представления

ASP.NET MVC поддерживает возможность определения «частичное представление» шаблоны, которые могут использоваться для инкапсуляции логики отрисовки представления для вложенных часть страницы. «Частичных представлений» — это удобное средство для определения логики отрисовки представления один раз и затем использовать его повторно в нескольких местах внутри приложения.

Чтобы помочь «ПРОБНОГО доступа» нашей Edit.aspx и дублирование шаблона представления Create.aspx, мы можно создать шаблон частичное представление с именем «DinnerForm.ascx», инкапсулирующий макета формы и общим для обоих входных элементов. Мы сделаем это, щелкнув правой кнопкой мыши на каталог ужинов/Views/и выбрав «Add -&gt;представление» команды меню:

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Откроется диалоговое окно «Добавление представления». Мы будем называть новое представление, мы хотите создавать «DinnerForm», установите флажок «Создать частичное представление» в диалоговом окне и указать, что мы будет передавать класс DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

При нажатии кнопки «Добавить», Visual Studio создаст новый шаблон «DinnerForm.ascx» представление для нас в каталоге «\Views\Dinners».

Мы можно затем скопировать и вставить в макет формы повторяющиеся / ввода кода элемента управления на основе наших шаблонов представления Edit.aspx/ Create.aspx в наш новый шаблон «DinnerForm.ascx» частичного представления:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Затем можно обновить наш Edit and Create шаблоны представлений для вызова частичного шаблона DinnerForm и исключить дублирование формы. Это можно сделать, вызывающий Html.RenderPartial("DinnerForm") в наши шаблоны представлений:

##### <a name="createaspx"></a>CREATE.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Можно явно указать полный путь при вызове Html.RenderPartial частичного шаблона (например: ~ Views/Dinners/DinnerForm.ascx»). В коде выше Однако мы преимуществами соглашения об именовании модель на основе в ASP.NET MVC и указав «DinnerForm» как имя разделяемого для подготовки к просмотру. При этом ASP.NET MVC будет искать сначала в каталоге представлений на основе соглашений (это было бы/Views/ужинов DinnersController). Если она не находит частичного шаблона существует он будет искать его в каталоге /Views/Shared.

При вызове Html.RenderPartial() просто имя частичного представления, ASP.NET MVC передает частичного представления же модель и ViewData словарь объектов, используемых вызывающего шаблона представления. Кроме того существуют перегруженные версии Html.RenderPartial(), позволяющие передавать альтернативный объект модели и/или словарь ViewData для частичного представления для использования. Это полезно в сценариях, где требуется передать подмножество полной модели/ViewModel.

| **На стороне Тема: Почему &lt;%%&gt; вместо &lt;% = %&gt;?** |
| --- |
| Среди прочего слабая, вы могли заметить в приведенном выше коде — мы используем &lt;%%&gt; блокировать вместо &lt;% = %&gt; блокировать при вызове Html.RenderPartial(). &lt;% = %&gt; блоки в ASP.NET указывают, что разработчику для подготовки к просмотру указанное значение (например: &lt;% = «Hello» %&gt; умеет отрисовывать «Hello»). &lt;%%&gt; блоки вместо этого указать, что разработчику для выполнения кода, и что любой созданный выходные данные в них должно делаться в явном виде (например: &lt;% Response.Write("Hello") %&gt;. Причина, мы используем &lt;%%&gt; блока с кодом Html.RenderPartial выше обусловлено Html.RenderPartial() метод не возвращает строки, а вместо этого выводит поток выходных данных содержимое непосредственно для вызова шаблона представления. Это делается для повышения эффективности производительности и в результате он исключает необходимость создания объекта (потенциально даже очень большими) временной строки. Это позволяет сократить использование памяти и повышает пропускную способность приложения в целом. Распространенной ошибкой при использовании Html.RenderPartial() забудьте добавить точку с запятой в конце вызова, когда он находится в пределах &lt;%%&gt; блока. Например, этот код приведет к ошибке компилятора: &lt;% Html.RenderPartial("DinnerForm") %&gt; вместо этого необходимо написать: &lt;% Html.RenderPartial("DinnerForm"); %&gt; это обусловлено &lt;%%&gt; блоки представляют собой инструкции автономный код и при использовании кода C#, операторы должны заканчиваться точкой с запятой. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Уточнить код с помощью шаблонов частичного представления

Мы создали шаблон частичного представления «DinnerForm» во избежание дублирования логики визуализации представления в нескольких местах. Это самая распространенная причина для создания шаблонов частичного представления.

Иногда по-прежнему имеет смысл для создания частичных представлений, даже в том случае, если они вызываются только в одном месте. Шаблоны очень сложные представления зачастую становится гораздо легче читается, когда их логики отрисовки представления при извлечении и секционирована на один или более хорошо с именем частичная шаблонов.

Например, рассмотрим ниже фрагмент кода из файла Site.master в нашем проекте (который мы заинтересованы в ближайшее время). Код является довольно тривиальной задачей для чтения — отчасти потому, что логика для отображения имени входа/выхода ссылку в верхней правой части экрана он был инкапсулирован в разделяемом «LogOnUserControl»:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Всякий раз, когда вы обнаружите начало путать попытке понять разметку html и код внутри шаблона представления, рекомендуется ли было бы легче, если часть была извлечена и подвергнуты рефакторингу в частичных представлений, иметь понятные имена.

### <a name="master-pages"></a>Главные страницы

В дополнение к поддержке частичных представлений, ASP.NET MVC также поддерживает возможность создавать шаблоны «главную страницу», которые могут использоваться для определения макета страницы и html верхнего уровня сайта. Содержимое заполнитель, который затем можно добавить элементы управления на главную страницу для определения заменяемых областей, которые можно переопределить или «заполнено» представления. Это позволяет очень эффективный (и "не повторяйся") для применения общий макет приложения.

По умолчанию новые проекты ASP.NET MVC имеют шаблон главной страницы, автоматически добавляется к ним. Эта главная страница называется «Site.master». он размещен в папке \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Файл Site.master по умолчанию соответствует приведенному ниже. Он определяет внешний HTML-код сайта, а также меню для навигации в верхней. Этот пакет содержит два элемента управления можно заменить заполнителя контента — один для заголовка, а другой — для которых должны быть заменены основного содержимого страницы:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Все шаблоны представлений, созданных для нашего приложения NerdDinner («List», «Сведения», «Изменить», «Создать», «Не найдено» т. д) в основе этого шаблона Site.master. Это обозначается с помощью атрибута «MasterPageFile», который был добавлен по умолчанию в верхней части &lt;% @ Page %&gt; директива, когда мы создавали наши представления, с помощью диалогового окна «Добавление представления»:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Это означает, что мы можем изменить содержимое Site.master, и иметь изменения автоматически применять и используется, когда мы визуализируем любой из наших шаблонов представлений.

Давайте обновим наши Site.master верхний колонтитул, таким образом, чтобы заголовок нашего приложения «NerdDinner» вместо «Мое приложение MVC». Также обновим наши меню навигации, чтобы первая вкладка — «Найти Dinner» (обрабатывается методом действия Index() HomeController) и добавим новую вкладку, которая называется «Узла Dinner» (обрабатывается методом действия Create() DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Если сохранить файл Site.master и обновить наш обозреватель, мы увидим заголовка изменения show до ко всем представлениям в наше приложение. Пример:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

И с */Dinners/Edit / [id]* URL-адрес:

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Следующий шаг

Частичные и главные страницы предоставляют очень гибкие средства, которые позволяют аккуратно упорядочивать представления. Вы найдете они помогают избежать дублирования представления содержимого / кода и упростить чтение и обслуживание шаблоны представления.

Давайте теперь вернемся к созданному ранее сценарию листинг и масштабируемой поддержки разбиения по страницам.

> [!div class="step-by-step"]
> [Назад](use-viewdata-and-implement-viewmodel-classes.md)
> [Вперед](implement-efficient-data-paging.md)
