2025-06-211350
Tags: #
__
Архитектура завязана на навигации. На навигации завязано все : состояние приложения, синхронизация с платформой и восстановление состояния. Так же можно изменять навигацию со стороны платформы (изменение юрл в браузере). Так же диплинки, на навигации завязаны экраны а на экранах жизненный цикл фич. Так же есть различные гуарды (например связь с аутентификационным статусом пользователя). 
Навигация и роутинг это разные вещи. Во Flutter 2 различных объекта : Navigator. Он отвечает за то, какие странички, какие экранчики вы видете перед собой. . То есть Navigator по сути это работа через Overlay. Overlay - это колода карт. Router - отдельный объект. Его задача это двухсторонняя связь с платформой. 

**Navigator** — самое доступное решение во Flutter для навигации, с которым прежде всего знакомятся начинающие разработчики. Navigator — это stateful-виджет, создаваемый внутри MaterialApp/CupertinoApp. State данного виджета содержит текущий стек навигации и предоставляет методы для изменения этого состояния.

Простой переход на новую страницу выглядит следующим образом:

```
Navigator.push(	context, 	MaterialPageRoute(builder: (context) => SecondPage()),);
```

В примере используется MaterialPageRoute, который реализует интерфейс Route.

Многие приложения имеют навигатор в верхней части иерархии виджетов, чтобы отображать свою логическую историю с помощью Overlay с недавно посещенными страницами визуально поверх старых страниц. Использование этого шаблона позволяет навигатору визуально переходить с одной страницы на другую, перемещая виджеты в наложении. Аналогичным образом, навигатор можно использовать для отображение диалогового окна, расположив виджет диалогового окна над текущей страницей.

Мобильные приложения обычно раскрывают свое содержимое через полноэкранные элементы, называемые "экранами" или "страницами". В Flutter эти элементы называются маршрутами и управляются виджетом [Navigator](https://api.flutter.dev/flutter/widgets/Navigator-class.html). Навигатор управляет стеком объектов [Route](https://api.flutter.dev/flutter/widgets/Route-class.html) и предоставляет два способа управления стеком: декларативный APINavigator[.pages](https://api.flutter.dev/flutter/widgets/Navigator/pages.html) или императивный API [Navigator.push](https://api.flutter.dev/flutter/widgets/Navigator/push.html) и [Navigator.pop](https://api.flutter.dev/flutter/widgets/Navigator/pop.html).

Навигатор [](https://api.flutter.dev/flutter/widgets/Navigator-class.html)преобразует свои [страницы Navigator](https://api.flutter.dev/flutter/widgets/Navigator/pages.html) в стек Route. Изменение в [Navigator.pages](https://api.flutter.dev/flutter/widgets/Navigator/pages.html) вызовет обновление стека Route. Навигатор обновит свои маршруты в соответствии с новой конфигурацией [Navigator.pages](https://api.flutter.dev/flutter/widgets/Navigator/pages.html). Чтобы использовать этот API, можно создать подкласс [Page](https://api.flutter.dev/flutter/widgets/Page-class.html) и определить список страниц для [Navigator.pages](https://api.flutter.dev/flutter/widgets/Navigator/pages.html). Обратный вызов [Navigator.onPopPage](https://api.flutter.dev/flutter/widgets/Navigator/onPopPage.html) также требуется для правильной очистки входных страниц в случае всплывания.

Страницы преобразуются в маршруты с помощью [Page.createRoute](https://api.flutter.dev/flutter/widgets/Page/createRoute.html) аналогичным способу превращения [виджетов](https://api.flutter.dev/flutter/widgets/Widget-class.html) в элементы (и состояния или [RenderObjects](https://api.flutter.dev/flutter/rendering/RenderObject-class.html)) с помощью [Widget.createElement](https://api.flutter.dev/flutter/widgets/Widget/createElement.html) (и [StatefulWidget.createState](https://api.flutter.dev/flutter/widgets/StatefulWidget/createState.html) или [RenderObjectWidget.createRenderObject](https://api.flutter.dev/flutter/widgets/RenderObjectWidget/createRenderObject.html)).

При обновлении этого списка новый список сравнивается с предыдущим списком, и набор маршрутов обновляется соответствующим образом.

Некоторые [маршруты](https://api.flutter.dev/flutter/widgets/Route-class.html) не соответствуют объектам [страницы](https://api.flutter.dev/flutter/widgets/Page-class.html), а именно тем, которые добавляются в историю с помощью [Navigator](https://api.flutter.dev/flutter/widgets/Navigator-class.html) API ([push](https://api.flutter.dev/flutter/widgets/Navigator/push.html) и friends). [Маршрут](https://api.flutter.dev/flutter/widgets/Route-class.html), который не соответствует объекту [страницы](https://api.flutter.dev/flutter/widgets/Page-class.html), называется маршрутом без страницы и привязан к [маршруту](https://api.flutter.dev/flutter/widgets/Route-class.html), который _соответствует_ объекту [страницы](https://api.flutter.dev/flutter/widgets/Page-class.html), который находится под ним в истории.

[MaterialApp](https://api.flutter.dev/flutter/material/MaterialApp-class.html) - это самый простой способ настройки. Домашняя страница [MaterialApp](https://api.flutter.dev/flutter/material/MaterialApp-class.html) становится маршрутом в нижней части стека [Navigator](https://api.flutter.dev/flutter/widgets/Navigator-class.html). Это то, что вы видите при запуске приложения.

```
void main() { runApp(const MaterialApp(home: MyAppHome())); }
```

## Использование Навигатора
The `Navigator` widget displays screens as a stack using the correct transition animations for the target platform. To navigate to a new screen, access the `Navigator` through the route's `BuildContext` and call imperative methods such as `push()` `or pop()`:

```
child: const Text('Open second screen'), onPressed: () { Navigator.of(context).push( MaterialPageRoute(builder: (context) => const SecondScreen()), ); },
```

Because `Navigator` keeps a stack of `Route` objects (representing the history stack), The `push()` method also takes a `Route` object. The `MaterialPageRoute` object is a subclass of `Route` that specifies the transition animations for Material Design. For more examples of how to use the `Navigator`, follow the [navigation recipes](https://docs.flutter.dev/cookbook#navigation) from the Flutter Cookbook or visit the [Navigator API documentation](https://api.flutter.dev/flutter/widgets/Navigator-class.html).

## Использование названных маршрутов
Приложения с простой навигацией и требованиями к глубоким ссылкам могут использовать Navigator для навигации и параметр MaterialApp.routes для глубоких ссылок:

```
child: const Text('Open second screen'), 
onPressed: () { Navigator.pushNamed(context, '/second'); },
```

## Ограничения

- у Navigator вызывается pushNamed со ссылкой в качестве параметра;
    
- Navigator вызывает onGenerateRoute, чтобы получить из него Route;
    
- полученный Route добавляется в стек навигации.
На первый взгляд проблема решена, но есть и нюанс — при переходе по ссылке в стек навигации всегда будет добавлен только один экран, потому что onGenerateRoute может вернуть только один Route. Не предполагается, что deeplink может инициировать добавление сразу нескольких страниц.

Хотя именованные маршруты могут обрабатывать глубокие ссылки, поведение всегда одинаково и не может быть настроено. Когда платформа получает новую глубокую ссылку, Flutter помещает новый маршрут в Navigator независимо от того, где в данный момент находится пользователь.

Flutter также не поддерживает кнопку браузера «вперед» для приложений, использующих именованные маршруты. По этим причинам мы не рекомендуем использовать именованные маршруты в большинстве приложений.

## Использование Router

Приложения Flutter с расширенными требованиями к навигации и маршрутизации (например, веб-приложение, использующее прямые ссылки на каждый экран, или приложение с несколькими виджетами Navigator) должны использовать пакет маршрутизации, такой как go_router, который может анализировать путь маршрута и настраивать Navigator всякий раз, когда приложение получает новую глубокую ссылку.

Чтобы использовать Router, переключитесь на конструктор маршрутизатора в MaterialApp или CupertinoApp и предоставьте ему конфигурацию Router. Пакеты маршрутизации, такие как go_router, обычно предоставляют конфигурацию маршрута, и маршруты можно использовать следующим образом:

```
child: const Text('Open second screen'),
onPressed: () => context.go('/second'),
```

Поскольку такие пакеты, как go_router, являются декларативными, они всегда будут отображать один и тот же экран (экраны) при получении глубокой ссылки.

__
###Zero-Links
-[[00 Navigation]]
__
###Links
-