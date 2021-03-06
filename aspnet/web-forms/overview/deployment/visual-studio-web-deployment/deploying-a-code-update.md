---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода | Документация Майкрософт'
author: tdykstra
description: В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения, веб-приложениях службы приложений Azure или у стороннего поставщика размещения, Пол...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833359"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Веб-развертывание ASP.NET с помощью Visual Studio: развертывание обновления кода
====================
по [том Дайкстра](https://github.com/tdykstra)

[Загрузите начальный проект](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> В этой серии руководств показано, как развернуть ASP.NET (публикации) веб-приложения для веб-приложениях службы приложений Azure или стороннего поставщика услуг размещения, с помощью Visual Studio 2012 или Visual Studio 2010. Сведения об этой серии см. в разделе [в первом учебнике серии](introduction.md).


## <a name="overview"></a>Обзор

После первоначального развертывания продолжает работу обслуживание и разработка веб-сайт, и вскоре требуется развернуть обновления. В этом руководстве описываются процесс развертывания обновления в код приложения. Обновление, реализации и развертывании в этом руководстве не влечет за собой изменение базы данных; Вы увидите, чем отличается развертывание изменения базы данных в следующем учебном курсе.

Напоминание: Если что-то не работает, как работать с учебником, вы получаете сообщение об ошибке, не забудьте установить флажок [страница устранения неполадок](troubleshooting.md).

## <a name="make-a-code-change"></a>Изменения кода

В качестве простого примера обновления для приложения, вы добавите **преподавателей** странице Список курсов, проводимых выбранным преподавателем.

При запуске **преподавателей** страницы, можно заметить, что существуют **выберите** ссылки в сетке, но они не было присвоено имя, отличное от марки серый цвет фона Включение строк.

![Странице "Instructors" с выбором](deploying-a-code-update/_static/image1.png)

Теперь добавьте код, который выполняется при запуске **выберите** ссылку нажатии и отображает список курсов, проводимых выбранным преподавателем.

1. В *Instructors.aspx*, добавьте следующую разметку сразу после **ErrorMessageLabel** `Label` управления:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Откройте страницу и выберите преподавателя. Появится список курсов, проводимых этого преподавателя.

    ![Странице "Instructors" с помощью курсов обучения](deploying-a-code-update/_static/image2.png)
3. Закройте браузер.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Развертывание обновления кода в тестовой среде

Перед использованием профилей публикации для развертывания для тестирования, промежуточной и рабочей среде, необходимо изменить параметры публикации базы данных. Больше не нужно выполнять скрипты развертывания grant и данных для базы данных членства.

1. Откройте **веб-публикации** мастера, щелкнув правой кнопкой мыши проект ContosoUniversity команду **публикации**.
2. Нажмите кнопку **теста** профиля в **профиль** стрелку раскрывающегося списка.
3. Нажмите кнопку **параметры** вкладки.
4. В разделе **DefaultConnection** в **баз данных** снимите **обновления базы данных** "флажок".
5. Нажмите кнопку **профиль** , а затем щелкните **промежуточной** профиля в **профиль** стрелку раскрывающегося списка.
6. При появлении, если вы хотите сохранить изменения, внесенные в **теста** , установите флажок **Да**.
7. Сделать то же изменение в профиле промежуточного хранения.
8. Повторите процедуру, чтобы сделать то же изменение в профиле рабочей среде.
9. Закрыть **веб-публикации** мастера.

Развертывание в тестовую среду — теперь просто выполняется одним щелчком повторной публикации. Чтобы сделать этот процесс быстрее, можно использовать **веб-публикация одним щелкните** панели инструментов.

1. В **представление** меню, выберите **панелей инструментов** , а затем выберите **веб-публикация одним щелкните**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. В **обозревателе решений**, выберите проект ContosoUniversity.
3. **веб-публикация одним щелкните** панели инструментов выберите **теста** профиль публикации, а затем нажмите кнопку **веб-публикации** (значок со стрелками влево и вправо).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio развертывает обновленное приложение и в браузере автоматически откроется на домашнюю страницу.
5. Откройте страницу Instructors и выберите преподавателя, чтобы проверить, что обновление успешно развернут.

Обычным также выполнить тестирование регрессии (то есть проверка остальной части сайта, чтобы убедиться в том, что новое изменение не нарушили существующие функциональные возможности). Но в этом руководстве можно будет пропустить этот шаг и следовать для развертывания обновления в промежуточной и рабочей среде.

При повторном развертывании, веб-развертывания автоматически определяет, какие файлы были изменены, и копирует только измененные файлы на сервер. По умолчанию веб-развертывание использует даты последнего изменения в файлы для определения, какие из них были изменены. Некоторые системы управления версиями изменения файла даты даже не изменяйте содержимое файла. В этом случае можно настроить веб-развертывание для использования контрольных сумм для определения, какие файлы были изменены. Дополнительные сведения см. в разделе [почему все мои файлы повторном развертывании несмотря на то, что я не изменили их?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) в часто задаваемых вопросов развертывания ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Возьмем приложение в автономный режим во время развертывания

Изменение, которое вы развертываете теперь является простое изменение на одной странице. Но иногда развертывание более масштабным изменениям, или развертывании изменений кода и базы данных и сайт может работать неправильно, если пользователь запрашивает страницу до завершения развертывания. Чтобы запретить пользователям доступ к сайту во время развертывания, можно использовать *приложения\_offline.htm* файла. Если поместить файл с именем *приложения\_offline.htm* в корневой папке приложения, службы IIS автоматически отображает этот файл, а не посредством запуска приложения. Поэтому для предотвращения доступа во время развертывания, находиться *приложения\_offline.htm* в корневой папке, запустите процесс развертывания, а затем удалите *приложения\_offline.htm* после успешной развертывание.

Можно настроить веб-развертывания для автоматического размещения по умолчанию *приложения\_offline.htm* файл на сервере при запуске развертывания и удалите его после его завершения. Что все, что необходимо сделать это добавьте следующий XML-элемент для публикации (.pubxml) профиля:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

В этом руководстве вы узнаете, как создать и использовать пользовательский *приложения\_offline.htm* файла.

С помощью *приложения\_offline.htm* на промежуточном сайте не требуется, так как у вас нет пользователей, обращающихся к промежуточный сайт. Но рекомендуется использовать промежуточного хранения для все проверки того, который вы планируете развернуть в рабочей среде.

### <a name="create-appofflinehtm"></a>Создание приложения\_offline.htm

1. В **обозревателе решений**, щелкните правой кнопкой мыши решение и нажмите кнопку **добавить**, а затем нажмите кнопку **новый элемент**.
2. Создание **HTML-страницу** с именем *приложения\_offline.htm* (Удаление последней «l» в *.html* расширение, Visual Studio по умолчанию).
3. Замените разметку шаблона следующую разметку:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Сохраните и закройте файл.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Копирование приложения\_offline.htm в корневую папку веб-сайта

Можно использовать любые средства FTP для копирования файлов на веб-сайт. [FileZilla](http://filezilla-project.org/) — это популярный инструмент FTP и показан на снимках экрана.

Чтобы использовать средство FTP, необходимы три вещи: URL-адрес FTP, имя пользователя и пароль.

URL-адрес отображается на странице панели мониторинга веб сайта на портале управления Azure и имя пользователя и пароль для FTP-сервера можно найти в *.publishsettings* файл, который вы загрузили ранее. Ниже показано, как получить эти значения.

1. На портале управления Azure щелкните **веб-сайтов** вкладку, а затем нажмите кнопку промежуточного веб-сайта.
2. На **панели мониторинга** странице прокрутите вниз, чтобы найти имя узла FTP в **быстрый обзор** раздел.

    ![Имя узла FTP](deploying-a-code-update/_static/image5.png)
3. Откройте промежуточной *.publishsettings* файл в блокноте или другом текстовом редакторе.
4. Найти `publishProfile` элемент, для FTP-профиля.
5. Копировать `userName` и `userPWD` значения.

    ![Учетные данные FTP](deploying-a-code-update/_static/image6.png)
6. Откройте FTP-клиент и войдите в систему URL-адрес FTP.
7. Копировать *приложения\_offline.htm* из папки решения для */site/wwwroot* папку на промежуточном сайте.

    ![Скопируйте app_offline](deploying-a-code-update/_static/image7.png)
8. Перейдите по URL-АДРЕСУ промежуточный сайт. Вы увидите, что *приложения\_offline.htm* страницы будет отображаться вместо домашней страницы.

    ![App_offline.htm в окне браузера](deploying-a-code-update/_static/image8.png)

Теперь вы готовы для развертывания в промежуточную среду.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Развертывание обновления кода в промежуточной и рабочей средах

1. В **веб-публикация одним щелкните** панели инструментов выберите **промежуточной** профиль публикации, а затем нажмите кнопку **веб-публикации**.

    Visual Studio развернет обновленное приложение и откроет в браузере домашнюю страницу. *Приложения\_offline.htm* отобразится файл. Перед тестированием для проверки успешного развертывания, необходимо удалить *приложения\_offline.htm* файла.
2. Вернитесь в ваше средство FTP и удалите **приложения\_offline.htm** промежуточного сайта.
3. В браузере откройте страницу Instructors в промежуточный сайт и выберите преподавателя, чтобы проверить, что обновление успешно развернут.
4. Выполните ту же процедуру для рабочей среды, как это делалось промежуточного хранения.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Просмотр изменений и развертывание определенных файлов

Visual Studio 2012 также дает возможность развертывать отдельные файлы. Для выбранного файла можно просмотреть различия между локальной и развернутой версии, разверните файл в целевой среде или скопируйте файл из целевой среды для локального проекта. В этом разделе руководства вы научитесь использовать эти возможности.

### <a name="make-a-change-to-deploy"></a>Изменения для развертывания

1. Откройте *Content/Site.css*и найти блок для `body` тега.
2. Измените значение `background-color` из `#fff` для `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Просмотр изменений в окне Просмотр публикации

При использовании **веб-публикации** мастер должен опубликовать проект, вы увидите, что изменения будут опубликованы, дважды щелкнув файл в **предварительной версии** окна.

1. Щелкните правой кнопкой мыши проект ContosoUniversity и нажмите кнопку **публикации**.
2. Из **профиль** стрелку раскрывающегося списка выберите **теста** профиль публикации.
3. Нажмите кнопку **предварительной версии**, а затем нажмите кнопку **начать просмотр**.
4. В **предварительной версии** панель, дважды щелкните **Site.css**.

    ![Дважды щелкните файл Site.css](deploying-a-code-update/_static/image9.png)

    **Предварительный просмотр изменений** диалоговое окно отображается предварительный просмотр изменений, которые будут развернуты.

    ![Просмотр изменений в Site.css](deploying-a-code-update/_static/image10.png)

    Если дважды щелкнуть *Web.config* файл, **Предварительный просмотр изменений** диалогового окна показан результат построения преобразования конфигураций и опубликовать профиль преобразования. На этом этапе вы ничего не сделали, приводящие к *Web.config* файла на сервере, чтобы изменить, поэтому вы ожидаете увидеть без изменений. Тем не менее **Предварительный просмотр изменений** окне неправильно отображаются два изменения. Похоже, два XML-элементы будут удалены. Эти элементы добавляются процессом публикации, при выборе **выполнить Code First Migrations при запуске приложения** для Code First класс контекста. Сравнение выполняется в том случае, прежде чем добавить процесс публикации этих элементов, так выглядит так, будто они удаляются несмотря на то, что они не будут удалены. Эта ошибка будет исправлена в будущих выпусках.
5. Нажмите кнопку **Закрыть**.
6. Нажмите кнопку **Опубликовать**.
7. Когда откроется браузер на домашнюю страницу сайта теста, нажмите CTRL + F5, чтобы вызывает жестких обновления, чтобы увидеть эффект от изменения CSS.

    ![Эффект изменения CSS](deploying-a-code-update/_static/image11.png)
8. Закройте браузер.

### <a name="publish-specific-files-from-solution-explorer"></a>Опубликовать определенные файлы из обозревателя решений

Предположим, что не нравится синий фон и восстановить исходный цвет. В этом разделе необходимо восстановить исходные параметры путем публикации определенного файла непосредственно из **обозревателе решений**.

1. Откройте *Content/Site.css* копирование и восстановление `background-color` значение `#fff`.
2. В **обозревателе решений**, щелкните правой кнопкой мыши *Content/Site.css* файла.

    В контекстном меню показаны три параметров публикации.

    ![Публиковать параметры из обозревателя решений](deploying-a-code-update/_static/image12.png)
3. Нажмите кнопку **Просмотр изменений в Site.css**.

    Откроется окно для отображения различий между локального файла и его версию в целевой среде.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. В **обозревателе решений**, щелкните правой кнопкой мыши **Site.css** еще раз и нажмите кнопку **публикации Site.css**.

    **Действий веб-публикации** окно показывает, что файл был опубликован.

    ![Окно действия публикации Web](deploying-a-code-update/_static/image14.png)
5. Откройте в браузере `http://localhost/contosouniversity` URL-адрес и нажмите клавишу CTRL + F5, чтобы вызвать жесткой обновить, чтобы увидеть эффект CSS изменения.

    ![Домашняя страница с обычной CSS](deploying-a-code-update/_static/image15.png)
6. Закройте браузер.

## <a name="summary"></a>Сводка

Теперь вы увидели несколько способов развертывания обновления приложения, не связанных с изменениями в базе данных, и вы узнали, как просматривать изменения, чтобы проверить, что будет обновляться ожиданиям. Теперь есть страницы преподавателей **курсы под руководством** раздел.

![Странице "Instructors" с помощью курсов обучения](deploying-a-code-update/_static/image16.png)

Следующее руководство показано, как развернуть изменения базы данных: вы добавите поле даты рождения, к базе данных и на страницу преподавателей.

> [!div class="step-by-step"]
> [Назад](deploying-to-production.md)
> [Вперед](deploying-a-database-update.md)
