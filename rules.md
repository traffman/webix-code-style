# webix-code-style


# Правила
## 1. Структура приложения

Приложение должно состоять из:

1. index.html - стартовый html файл в вашем приложения, который инклюдит app.js (например: <script type="text/javascript" data-main="app" src="libs/require.js">)
2. app.js - файл, который содержит конфигурацию приложения
3. views - папка, содержащая модули интерфейса элементов
4. models - папка, содержащая модели (например: webix.DataCollection)
5. libs - папка, которая содержит все сторонние модули (например: socket.io)

### 1.1
Всегда должна быть некая точка входа (например файл app.js). Этот файл служит сугубо для того, чтобы инициализировать начальное состояние приложения. 


```js 
	define([
	"libs/core",
], function(core){
	//configuration
	var app = core.create({
		id:			"ExampleApp",
		name:		"ExampleApp",
		version:	"1",
		start:		"/app/home" 
	});
	return app;
});
```

### ВАЖНО
Дополнительная информация https://www.gitbook.com/book/webix/webix-jet/details
