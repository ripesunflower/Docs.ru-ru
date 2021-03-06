---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: 'Итерация #2 — Создание приложения поиска неплохо (C#) | Документация Майкрософт'
author: microsoft
description: В этой итерации мы улучшить внешний вид приложения, изменив значение по умолчанию master страница представления ASP.NET MVC и каскадные таблицы стилей.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: 7385756d6ae362f8e39d56bb3f0e68caa89329b2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826343"
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Итерация #2 — Создание приложения поиска неплохо (C#)
====================
по [Microsoft](https://github.com/microsoft)

[Скачать код](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> В этой итерации мы улучшить внешний вид приложения, изменив значение по умолчанию master страница представления ASP.NET MVC и каскадные таблицы стилей.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Создание приложения управления контактами ASP.NET MVC (C#)
  

В этой серии руководств мы создаем всего приложения управления контактами от начала до конца. Приложение диспетчера контактов позволяет хранить контактные данные - имена, номера телефонов и адреса электронной почты — список людей.

Мы создаем приложение через несколько итераций. С каждой итерацией мы постепенно улучшить приложение. Этот подход с несколькими итерации предназначена для того, чтобы можно было понять причину для каждого изменения.

- Итерация #1 - Создание приложения. В первой итерации мы создадим Contact Manager простейшим способом невозможно. Мы добавили поддержку для основных операций базы данных: создание, чтение, обновление и удаление (CRUD).

- #2 - итерация приложения красиво выглядеть. В этой итерации мы улучшить внешний вид приложения, изменив значение по умолчанию master страница представления ASP.NET MVC и каскадные таблицы стилей.

- Итерации #3 - Добавление проверки форм. В третьей итерации мы добавим проверки базовой форме. Мы запретить Отправка формы без завершения обязательные поля. Кроме того, мы проверяем, адреса электронной почты и номера телефонов.

- Итерация #4 - Создание слабых связей в приложении. В этот третий шаг цикла мы воспользоваться преимуществами нескольких шаблонов дизайна программного обеспечения, чтобы упростить обслуживании и изменении приложения диспетчера контактов. Например мы выполнили рефакторинг наше приложение, чтобы использовать шаблон репозитория и шаблон внедрения зависимостей.

- Итерации #5 - Создание модульных тестов. В пятой итерации мы вам наше приложение проще обслуживании и изменении путем добавления модульных тестов. Мы макетирование наших классов модели данных и создания модульных тестов для контроллеров и логику проверки.

- Итерация #6 - использование управляемой тестами разработки. В этой шестой итерации мы добавляем новые функциональные возможности в наше приложение, сначала написание модульных тестов и написании кода в отношении модульные тесты. В этой итерации мы добавляем групп контактов.

- Итерации #7 - Добавление функций Ajax. В седьмой итерации мы повысить скорость реагирования и производительность приложения, добавляя поддержку Ajax.

## <a name="this-iteration"></a>Эта итерация

Цель этой итерации — улучшить внешний вид приложения диспетчера контактов. В настоящее время Contact Manager использует по умолчанию ASP.NET MVC представления главной страницы и таблицы каскадных стилей (см. рис. 1). Эти Дон t выглядеть неправильный, но нигде t want Contact Manager для поиска так же, как каждый другой ASP.NET MVC веб-сайт. Я хочу заменить эти файлы пользовательских файлов.


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Рис 01**: внешний вид приложения ASP.NET MVC по умолчанию ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


В этой итерации обсуждаются два подхода к улучшает Графический дизайн нашего приложения. Во-первых мне Показать, как использовать преимущества ASP.NET MVC макетов для загрузки бесплатной шаблона проектирования ASP.NET MVC. ASP.NET MVC макетов позволяет создавать профессиональные веб-приложения без выполнения действий.

Я решил не использовать шаблон из коллекции проектов MVC для ASP.NET для приложения диспетчера контактов. Вместо этого мне нужно было пользовательским дизайном, созданные фирма профессионального дизайна. Во второй части этого учебника я объясню, как я работал с компанией профессионального дизайна для создания в окончательном проекте ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>ASP.NET MVC макетов

Макетов ASP.NET MVC — это бесплатный ресурс, предоставляемые корпорацией Майкрософт. MVC ASP.NET она расположена по следующему адресу:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

ASP.NET MVC макетов размещает коллекцию схем бесплатный веб-сайт, которые были созданы специально для использования в проекте ASP.NET MVC. Макеты, передаются по членами сообщества. Посетителям коллекции смогут проголосовать за свои избранные модели (см. рис. 2).


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Рис. 02**: The ASP.NET MVC макетов ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


Пока я пишу этот учебник, наиболее популярных разработки в коллекции — это Дизайн, с именем октября, Дэвид Hauser. Такой подход можно использовать для проекта ASP.NET MVC, выполнив следующие действия:

1. Нажмите кнопку **загрузить** кнопку, чтобы загрузить файл October.zip на компьютер.
2. Щелкните правой кнопкой мыши загруженный файл October.zip и нажмите кнопку **Unblock** кнопку (см. рис. 3).
3. Распакуйте файл в папку с именем октября.
4. Выберите все файлы в папке DesignTemplate, содержащиеся в папке октября, правой кнопкой мыши и выберите пункт меню **копирования**.
5. Щелкните правой кнопкой мыши узел проекта ContactManager в окне обозревателя решений Visual Studio и выберите пункт меню **вставить** (см. рис. 4).
6. Выберите пункт меню в Visual Studio **Правка, найти и заменить, Быстрая замена** и замените *[MyProjectName]* с *ContactManager* (см. рис. 5).


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Рис 03**: разблокирования файла загруженных из Интернета ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Рис. 04**: перезапись файлов в обозревателе решений ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**05 рис**: замена ContactManager [имя_проекта] ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


После выполнения этих действий веб-приложение будет использовать новый дизайн. Страница, на рис. 6 показан внешний вид приложения диспетчера контактов и дизайна октября.


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Рис 06**: ContactManager с шаблоном октября ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Создание проекта пользовательского ASP.NET MVC

ASP.NET MVC макетов имеет хорошего выбора разных структур стили. Коллекции вы найдете безболезненным способ настройки внешнего вида ваших приложений ASP.NET MVC. И, конечно, в коллекции есть существенное преимущество бесплатности полностью.

Тем не менее может потребоваться создать полностью уникального дизайна для веб-сайта. В этом случае имеет смысл работать с веб-сайта компании по проектированию. Я решил применить такой подход по проектированию для приложения диспетчера контактов.

Я ZIP-архив копии диспетчера контактов из итерации 1 и отправляемые компании по проектированию проекта. Они не было Visual Studio (корпорации на них!), но, что t является проблемой. Они смогут бесплатно скачайте Microsoft Visual Web Developer из [ https://www.asp.net ](https://www.asp.net) веб-сайта и откройте приложение диспетчера контактов в Visual Web Developer. За несколько дней они вернуло разработки на рис. 7.


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Рис 07**: ASP.NET MVC Contact Manager конструирования ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


Новый дизайн состоял из двух файлов: новый CSS-файла и новый файл главной страницы представления. Главная страница представления содержит макет и общего содержимого для представления приложения ASP.NET MVC. Например главной страницы представления включает в себя заголовок, вкладки навигации и нижний колонтитул, отображаются на рис. 7. Я переписал существующего представления страницей Site.Master в папке Views\Shared с новым файлом Site.Master из компании по проектированию

Компания разработки, также создается новый таблицы каскадных стилей и набор изображений. Поместить эти новые файлы в папке Content я переписал существующий файл Site.css. В папке содержимого необходимо поместить все статическое содержимое.

Обратите внимание на то, что новый дизайн для Contact Manager включает в себя образы для редактирования и удаления контактов. Изменение и удаление образа отображаются рядом с каждого контакта в HTML-таблицы контактов.

Изначально ссылкам, которые были отображены с HTML-код. Вспомогательный ActionLink() следующим образом:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Метод Html.ActionLink() не поддерживает образы (метод HTML кодирует текст ссылки, по соображениям безопасности). Поэтому я заменены вызовы Html.ActionLink() вызовы Url.Action() следующим образом:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Метод Html.ActionLink() отображает весь HTML-гиперссылка. Метод Url.Action(), с другой стороны, выводит только URL-адрес без &lt;&gt; тега.

Кроме того, обратите внимание, что новый формат содержит выбранном и невыбранном вкладки. Например, на рис. 8 **создать новый контакт** выбрана вкладка и **контактов** не выбрана вкладка.


[![В диалоговом окне нового проекта](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Рис 08**: выбранной или невыбранной вкладок ([Просмотр полноразмерного изображения](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Для поддержки визуализации выбранном и невыбранном вкладки, я создал пользовательский вспомогательный метод HTML, с именем MenuItemHelper. Этот вспомогательный метод отображает либо &lt;li&gt; тега или &lt;класс li = «selected»&gt; тега в зависимости от ли текущий контроллер и действие соответствует имени контроллера и действия, передаваемые вспомогательное приложение. Код для MenuItemHelper содержится в листинге 1.

**В листинге 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

MenuItemHelper использует класс TagBuilder для внутренних целей для построения &lt;li&gt; HTML-тега. Класс TagBuilder является очень полезным служебный класс, который можно использовать всякий раз, когда вам нужно создать новый тег HTML. Он включает методы для добавления атрибутов, добавив классы CSS, генерации идентификаторов и изменение тега s внутренний HTML-код.

## <a name="summary"></a>Сводка

В этой итерации мы улучшили визуальную структуру наше приложение ASP.NET MVC. Во-первых вы ознакомились с ASP.NET MVC макетов. Вы узнали, как скачать бесплатные шаблоны из библиотеки макетов ASP.NET MVC, который можно использовать в приложениях ASP.NET MVC.

Далее мы рассмотрели создание пользовательской системы путем изменения по умолчанию CSS-файла и файла подкачки главному представлению. Чтобы обеспечить поддержку новых методов разработки, нам пришлось внести некоторые незначительные изменения в наше приложение диспетчера контактов. Например мы добавили новый вспомогательный метод HTML, с именем MenuItemHelper, выбранном и невыбранном вкладок.

В следующей итерации мы разберем очень важным предметом проверки. Мы добавьте код проверки в наше приложение, чтобы пользователь не может создать контакт без указания необходимые значения, например человека s сначала имени и фамилии.

> [!div class="step-by-step"]
> [Назад](iteration-1-create-the-application-cs.md)
> [Вперед](iteration-3-add-form-validation-cs.md)
