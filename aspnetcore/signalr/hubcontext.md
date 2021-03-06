---
title: SignalR HubContext
author: rachelappel
description: Узнайте, как использовать службу ASP.NET Core SignalR HubContext для отправки уведомлений клиентам за пределами концентратора.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277765"
---
# <a name="send-messages-from-outside-a-hub"></a>Отправка сообщения извне концентратора

По [Mengistu Микаэль](https://twitter.com/MikaelM_12)

Центр SignalR — это основная абстракция для отправки сообщений для клиентов, подключенных к серверу SignalR. Можно также отправлять сообщения из других частях приложения с использованием `IHubContext` службы. В этой статье объясняется, как получить доступ к SignalR `IHubContext` для отправки уведомлений клиентам за пределами концентратора.

[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(как для загрузки)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>Получите экземпляр класса `IHubContext`

В ASP.NET Core SignalR, можно обращаться к экземпляру компонента `IHubContext` через внедрения зависимостей. Можно запустить экземпляр `IHubContext` в контроллере, по промежуточного слоя или другая служба DI. Используйте экземпляр для отправки сообщений клиентам.

> [!NOTE]
> Это отличается от ASP.NET SignalR, которое используется для предоставления доступа к GlobalHost `IHubContext`. ASP.NET Core имеет платформа внедрения зависимостей, которая избавляет от необходимости этот глобальный singleton.

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>Запустить экземпляр `IHubContext` в контроллере

Можно запустить экземпляр `IHubContext` в контроллер, добавив его в конструктор:

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

Теперь с доступом к экземпляру `IHubContext`, как будто находятся в самой концентратора можно вызывать методы концентратора.

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>Получить экземпляр `IHubContext` в по промежуточного слоя

Доступ `IHubContext` в рамках конвейера по промежуточного слоя следующим образом:

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> При вызове методов концентратора из вне `Hub` класса, то связанные с вызовом вызывающий объект. Таким образом, нет нет доступа к `ConnectionId`, `Caller`, и `Others` свойства.

## <a name="related-resources"></a>Связанные ресурсы

* [Начало работы](xref:tutorials/signalr)
* [Центры](xref:signalr/hubs)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
