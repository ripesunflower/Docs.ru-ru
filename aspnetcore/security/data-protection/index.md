---
title: Защита данных в ASP.NET Core
author: rick-anderson
description: В этом документе приводится перечень различных разделов о защите данных в ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277128"
---
# <a name="data-protection-in-aspnet-core"></a>Защита данных в ASP.NET Core

* [Общие сведения о защите данных](xref:security/data-protection/introduction)

* [Начало работы с API защиты данных](xref:security/data-protection/using-data-protection)

* [Потребительские API](xref:security/data-protection/consumer-apis/index)

  * [Обзор потребительских API](xref:security/data-protection/consumer-apis/overview)

  * [Строки назначений](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Иерархия назначений и мультитенантность](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Хэшированные пароли](xref:security/data-protection/consumer-apis/password-hashing)

  * [Ограничение времени существования защищенных полезных данных](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Снятие защиты полезных данных, ключи которых были отозваны](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Конфигурация](xref:security/data-protection/configuration/index)

  * [Настройка защиты данных в ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Параметры по умолчанию](xref:security/data-protection/configuration/default-settings)

  * [Политики уровня компьютера](xref:security/data-protection/configuration/machine-wide-policy)

  * [Сценарии, не поддерживающие внедрение зависимостей](xref:security/data-protection/configuration/non-di-scenarios)

* [API расширяемости](xref:security/data-protection/extensibility/index)

  * [Расширяемость базового шифрования](xref:security/data-protection/extensibility/core-crypto)

  * [Расширяемость управления ключами](xref:security/data-protection/extensibility/key-management)

  * [Различные API](xref:security/data-protection/extensibility/misc-apis)

* [Реализация](xref:security/data-protection/implementation/index)

  * [Сведения о шифровании с проверкой подлинности](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Формирование подключа и шифрование с проверкой подлинности](xref:security/data-protection/implementation/subkeyderivation)

  * [Заголовки контекста](xref:security/data-protection/implementation/context-headers)

  * [Управление ключами](xref:security/data-protection/implementation/key-management)

  * [Поставщики хранилищ ключей](xref:security/data-protection/implementation/key-storage-providers)

  * [Шифрование ключей при хранении](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Неизменность и параметры ключа](xref:security/data-protection/implementation/key-immutability)

  * [Формат хранилища ключей](xref:security/data-protection/implementation/key-storage-format)

  * [Временные поставщики защиты данных](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Совместимость](xref:security/data-protection/compatibility/index)

  * [Замена ASP.NET <machineKey> в ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
