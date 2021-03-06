---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First с ASP.NET MVC: Настройка представления | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7359b8daddc74e375675d73126d7d76b288e853d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817192"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First с ASP.NET MVC: Настройка представления
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных. Созданный код соответствует столбцам в таблице базы данных.
> 
> Части этой серии посвящена изменении представлений автоматически создается, чтобы улучшить представление.


## <a name="add-enrolled-courses-to-student-details"></a>Добавление зарегистрированных курсов сведения об учащихся

Сформированный код предусматривает хорошей отправной точкой для вашего приложения, но он не обязательно предоставляет все функциональные возможности, необходимые в приложении. Вы можете настроить код в соответствии с требованиями приложения. В настоящее время приложение не отображает зарегистрированные курсов для выбранного учащегося. В этом разделе вы добавите зарегистрированных курсов для каждого учащегося, чтобы **сведения** представление для учащегося.

Откройте **Students/Details.cshtml**и ниже последнего &lt;/dl&gt; вкладки, но перед закрывающей скобкой &lt;/div&gt; , добавьте следующий код.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Этот код создает таблицу, которая отображается по одной строке для каждой записи в таблицу Enrollment для выбранного учащегося. **Отображения** метод выводит HTML для объекта (modelItem), который представляет выражение. Используйте метод отображения (а не просто внедрение значение свойства в коде) чтобы убедиться в том, значение форматируется правильно на основе его типа и шаблон для этого типа. В этом примере каждое выражение возвращает одно свойство из текущей записи в цикле, а значения являются типами-примитивами, которые подготавливаются к просмотру как текст.

Перейдите в представление указателя учащихся и еще раз и выберите **сведения** для одного из учащихся. Вы увидите, что Зарегистрированные курсы были включены в представлении.

![учащихся с регистрацией](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Назад](changing-the-database.md)
> [Вперед](enhancing-data-validation.md)
