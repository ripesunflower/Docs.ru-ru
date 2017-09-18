## <a name="register-the-database-context"></a><span data-ttu-id="eca60-101">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="eca60-101">Register the database context</span></span>

<span data-ttu-id="eca60-102">Чтобы внедрить контекст базы данных в контроллер, необходимо зарегистрировать его посредством контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="eca60-102">In order to inject the database context into the controller, we need to register it with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="eca60-103">Зарегистрируйте контекст базы данных в контейнере службы с помощью встроенной поддержки [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="eca60-103">Register the database context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="eca60-104">Замените содержимое файла *Startup.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="eca60-104">Replace the contents of the *Startup.cs* file with the following:</span></span>

<span data-ttu-id="eca60-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span><span class="sxs-lookup"><span data-stu-id="eca60-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span></span>

<span data-ttu-id="eca60-106">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="eca60-106">The preceding code:</span></span>

* <span data-ttu-id="eca60-107">Удаляет неиспользуемый код.</span><span class="sxs-lookup"><span data-stu-id="eca60-107">Removes the code we're not using.</span></span>
* <span data-ttu-id="eca60-108">Указывает базу данных в памяти, которая внедряется в контейнер службы.</span><span class="sxs-lookup"><span data-stu-id="eca60-108">Specifies an in-memory database is injected into the service container.</span></span>