---
{"dg-publish":true,"permalink":"/CS计算机科学/后端开发/Tortoise ORM/","created":"2024-03-16T08:30:13.942+08:00","updated":"2024-03-16T09:16:56.058+08:00"}
---

## 数据库连接和模式生成

使用`Tortoise.init()`和`await Tortoise.generate_schemas()`进行数据库连接和模式生成的方式，与使用`register_tortoise`函数为FastAPI应用注册Tortoise ORM有一些区别。主要区别在于控制级别、集成度、以及使用场景：

### 使用`Tortoise.init()`的方式

1. **直接控制**：通过`Tortoise.init()`直接初始化数据库连接和配置，给开发者提供了更精细的控制能力。你可以在任意位置调用`Tortoise.init()`来初始化数据库连接，这在一些特定场景（如脚本执行或特定任务中）很有用。
2. **手动生成模式**：使用`await Tortoise.generate_schemas()`手动控制何时生成数据库模式。这可以确保在数据库操作之前，所有的表结构都已正确生成。这种方式适用于需要精确控制数据库表生成时机的场景。
3. **灵活性**：这种方式允许在不同的环境或时机下灵活地初始化数据库。例如，在FastAPI应用之外的异步脚本中使用Tortoise ORM。
4. **缺少自动化**：相较于`register_tortoise`，这种方式缺少了一些自动化处理，如自动关闭数据库连接等。开发者需要更加注意资源管理和错误处理。

### 使用`register_tortoise`的方式

`register_tortoise_app(app)`函数是在FastAPI应用中注册Tortoise ORM的关键步骤，它起到几个重要的作用和机制：

1. **数据库连接和配置的绑定**：通过这个函数调用，Tortoise ORM 的配置信息（包括数据库连接信息和模型位置等）会被绑定到FastAPI应用上。这样，当应用启动和停止时，Tortoise ORM可以自动地处理数据库连接的开启和关闭。
2. **自动生成数据库模式**：如果配置了`generate_schemas=True`，Tortoise ORM会在应用启动时自动根据定义的模型生成数据库表结构。这意味着你不需要手动运行迁移脚本来创建或更新数据库表，Tortoise ORM会处理这些工作。
3. **异常处理器的添加**：通过设置`add_exception_handlers=True`，Tortoise ORM会向FastAPI应用添加一些异常处理器，这些处理器能够捕获数据库操作中可能出现的异常，并以HTTP异常的形式返回给客户端。这样可以提高应用的健壮性，并且减少了开发者处理常见数据库错误的工作量。
4. **中间件的集成**：虽然不是直接通过`register_tortoise`函数实现的，但Tortoise ORM的工作方式通常涉及到异步数据库操作。在FastAPI中，这需要正确地设置和管理异步上下文。通过注册Tortoise ORM，你可以确保数据库会话和事务在每个请求的生命周期内被正确地处理。
5. **便于项目的扩展和维护**：将Tortoise ORM注册到FastAPI应用中，可以让你的项目结构更加清晰，数据库操作与API逻辑分离，有利于项目的扩展和维护。

`register_tortoise_app(app)`是将Tortoise ORM整合到FastAPI应用的桥梁，确保了数据库操作的正确性、便捷性和高效性。这样的设计让开发者可以更加专注于业务逻辑的实现，而不用担心数据库层面的细节。

优点：

1. **集成度高**：`register_tortoise`函数专为FastAPI设计，它将Tortoise ORM的配置和初始化过程集成到FastAPI的生命周期中。这意味着数据库连接的开启和关闭会自动管理，与FastAPI应用的启动和停止同步。
2. **自动生成数据库模式**：通过配置参数`generate_schemas=True`，可以在应用启动时自动创建或更新数据库模式。这减少了手动维护数据库模式的需要。
3. **异常处理**：通过`add_exception_handlers=True`，自动为应用添加数据库相关的异常处理器。这提高了应用的健壮性和开发效率。
4. **适合Web应用**：这种方式更适合于Web应用的开发场景，尤其是当使用FastAPI作为后端框架时。它简化了配置和管理过程，让开发者可以专注于业务逻辑。


### 总结

直接使用`Tortoise.init()`和`await Tortoise.generate_schemas()`提供了更灵活的数据库操作方式，适合于需要在应用外部或需要精细控制数据库初始化时机的场景。而`register_tortoise`函数则为FastAPI应用提供了一种高度集成的方式来使用Tortoise ORM，简化了配置和管理，适合快速开发Web应用。根据你的具体需求和场景，你可以选择最适合的方式来使用Tortoise ORM。