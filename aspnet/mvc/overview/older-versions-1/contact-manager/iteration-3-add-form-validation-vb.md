---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Итерации #3 – Добавление проверки форм (VB) | Документация Майкрософт'
author: microsoft
description: В третьей итерации мы добавим проверки базовой форме. Мы запретить Отправка формы без завершения обязательные поля. Мы также проверить электронной почт...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: e9099b669fd809d27a06f333345e7d6feb7b800d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824665"
---
<a name="iteration-3--add-form-validation-vb"></a>Итерации #3 – Добавление проверки форм (VB)
====================
по [Microsoft](https://github.com/microsoft)

[Скачать код](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> В третьей итерации мы добавим проверки базовой форме. Мы запретить Отправка формы без завершения обязательные поля. Кроме того, мы проверяем, адреса электронной почты и номера телефонов.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Создание приложения управления контактами ASP.NET MVC (VB)
  

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

В этой второй итерации приложения диспетчера контактов мы добавляем проверки базовой форме. Мы запретить пользователям передавать контакт без ввода значения для обязательных полей формы. Кроме того, мы проверяем, номера телефонов и адреса электронной почты (см. рис. 1).


[![В диалоговом окне нового проекта](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Рис 01**: формы с помощью проверки ([Просмотр полноразмерного изображения](iteration-3-add-form-validation-vb/_static/image2.png))


В этой итерации мы добавим логику проверки непосредственно к действиям контроллера. Как правило это не рекомендуемый способ добавления проверки в приложение ASP.NET MVC. Лучше размещать приложения s логику проверки допустимости в отдельном [слой служб](http://martinfowler.com/eaaCatalog/serviceLayer.html). В следующей итерации мы выполнили рефакторинг приложения диспетчера контактов, чтобы сделать приложение более удобным.

В этой итерации чтобы не усложнять, мы напишем весь код проверки вручную. Вместо того чтобы писать код проверки, сами, мы может воспользоваться преимуществами структуры проверки. Например Microsoft Enterprise Library Проверка приложения блок (VAB) можно использовать для реализации логики проверки для приложения ASP.NET MVC. Дополнительные сведения о Validation Application Block, см. в разделе:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Добавление проверки для представления создания

Позвольте s начните с добавления логики проверки для представления создания. К счастью так как мы формируется Create view с Visual Studio, создать представление уже содержит все логики интерфейса пользователя, необходимые для отображения сообщений проверки. Создать представление содержится в листинге 1.

**В листинге 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Ошибка при проверке поля класса, используемого для стиля выходных данных, подготовленных Html.ValidationMessage() вспомогательной функцией. Ошибка при проверке входных данных класса, используемого для стиля текстового поля (input) к просмотру Html.TextBox() вспомогательной функцией. Сводка ошибки класс используется для стиля неупорядоченный список к просмотру Html.ValidationSummary() вспомогательной функцией.

> [!NOTE] 
> 
> Вы можете изменить классы стилей таблицы стилей, описанные в этом разделе, чтобы настроить внешний вид сообщений об ошибках проверки.


## <a name="adding-validation-logic-to-the-create-action"></a>Добавление логики проверки, чтобы создать действие

Прямо сейчас, представления создания никогда не отображает сообщения об ошибках проверки, так как мы не написали логику для создания сообщения. Для отображения сообщений об ошибках проверки, необходимо добавить сообщения об ошибках для ModelState.

> [!NOTE] 
> 
> Метод UpdateModel() добавляет сообщения об ошибках ModelState автоматически при ошибке, следует указать поля формы к свойству. Например при попытке присвоить свойство BirthDate, принимающее значения даты и времени строка «apple», затем метод UpdateModel() добавляет ошибку ModelState.


Измененный метод Create() в листинге 2 содержит раздел, который проверяет свойства на класс Contact, перед вставкой нового контакта в базе данных.

**В листинге 2 - Controllers\ContactController.vb (создать с помощью проверки)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

В разделе проверки применяет четыре различных условий:

- Свойство «имя» должно иметь ненулевую длину (и он не может состоять только из пробелов)
- Свойство LastName должно иметь ненулевую длину (и он не может состоять только из пробелов)
- Если свойство Phone имеет значение (имеет длину больше 0), а затем свойство Phone должно соответствовать регулярному выражению.
- Если свойство сообщения электронной почты имеет значение (имеет длину больше 0), а затем свойство электронной почты должно соответствовать регулярному выражению.

Если нарушение правила проверки, сообщение об ошибке добавляется с помощью метода AddModelError() ModelState. При добавлении сообщение ModelState, укажите имя свойства и текст сообщений об ошибках проверки. Это сообщение об ошибке отображается в представлении с Html.ValidationSummary() и Html.ValidationMessage() вспомогательные методы.

После выполнения правила проверки, проверяется свойство IsValid ModelState. Свойство IsValid возвращает значение false, после добавления сообщения об ошибках проверки для ModelState. Если проверка завершается неудачно, создание формы отобразится в сообщения об ошибках.

> [!NOTE] 
> 
> У меня регулярные выражения для проверки адреса электронной почты и номер телефона из репозитория регулярных выражений в [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Добавление логики проверки в действие изменения

Действие Edit() обновляет контакт. Действие Edit() должна выполнить ту же проверку, как действие Create(). Вместо повторения один и тот же код проверки, мы следует перерабатывать контроллера контакта, таким образом, Create() и Edit() действия вызывать один и тот же метод проверки.

Измененный класс контроллера Contact содержится в листинге 3. Этот класс содержит новый метод ValidateContact(), который вызывается в Create() и Edit() действия.

**Листинг 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Сводка

В этой итерации мы добавили проверки базовой форме в наше приложение диспетчера контактов. Наша логика проверки запрещает пользователям отправке нового контакта или редактировании существующего контакта без указания значения для свойства FirstName и LastName. Кроме того пользователи должны предоставлять актуальный телефонный номеров и адресов электронной почты.

В этой итерации мы добавили логику проверки в наше приложение диспетчера контактов простым способом. Тем не менее смешение наша логика проверки в логику контроллера создаст проблем в долгосрочной перспективе. Наше приложение будет сложнее в обслуживании и изменении со временем.

В следующей итерации мы рефакторинга наших логику проверки и логики доступа к базе данных из наших контроллеров. Мы рассмотрим преимущества несколько принципов проектирования программного обеспечения, чтобы мы могли создать приложение более разобщенными и упрощает управление системой.

> [!div class="step-by-step"]
> [Назад](iteration-2-make-the-application-look-nice-vb.md)
> [Вперед](iteration-4-make-the-application-loosely-coupled-vb.md)
