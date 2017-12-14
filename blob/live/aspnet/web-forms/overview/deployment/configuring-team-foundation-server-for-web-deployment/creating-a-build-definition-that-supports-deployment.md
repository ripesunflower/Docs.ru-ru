---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: "Создание определения построения, поддерживающего развертывание | Документы Microsoft"
author: jrjlee
description: "Если вы хотите выполнять любой вид сборки в Team Foundation Server (TFS) 2010, необходимо создать определение построения в пределах командного проекта. Это статье des..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c2e7a768c2cf9900731b822ec187093a4b250ead
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-build-definition-that-supports-deployment"></a><span data-ttu-id="f028b-104">Создание определения построения, поддерживающего развертывание</span><span class="sxs-lookup"><span data-stu-id="f028b-104">Creating a Build Definition That Supports Deployment</span></span>
====================
<span data-ttu-id="f028b-105">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="f028b-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="f028b-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="f028b-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="f028b-107">Если вы хотите выполнять любой вид сборки в Team Foundation Server (TFS) 2010, необходимо создать определение построения в пределах командного проекта.</span><span class="sxs-lookup"><span data-stu-id="f028b-107">If you want to perform any kind of build in Team Foundation Server (TFS) 2010, you need to create a build definition within your team project.</span></span> <span data-ttu-id="f028b-108">В этом разделе описывается, как создать новое определение сборки в TFS и как управлять веб-развертывания в рамках процесса сборки в Team Build.</span><span class="sxs-lookup"><span data-stu-id="f028b-108">This topic describes how to create a new build definition in TFS and how to control web deployment as part of the build process in Team Build.</span></span>


<span data-ttu-id="f028b-109">Этот раздел входит в состав серию учебников, исходя из требования к развертыванию enterprise вымышленная компания Fabrikam, Inc. Этот учебник ряд использует образец решения & #x 2014; [решения диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; для представления веб-приложения с реалистичных уровень сложности, включая приложения ASP.NET MVC 3, Windows Службы Communication Foundation (WCF) и проект базы данных.</span><span class="sxs-lookup"><span data-stu-id="f028b-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="f028b-110">Метод развертывания, в основе этих учебников основан на разбиение проекта файл подход, описанный в [основные сведения о файле проекта](../web-deployment-in-the-enterprise/understanding-the-project-file.md), в которой управляет процессом построения и развертывания двух файлов проекта & #x 2014; o NE, содержащий инструкции сборки, которые применяются для каждой целевой среде и, содержащий параметры построения и развертывания конкретной среды.</span><span class="sxs-lookup"><span data-stu-id="f028b-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="f028b-111">Во время построения файла проекта среды объединяется в файл проекта зависит от среды, образуют полный набор инструкций построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="f028b-112">Общие сведения о задаче</span><span class="sxs-lookup"><span data-stu-id="f028b-112">Task Overview</span></span>

<span data-ttu-id="f028b-113">Определение сборки — это механизм, которое определяет, как и когда построения выполняются для командных проектов в TFS.</span><span class="sxs-lookup"><span data-stu-id="f028b-113">A build definition is the mechanism that controls how and when builds occur for team projects in TFS.</span></span> <span data-ttu-id="f028b-114">Указывает определения сборки:</span><span class="sxs-lookup"><span data-stu-id="f028b-114">Each build definition specifies:</span></span>

- <span data-ttu-id="f028b-115">Задачи, которые требуется выполнить сборку, такие как файлы решения Visual Studio или пользовательские файлы проекта Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="f028b-115">The things you want to build, like Visual Studio solution files or custom Microsoft Build Engine (MSBuild) project files.</span></span>
- <span data-ttu-id="f028b-116">Условия, которые определяют, когда необходимо выполнить сборку или кавычки, как вручную триггеры, непрерывной интеграции (CI) или проверкой.</span><span class="sxs-lookup"><span data-stu-id="f028b-116">The criteria that determine when a build should take place, like manual triggers, continuous integration (CI), or gated check-ins.</span></span>
- <span data-ttu-id="f028b-117">Расположение, в котором Team Build должны отправлять выходные данные сборки, включая развертывание артефактов, например веб-пакетов и скриптов базы данных.</span><span class="sxs-lookup"><span data-stu-id="f028b-117">The location to which Team Build should send build outputs, including deployment artifacts like web packages and database scripts.</span></span>
- <span data-ttu-id="f028b-118">Количество времени, должны сохраняться каждой сборки.</span><span class="sxs-lookup"><span data-stu-id="f028b-118">The amount of time that each build should be retained.</span></span>
- <span data-ttu-id="f028b-119">Различные другие параметры процесса построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-119">Various other parameters of the build process.</span></span>

> [!NOTE]
> <span data-ttu-id="f028b-120">Дополнительные сведения о определений сборки см. в разделе [определить процедуру сборки](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="f028b-120">For more information on build definitions, see [Define Your Build Process](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span></span>


<span data-ttu-id="f028b-121">В этом разделе показано, как создать определение сборки, которое использует CI, чтобы сборки запускается в том случае, когда разработчик возвращает в новое содержимое.</span><span class="sxs-lookup"><span data-stu-id="f028b-121">This topic will show you how to create a build definition that uses CI, so that a build is triggered when a developer checks in new content.</span></span> <span data-ttu-id="f028b-122">В случае успешного построения, запущена служба построения пользовательском файле проекта для развертывания решения в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="f028b-122">If the build succeeds, the build service runs a custom project file to deploy the solution to a test environment.</span></span>

<span data-ttu-id="f028b-123">Если запустить сборку, эти действия должно произойти:</span><span class="sxs-lookup"><span data-stu-id="f028b-123">When you trigger a build, these actions need to happen:</span></span>

- <span data-ttu-id="f028b-124">Во-первых Team Build необходимо построить решение.</span><span class="sxs-lookup"><span data-stu-id="f028b-124">First, Team Build should build the solution.</span></span> <span data-ttu-id="f028b-125">В рамках этого процесса Team Build будет вызывать конвейера публикации Web (WPP) для создания пакетов веб-развертывания для каждого из проектов веб-приложений в решении.</span><span class="sxs-lookup"><span data-stu-id="f028b-125">As part of this process, Team Build will invoke the Web Publishing Pipeline (WPP) to generate web deployment packages for each of the web application projects in the solution.</span></span> <span data-ttu-id="f028b-126">Team Build будет также выполняться все модульные тесты, связанные с решением.</span><span class="sxs-lookup"><span data-stu-id="f028b-126">Team Build will also run any unit tests associated with the solution.</span></span>
- <span data-ttu-id="f028b-127">При сбое построения решений, Team Build займет никаких дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="f028b-127">If the solution build fails, Team Build should take no further action.</span></span> <span data-ttu-id="f028b-128">Сбоев модульных тестов должны рассматриваться как сбой сборки.</span><span class="sxs-lookup"><span data-stu-id="f028b-128">Unit test failures should be treated as a build failure.</span></span>
- <span data-ttu-id="f028b-129">В случае успешного построения решения, Team Build выполняйте файле пользовательского проекта, управляющий развертывания решения.</span><span class="sxs-lookup"><span data-stu-id="f028b-129">If the solution build succeeds, Team Build should run the custom project file that controls the deployment of the solution.</span></span> <span data-ttu-id="f028b-130">В рамках этого процесса Team Build будет вызывать средство Internet Information Services (IIS) веб-развертывания (Web Deploy) для установки упакованных веб-приложений на веб-серверах назначения, и она вызывает служебную программу VSDBCMD.exe, чтобы запустить процесс создания базы данных сценарии на целевых серверах базы данных.</span><span class="sxs-lookup"><span data-stu-id="f028b-130">As part of this process, Team Build will invoke the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to install the packaged web applications on the destination web servers, and it will invoke the VSDBCMD.exe utility to run database creation scripts on the destination database servers.</span></span>

<span data-ttu-id="f028b-131">Это показывает процесс:</span><span class="sxs-lookup"><span data-stu-id="f028b-131">This illustrates the process:</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

<span data-ttu-id="f028b-132">[Диспетчера контактов](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) образец решения включает в себя пользовательского файла проекта MSBuild *Publish.proj*, который можно запустить из MSBuild или Team Build.</span><span class="sxs-lookup"><span data-stu-id="f028b-132">The [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution includes a custom MSBuild project file, *Publish.proj*, that you can run from MSBuild or Team Build.</span></span> <span data-ttu-id="f028b-133">Как описано в [основные сведения о процессе сборки](../web-deployment-in-the-enterprise/understanding-the-build-process.md), этот файл проекта определяет логику, которая развертывает веб-пакетов и баз данных в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="f028b-133">As described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), this project file defines the logic that deploys your web packages and databases to a target environment.</span></span> <span data-ttu-id="f028b-134">Файл содержит логику, которая пропускает построения и обработки пакетов, если он выполняется в Team Build, оставляя только задачи развертывания для запуска.</span><span class="sxs-lookup"><span data-stu-id="f028b-134">The file includes logic that omits the building and packaging process if it's running in Team Build, leaving just the deployment tasks to run.</span></span> <span data-ttu-id="f028b-135">Это потому, что при автоматизации развертывания таким образом, обычно имеет смысл убедиться, что сборка решения выполняется успешно и передает все модульные тесты, прежде чем начнется процесс развертывания.</span><span class="sxs-lookup"><span data-stu-id="f028b-135">This is because when you automate deployment in this way, you'll typically want to ensure that the solution builds successfully and passes any unit tests before the deployment process commences.</span></span>

<span data-ttu-id="f028b-136">В следующем разделе показано, как реализовать этот процесс, создав новое определение сборки.</span><span class="sxs-lookup"><span data-stu-id="f028b-136">The next section explains how to implement this process by creating a new build definition.</span></span>

> [!NOTE]
> <span data-ttu-id="f028b-137">Этой процедуры & #x 2014; создает один автоматический процесс проверяет и развертывает решение & #x 2014, скорее всего, будут наиболее подходит для развертывания в тестовую среду.</span><span class="sxs-lookup"><span data-stu-id="f028b-137">This procedure&#x2014;in which a single automated process builds, tests, and deploys a solution&#x2014;is likely to be most suited to deployment to test environments.</span></span> <span data-ttu-id="f028b-138">Для промежуточной и производственной средах вы более обычно требуется развернуть содержимое из предыдущего построения, которое уже проверены и проверить в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="f028b-138">For staging and production environments you're a lot more likely to want to deploy content from a previous build that you've already verified and validated in a test environment.</span></span> <span data-ttu-id="f028b-139">Этот подход описан в следующем разделе [развертывание определенной сборки](deploying-a-specific-build.md).</span><span class="sxs-lookup"><span data-stu-id="f028b-139">This approach is described in the next topic, [Deploying a Specific Build](deploying-a-specific-build.md).</span></span>


### <a name="who-performs-this-procedure"></a><span data-ttu-id="f028b-140">Кто выполняет эту процедуру?</span><span class="sxs-lookup"><span data-stu-id="f028b-140">Who Performs This Procedure?</span></span>

<span data-ttu-id="f028b-141">Как правило администратор TFS выполняет эту процедуру.</span><span class="sxs-lookup"><span data-stu-id="f028b-141">Typically, a TFS administrator performs this procedure.</span></span> <span data-ttu-id="f028b-142">В некоторых случаях руководитель группы разработчиков может отвечать за коллекции командных проектов в TFS.</span><span class="sxs-lookup"><span data-stu-id="f028b-142">In some cases, a developer team leader may take responsibility for the team project collection in TFS.</span></span> <span data-ttu-id="f028b-143">Чтобы создать новое определение сборки, необходимо быть членом группы **Администраторы построения коллекций проектов** для коллекции командных проектов, содержащая решение.</span><span class="sxs-lookup"><span data-stu-id="f028b-143">In order to create a new build definition, you need to be a member of the **Project Collection Build Administrators** group for the team project collection that contains your solution.</span></span>

## <a name="create-a-build-definition-for-ci-and-deployment"></a><span data-ttu-id="f028b-144">Создание определения построения для непрерывной Интеграции и развертывания</span><span class="sxs-lookup"><span data-stu-id="f028b-144">Create a Build Definition for CI and Deployment</span></span>

<span data-ttu-id="f028b-145">Далее описано, как для создания определения построения, которое запускает CI.</span><span class="sxs-lookup"><span data-stu-id="f028b-145">The next procedure describes how to create a build definition that CI triggers.</span></span> <span data-ttu-id="f028b-146">В случае успешного построения, развертывания решения с помощью логики в пользовательский файл проекта MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f028b-146">If the build succeeds, the solution is deployed using the logic in a custom MSBuild project file.</span></span>

<span data-ttu-id="f028b-147">**Чтобы создать определение построения для непрерывной Интеграции и развертывания**</span><span class="sxs-lookup"><span data-stu-id="f028b-147">**To create a build definition for CI and deployment**</span></span>

1. <span data-ttu-id="f028b-148">В Visual Studio 2010 в **Team Explorer** окна, разверните узел командного проекта, щелкните правой кнопкой мыши **строит**, а затем нажмите кнопку **создать определение построения**.</span><span class="sxs-lookup"><span data-stu-id="f028b-148">In Visual Studio 2010, in the **Team Explorer** window, expand your team project node, right-click **Builds**, and then click **New Build Definition**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. <span data-ttu-id="f028b-149">На **Общие** вкладки, присвойте имя определения сборки (например, **DeployToTest**) и необязательное описание.</span><span class="sxs-lookup"><span data-stu-id="f028b-149">On the **General** tab, give the build definition a name (for example, **DeployToTest**) and an optional description.</span></span>
3. <span data-ttu-id="f028b-150">На **триггер** выберите условия, в которых требуется запускают новую сборку.</span><span class="sxs-lookup"><span data-stu-id="f028b-150">On the **Trigger** tab, select the criteria on which you want to trigger a new build.</span></span> <span data-ttu-id="f028b-151">Например, если необходимо построить и развернуть в среде тестирования каждый раз, разработчик возвращает в новом коде, выберите **непрерывной интеграции**.</span><span class="sxs-lookup"><span data-stu-id="f028b-151">For example, if you want to build the solution and deploy to the test environment every time a developer checks in new code, select **Continuous Integration**.</span></span>
4. <span data-ttu-id="f028b-152">На **сборки по умолчанию** вкладке **Копировать выходные данные построения в следующую папку сброса** введите путь к папке сброса соглашения об универсальных именах (UNC) (например,  **\\TFSBUILD\Drops**).</span><span class="sxs-lookup"><span data-stu-id="f028b-152">On the **Build Defaults** tab, in the **Copy build output to the following drop folder** box, type the Universal Naming Convention (UNC) path of your drop folder (for example, **\\TFSBUILD\Drops**).</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="f028b-153">Это расположение сброса хранит несколько сборок, в зависимости от настроенной политики хранения.</span><span class="sxs-lookup"><span data-stu-id="f028b-153">This drop location stores several builds, depending on the retention policy you configure.</span></span> <span data-ttu-id="f028b-154">Если вы хотите опубликовать артефакты развертывания из конкретной сборки в промежуточной или рабочей среде, это где их можно найти.</span><span class="sxs-lookup"><span data-stu-id="f028b-154">When you want to publish deployment artifacts from a specific build to a staging or production environment, this is where you'll find them.</span></span>
5. <span data-ttu-id="f028b-155">На **процесс** вкладке **файл процесса построения** раскрывающегося списка, оставьте **DefaultTemplate.xaml** выбранных.</span><span class="sxs-lookup"><span data-stu-id="f028b-155">On the **Process** tab, in the **Build process file** dropdown list, leave **DefaultTemplate.xaml** selected.</span></span> <span data-ttu-id="f028b-156">Это один из шаблонов процесса сборки по умолчанию, которые добавляются в новые командные проекты.</span><span class="sxs-lookup"><span data-stu-id="f028b-156">This is one of the default build process templates that get added to all new team projects.</span></span>
6. <span data-ttu-id="f028b-157">В **параметры процесса построения** таблицы, нажмите кнопку в **элементы для построения** , а затем нажмите **многоточие** кнопки.</span><span class="sxs-lookup"><span data-stu-id="f028b-157">In the **Build process parameters** table, click in the **Items to Build** row, and then click the **ellipsis** button.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. <span data-ttu-id="f028b-158">В **элементы для построения** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f028b-158">In the **Items to Build** dialog box, click **Add**.</span></span>
8. <span data-ttu-id="f028b-159">Перейдите к нужному файл решения и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f028b-159">Browse to the location of your solution file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. <span data-ttu-id="f028b-160">В **элементы для построения** диалоговое окно, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="f028b-160">In the **Items to Build** dialog box, click **Add**.</span></span>
10. <span data-ttu-id="f028b-161">В **элементы типа** раскрывающегося списка выберите **файлы проекта MSBuild**.</span><span class="sxs-lookup"><span data-stu-id="f028b-161">In the **Items of type** dropdown list, select **MSBuild Project files**.</span></span>
11. <span data-ttu-id="f028b-162">Перейдите к нужному файлу пользовательского проекта с помощью которого контролировать процесс развертывания, выберите файл и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f028b-162">Browse to the location of the custom project file with which you control the deployment process, select the file, and then click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. <span data-ttu-id="f028b-163">**Элементы для построения** диалоговое окно появляются два элемента.</span><span class="sxs-lookup"><span data-stu-id="f028b-163">The **Items to Build** dialog box should now show two items.</span></span> <span data-ttu-id="f028b-164">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="f028b-164">Click **OK**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. <span data-ttu-id="f028b-165">На **процесс** вкладке **параметры процесса построения** таблица, разверните **Дополнительно** раздела.</span><span class="sxs-lookup"><span data-stu-id="f028b-165">On the **Process** tab, in the **Build process parameters** table, expand the **Advanced** section.</span></span>
14. <span data-ttu-id="f028b-166">В **Аргументы MSBuild** строк, добавьте все аргументы командной строки MSBuild, *либо* элементов для построения требуется.</span><span class="sxs-lookup"><span data-stu-id="f028b-166">In the **MSBuild Arguments** row, add any MSBuild command-line arguments that *either* of your items to build requires.</span></span> <span data-ttu-id="f028b-167">В сценарии решения диспетчера контактов эти аргументы не требуются:</span><span class="sxs-lookup"><span data-stu-id="f028b-167">In the Contact Manager solution scenario, these arguments are required:</span></span>

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. <span data-ttu-id="f028b-168">В этом примере:</span><span class="sxs-lookup"><span data-stu-id="f028b-168">In this example:</span></span>

    1. <span data-ttu-id="f028b-169">**DeployOnBuild = true** и **DeployTarget = пакет** аргументы являются обязательными при построении решения диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="f028b-169">The **DeployOnBuild=true** and **DeployTarget=package** arguments are required when you build the Contact Manager solution.</span></span> <span data-ttu-id="f028b-170">Это указывает, что MSBuild для создания веб-развертывания пакетов после построения проекта каждого веб-приложения, как описано в [построения и упаковки проектов веб-приложений](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span><span class="sxs-lookup"><span data-stu-id="f028b-170">This instructs MSBuild to create web deployment packages after building each web application project, as described in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>
    2. <span data-ttu-id="f028b-171">**TargetEnvPropsFile** аргумент является обязательным при построении *Publish.proj* файла.</span><span class="sxs-lookup"><span data-stu-id="f028b-171">The **TargetEnvPropsFile** argument is required when you build the *Publish.proj* file.</span></span> <span data-ttu-id="f028b-172">Это свойство указывает расположение файла конфигурации, относящиеся к среде, как описано в [основные сведения о процессе построения](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="f028b-172">This property indicates the location of the environment-specific configuration file, as described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>
16. <span data-ttu-id="f028b-173">На **политики хранения** настройте количество построений с результатами каждого типа, необходимо сохранить в соответствии с требованиями.</span><span class="sxs-lookup"><span data-stu-id="f028b-173">On the **Retention Policy** tab, configure how many builds of each type you want to retain as required.</span></span>
17. <span data-ttu-id="f028b-174">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="f028b-174">Click **Save**.</span></span>

## <a name="queue-a-build"></a><span data-ttu-id="f028b-175">Помещение построения в очередь</span><span class="sxs-lookup"><span data-stu-id="f028b-175">Queue a Build</span></span>

<span data-ttu-id="f028b-176">На этом этапе вы создали по крайней мере одного нового определения построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-176">At this point, you have created at least one new build definition.</span></span> <span data-ttu-id="f028b-177">Процесс построения, заданные теперь будет выполняться в соответствии с триггеры, указанный в определении сборки.</span><span class="sxs-lookup"><span data-stu-id="f028b-177">The build process you defined will now run according to the triggers you specified in the build definition.</span></span>

<span data-ttu-id="f028b-178">Если вы настроили определение сборки для использования элемента конфигурации, можно проверить определение сборки двумя способами:</span><span class="sxs-lookup"><span data-stu-id="f028b-178">If you've configured your build definition to use CI, you can test your build definition in two ways:</span></span>

- <span data-ttu-id="f028b-179">Проверка некоторое содержимое в командный проект, чтобы запустить автоматическую сборку.</span><span class="sxs-lookup"><span data-stu-id="f028b-179">Check in some content to the team project to trigger an automatic build.</span></span>
- <span data-ttu-id="f028b-180">Помещение построения в очередь вручную.</span><span class="sxs-lookup"><span data-stu-id="f028b-180">Queue a build manually.</span></span>

<span data-ttu-id="f028b-181">**Построение в очередь вручную**</span><span class="sxs-lookup"><span data-stu-id="f028b-181">**To queue a build manually**</span></span>

1. <span data-ttu-id="f028b-182">В **Team Explorer** окно, щелкните правой кнопкой мыши определение построения и нажмите кнопку **новую сборку в очередь**.</span><span class="sxs-lookup"><span data-stu-id="f028b-182">In the **Team Explorer** window, right-click the build definition, and then click **Queue New Build**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. <span data-ttu-id="f028b-183">В **поставить в очередь построение** диалоговое окно, просмотрите свойства построения и нажмите кнопку **очереди**.</span><span class="sxs-lookup"><span data-stu-id="f028b-183">In the **Queue Build** dialog box, review the build properties, and then click **Queue**.</span></span>

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

<span data-ttu-id="f028b-184">Чтобы просмотреть ход выполнения и результат сборки & #x 2014; независимо от того, является ли оно было запущено вручную или автоматически & #x 2014; дважды щелкните определение построения в **Team Explorer** окна.</span><span class="sxs-lookup"><span data-stu-id="f028b-184">To review the progress and the outcome of a build&#x2014;regardless of whether it was triggered manually or automatically&#x2014;double-click the build definition in the **Team Explorer** window.</span></span> <span data-ttu-id="f028b-185">После этого откроется **Обозреватель построений** вкладки.</span><span class="sxs-lookup"><span data-stu-id="f028b-185">This will open a **Build Explorer** tab.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

<span data-ttu-id="f028b-186">Здесь вы можете устранить сбоям сборок.</span><span class="sxs-lookup"><span data-stu-id="f028b-186">From here, you can troubleshoot failed builds.</span></span> <span data-ttu-id="f028b-187">Если дважды щелкнуть отдельные сборки, можно просмотреть сводные данные и просмотрите подробные журналы.</span><span class="sxs-lookup"><span data-stu-id="f028b-187">If you double-click an individual build, you can view summary information and click through to detailed log files.</span></span>

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

<span data-ttu-id="f028b-188">Эти сведения можно использовать для устранения неполадок сбоям сборок и решить проблемы, прежде чем другой сборки.</span><span class="sxs-lookup"><span data-stu-id="f028b-188">You can use this information to troubleshoot failed builds and address any problems before you attempt another build.</span></span>

> [!NOTE]
> <span data-ttu-id="f028b-189">Сборки, выполнения логики развертывания, могут завершаться сбоем, пока сервер сборки был предоставлен все разрешения, необходимые в целевой среде.</span><span class="sxs-lookup"><span data-stu-id="f028b-189">Builds that execute deployment logic are likely to fail until you have granted the build server any permissions required in the destination environment.</span></span> <span data-ttu-id="f028b-190">Дополнительные сведения см. в разделе [Настройка разрешений для развертывания сборки Team](configuring-permissions-for-team-build-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="f028b-190">For more information, see [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md).</span></span>


## <a name="monitor-the-build-process"></a><span data-ttu-id="f028b-191">Монитор процесса построения</span><span class="sxs-lookup"><span data-stu-id="f028b-191">Monitor the Build Process</span></span>

<span data-ttu-id="f028b-192">TFS предоставляет широкий набор функций для мониторинга процесса построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-192">TFS provides a broad range of functionality to help you monitor the build process.</span></span> <span data-ttu-id="f028b-193">Например TFS можно отправить вам сообщение электронной почты или отображения предупреждения в области уведомлений на панели задач после завершения построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-193">For example, TFS can send you an email or display alerts in your taskbar notification area when a build has completed.</span></span> <span data-ttu-id="f028b-194">Дополнительные сведения см. в разделе [выполнения и отслеживания построений](https://msdn.microsoft.com/en-us/library/ms181721.aspx).</span><span class="sxs-lookup"><span data-stu-id="f028b-194">For more information, see [Run and Monitor Builds](https://msdn.microsoft.com/en-us/library/ms181721.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="f028b-195">Заключение</span><span class="sxs-lookup"><span data-stu-id="f028b-195">Conclusion</span></span>

<span data-ttu-id="f028b-196">В этом разделе описано, как создать определение сборки в TFS.</span><span class="sxs-lookup"><span data-stu-id="f028b-196">This topic described how to create a build definition in TFS.</span></span> <span data-ttu-id="f028b-197">Определение сборки настроен для элементов конфигурации, поэтому каждый раз, когда разработчик возвращает в содержимом для командного проекта запускается процесс построения.</span><span class="sxs-lookup"><span data-stu-id="f028b-197">The build definition is configured for CI, so the build process runs whenever a developer checks in content to the team project.</span></span> <span data-ttu-id="f028b-198">Определение построения выполняет пользовательского файла проекта MSBuild для развертывания веб-пакетов и скриптов базы данных сервера к целевой среде.</span><span class="sxs-lookup"><span data-stu-id="f028b-198">The build definition executes a custom MSBuild project file to deploy web packages and database scripts to a target server environment.</span></span>

<span data-ttu-id="f028b-199">В порядке для автоматизированного развертывания для успешного выполнения в рамках процесса построения необходимо будет предоставить соответствующие разрешения для учетной записи службы построения на целевой веб-серверов и целевым сервером базы данных.</span><span class="sxs-lookup"><span data-stu-id="f028b-199">In order for an automated deployment to succeed as part of a build process, you'll need to grant appropriate permissions to the build service account on the target web servers and the target database server.</span></span> <span data-ttu-id="f028b-200">В последнем разделе этого учебника [Настройка разрешений для развертывания сборки Team](configuring-permissions-for-team-build-deployment.md), описывает, как определить и настроить разрешения, необходимые для автоматического развертывания с сервера Team Build.</span><span class="sxs-lookup"><span data-stu-id="f028b-200">The final topic in this tutorial, [Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md), describes how to identify and configure the permissions required for automated deployment from a Team Build server.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f028b-201">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="f028b-201">Further Reading</span></span>

<span data-ttu-id="f028b-202">Дополнительные сведения о создании определений сборки см. в разделе [создание базового определения сборки](https://msdn.microsoft.com/en-us/library/ms181716.aspx) и [определить процедуру сборки](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span><span class="sxs-lookup"><span data-stu-id="f028b-202">For more information on creating build definitions, see [Create a Basic Build Definition](https://msdn.microsoft.com/en-us/library/ms181716.aspx) and [Define Your Build Process](https://msdn.microsoft.com/en-us/library/ms181715.aspx).</span></span> <span data-ttu-id="f028b-203">Дополнительные рекомендации по очереди сборок см. в разделе [очередь сборку](https://msdn.microsoft.com/en-us/library/ms181722.aspx).</span><span class="sxs-lookup"><span data-stu-id="f028b-203">For more guidance on queuing builds, see [Queue a Build](https://msdn.microsoft.com/en-us/library/ms181722.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f028b-204">[Назад](configuring-a-tfs-build-server-for-web-deployment.md)
[Вперед](deploying-a-specific-build.md)</span><span class="sxs-lookup"><span data-stu-id="f028b-204">[Previous](configuring-a-tfs-build-server-for-web-deployment.md)
[Next](deploying-a-specific-build.md)</span></span>