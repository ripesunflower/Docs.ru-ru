---
uid: webhooks/diagnostics/debugging
title: Веб-перехватчики ASP.NET Отладка | Документация Майкрософт
author: rick-anderson
description: Способы отладки ASP.NET веб-перехватчики.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801992"
---
# <a name="aspnet-webhooks-debugging"></a>Веб-перехватчики ASP.NET отладки  

## <a name="debugging-in-azure"></a>Отладка в Azure

Отладка веб-приложения во время выполнения в Azure, см. в разделе учебника [Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Отладка с помощью исходного кода и символов

В дополнение к отладке кода, можно отлаживать непосредственно в веб-перехватчики Microsoft ASP.NET и на самом деле все .NET. Это работает независимо от того, является ли отладка локально или удаленно. Во-первых, настроить Visual Studio, чтобы найти источник и символы в **Отладка** и затем **параметры и настройки**. Задание параметров следующим образом:

![Параметры и настройки](_static/SourceSymbols.png)

Затем добавьте ссылку на [symbolsource.org](http://symbolsource.org) для загрузки исходного кода и символов. Перейдите к **символы** вкладке меню вверху и добавьте следующее расположение символов:

```
http://srv.symbolsource.org/pdb/Public
```

Кроме того убедитесь, что каталог кэша имеет короткое имя; в противном случае имена файлов можно получить слишком длинное вызывающее символы, не загружаются. — Пример пути:

```
C:\SymCache
```

Параметры должны выглядеть примерно следующим образом:

![Пример расположения файла параметры символов](_static/SymSource.png)
