---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: "Тестирование стойкость пароля (VB) | Документы Microsoft"
author: wenz
description: "Пароли являются обязательными практически в любом месте, чтобы отложенной пользователей, как правило, выберите простые пароли, которые легко взломать. Элемент управления PasswordStrength в ASP. N...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f09a05fd4b5771b7ab532d40476fe45cbd3fe38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="5c602-104">Тестирование стойкость пароля (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="5c602-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="5c602-105">по [Кристиан Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5c602-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5c602-106">[Загрузить код](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) или [скачать PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5c602-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="5c602-107">Пароли являются обязательными практически в любом месте, чтобы отложенной пользователей, как правило, выберите простые пароли, которые легко взломать.</span><span class="sxs-lookup"><span data-stu-id="5c602-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="5c602-108">PasswordStrength управления в наборе элементов управления ASP.NET AJAX можно проверить, насколько хорошо находится пароль.</span><span class="sxs-lookup"><span data-stu-id="5c602-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="5c602-109">Обзор</span><span class="sxs-lookup"><span data-stu-id="5c602-109">Overview</span></span>

<span data-ttu-id="5c602-110">Пароли являются обязательными практически в любом месте, чтобы отложенной пользователей, как правило, выберите простые пароли, которые легко взломать.</span><span class="sxs-lookup"><span data-stu-id="5c602-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="5c602-111">`PasswordStrength` Элемента управления в наборе элементов управления ASP.NET AJAX можно проверить, насколько хорошо находится пароль.</span><span class="sxs-lookup"><span data-stu-id="5c602-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="5c602-112">Шаги</span><span class="sxs-lookup"><span data-stu-id="5c602-112">Steps</span></span>

<span data-ttu-id="5c602-113">`PasswordStrength` Управления расширяет текстовое поле и проверяет, является ли пароль в нем достаточно хорошо.</span><span class="sxs-lookup"><span data-stu-id="5c602-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="5c602-114">Он предлагает широкий набор функций через атрибуты; Ниже приведены только некоторые из них.</span><span class="sxs-lookup"><span data-stu-id="5c602-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="5c602-115">`MinimumNumericCharacters`Минимальное число цифр в пароле</span><span class="sxs-lookup"><span data-stu-id="5c602-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="5c602-116">`MinimumSymbolCharacters`Минимальное количество специальных символов (не букв и цифр) в пароле</span><span class="sxs-lookup"><span data-stu-id="5c602-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="5c602-117">`PreferredPasswordLength`Минимальная длина пароля</span><span class="sxs-lookup"><span data-stu-id="5c602-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="5c602-118">`RequiresUpperAndLowerCaseCharacters`нужно ли использовать прописные и строчные буквы пароль</span><span class="sxs-lookup"><span data-stu-id="5c602-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="5c602-119">`StrengthIndicatorType` Сведения будут отображаться стойкость пароля, как текст (значение `"Text"`) или как тип индикатор хода выполнения (значение `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="5c602-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="5c602-120">В `DisplayPosition` атрибут, настройке где отображаются сведения.</span><span class="sxs-lookup"><span data-stu-id="5c602-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="5c602-121">Ниже приведен полный пример, включая ASP.NET AJAX `ScriptManager` управления `PasswordStrength` управления и, конечно, текстовое поле, где пользователь может ввести пароль.</span><span class="sxs-lookup"><span data-stu-id="5c602-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="5c602-122">Для примера поле последняя форма является регулярного текстовое поле, а не поле пароля, чтобы во время разработки можно увидеть введя.</span><span class="sxs-lookup"><span data-stu-id="5c602-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="5c602-123">Запустите страницу и введите отсутствовали: только после ввода строчные буквы, прописные буквы, цифры и символы, считается как неразрывный пароль.</span><span class="sxs-lookup"><span data-stu-id="5c602-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="5c602-124">[![Теперь пароль () вполне](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5c602-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="5c602-125">Теперь пароль () вполне ([Просмотр полноразмерное изображение](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5c602-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5c602-126">Назад</span><span class="sxs-lookup"><span data-stu-id="5c602-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)