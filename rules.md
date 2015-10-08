# webix-code-style


# Правила
## 1. Структура приложения

Приложение должно состоять из:

1. index.html - стартовый html файл в вашем приложения, который инклюдит app.js
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
		start:		"/app/start" //ВАЖНО! - будет выполнен файл  /views/start.js
	});
	return app;
});
```

### 1.2 Определение view

views/start

Файл start.js описывает вид стартовой страницы приложения
```js
	//views/start.js
	define([
	"views/right" /// !!! во время инициализации view start.js будет подключен файл view/right.js
	], function(right){
	    return {
	        cols:[
	            { view:"menu" },
	            right ///  сюда отрендерится вьюшка из файла rigth.js 
	        ]
	    };
	});
```
views/right
```js
	//views/right.js
	define([],function(){
	    return {
	        template:"Start page"
	    };
	});
````
Такой модульный подход позволяет уйти от "спагетти" кода, разбить интерфейс приложения на независимые модули, которые позволяют без боли расширять приложение.


### 1.3 Загрузка данных при помощи моделей

Нужно не путать что вьюшка только отображает данные, а хранение и обработку данных берет на себя модель.

```js
	//models/records.js
	define([],function(){
	    var collection = new webix.DataCollection({ 
	        url:"data.php"
	    });    
	
	    return {
	        data: collection
	    };
	});
```	

Для подключения модели во вьюшки используют следующий подход:
```js
	//views/data.js
	define(["models/records"],function(records){
	    var ui = {
	        view:"datatable", autoConfig:true
	    };
	
	    return {
	        $ui: ui,
	        $oninit:function(view){
	            view.parse(records.data);
	        }
	    };
	});
```	

### 1.4 Прежде чем начать разработку приложения
На этом этапе вам следует определить функциональность приложения и разделить его на независимые модули.

### 1.5 Проектирование интерфейса
Основываясь на ТЗ приложения вы должны выбрать webix компоненты и решить, как они будут связаны друг с другом.

Основные "строительные" компоненты, которые могут содержать в себе другие и в целом управлять внешним видом - следующие:
	-Layout
	-Multiview
	-Accordion
	-Scrollview
	-Carousel
Так же существуют огромное количество дата-компонентов (dataview, datatable) и "специальных" (toolbar, menu, calendar etc.), которые решат любую задачу. Для "особых" случаев, когда нет подходящего компонента - можно расширить максимально подходящий из существующих (http://docs.webix.com/desktop__custom_component.html) или использовать сторонний компонент (http://docs.webix.com/desktop__third_party_integration.html)

### 1.6 Главные принципы коддинга на webix

```js 
	webix.ui({
	    view:"...",// тип компонента который создаете
	    id:"...",
	    //другие параметры
	});
```
или 
```js 
	var table = new webix.ui({ //тип компонента который создаете
	    view:"...",
	    id:"...",
	    //другие параметры
	});
```
### 1.7 Советы

Если надо удалить какой-то компонент из отображения (при условии  что он больше не нужен) - используйте метод desctuctor(), который удалит html компонента из DOM и очистит все события компонента.

```js 
	$$('mydataview1').destructor();
```
Для проверки на существование компонента с указанным индификатором - используйте метод exists()
```js
	if(!$$("my_dataview").exists())// -> returns boolean value
	    var dataview = new webix.ui({
	        view:"dataview",
	        id:"my_dataview",
	        ..config
	    });
```

### 1.8 Оптимизация кода
Для повышения читабельности кода и ухода от ошибок, разбивайте конфиги компонентов на отдельные переменные и в дальнейшем ссылайтесь на них.

```js 
	var simple_config1 = ...;
	var simple_config2 = ...;
	var simple_config3 = ...;
	webix.ui({
	    rows:[
	        simple_config1,
	        simple_config2,
	        simple_config3,
	    ]
	});
```
### 1.9 10 функций "помощников"
http://docs.webix.com/helpers__top_ten_helpers.html


### ВАЖНО
Дополнительная информация:
https://www.gitbook.com/book/webix/webix-jet/details 
http://docs.webix.com/tutorials__start_coding.html
