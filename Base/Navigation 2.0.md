2025-06-230959
Tags: #
__
 Основные сущности Navigator 2.0
 
 ### **Router**
 Виджет, который отвечает за образование стека страниц `Page` навигатора в зависимости от состояния приложения.
 
 Чтобы воспользоваться `Router` в проекте, мы можем вызвать соответствующий именованный конструктор `MaterialApp.router`:

void main() {
  runApp(SomeApp());
}

```
class SomeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    /// Вместо MaterialApp вы также можете использовать
    /// CupertinoApp и WidgetApp
    return MaterialApp.router(
      /// Об этих параметрах речь пойдёт далее
      routerDelegate: ...,
      routeInformationParser: ...,
      routeInformationProvider: ...,
      backButtonDispatcher: ...,
      ...
    );
  }
}

```
или виджет `Router`, если хотим создать поддерево навигации:
```
SomeWidget(
child: Router( 
routerDelegate: ...,
routeInformationParser: ...,
routeInformationProvider: ..., 
backButtonDispatcher: ..., ), ),
```

### **BackButtonDispatcher**

Класс, сообщающий `Router` через коллбэк, что пользователь нажал на системную кнопку «Назад» на платформах, поддерживающих данную функциональность (например, Android OS). При вызове коллбэка `Router` должен передать сообщение в `RouterDelegate` на обработку и вернуть в `BackButtonDispatcher` результат этой обработки.

Сам параметр `backButtonDispatcher` не является обязательным и может быть опущен. По умолчанию используется класс `RootBackButtonDispatcher`, который прослушивает ивенты закрытия `pop()` от платформы.

Если в приложении есть необходимость использовать несколько диспетчеров, можно использовать `ChildBackButtonDispatcher`. Данный диспетчер слушает уведомления от родительского диспетчера и может:

- перехватывать контроль над обработкой событий (`takePriority`);
    
- делегировать контроль на дочерний диспетчер (`deferTo`).
    

Наличие диспетчера не гарантирует доступность действия «назад». Должны также соблюдаться следующие условия:

- для мобильных платформ — наличие в `pages` больше одной страницы;
    
- для веба — наличие в истории браузера предыдущих состояний.

### **RouteInformationProvider**

Провайдер путей навигации для виджета `Router`. Этот класс — наследник `ValueListenable`, в `value` которого лежит информация о поступившем пути. `Router`  использует `value` для самого первого построения навигационного стека и далее при работе приложения, передавая информацию на обработку в `RouteInformationParser`.

Параметр `routeInformationProvider` не обязателен, и в случае с `MaterialApp.router` по умолчанию будет использоваться `PlatformRouteInformationProvider`. Данная реализация предоставляет в `Router` информацию навигации от платформы (например, диплинки), а также информацию о новых путях от `Router` обратно в Flutter Engine.

При попытке создать экземпляр `PlatformRouteInformationProvider` вручную мы столкнёмся с его обязательным параметром — `initialRouteInformation`. Сам параметр — это объект класса `RouteInformation`, то есть информация о пути.

Она состоит из строки местоположения приложения (location) и объекта состояния, который конфигурирует приложение в этом месте. `RouterInformationProvider` и `Router` передают данный объект друг другу при взаимодействии. Также класс может использоваться для сохранения состояния навигации при закрытии приложения.

### **RouteInformationParser**

Компонент, позволяющий `Router` парсить поступающую от `RouteInformationProvider`
информацию о пути в состояние (конфигурацию) навигации. Вся информация, поступающая от `RouteInformationProvider` в `Router`, предоставляется парсером через метод `parseRouteInformation()`.

И наоборот, из `Router` в `RouteInformationProvider` через метод `restoreRouteInformation()`. Оба метода решают проблемы с навигацией по диплинкам и отображением корректного URL в веб-приложении. 

Параметр `routeInformationParser` не является обязательным и может быть опущен.

### **RouterDelegate**

Класс отвечает за то, как именно `Router` узнаёт об изменениях состояния приложения и реагирует на них. Для этого он прослушивает `RouteInformationParser` и состояние навигации, а затем встраивает в дерево виджет `Navigator` с готовым стеком страниц.

Рассмотрим стандартную реализацию данного класса:

```
class SomeAppRouterDelegate extends RouterDelegate<SomeAppRouteConfiguration>{
  @override
  void addListener(VoidCallback listener) {
    // TODO: implement addListener
  }

  @override
  Widget build(BuildContext context)
    // TODO: implement build 
    throw UnimplementedError();
  }

  @override
  Future<bool> popRoute() {
    // TODO: implement popRoute
    throw UnimplementedError();
  }

  @override
  void removeListener(VoidCallback listener) {
    // TODO: implement removeListener
  }

  @override
  Future<void> setNewRoutePath(SomeAppRouteConfiguration configuration) {
    // TODO: implement removeListener
    throw UnimplementedError();
  }
}



```

Класс необходимо типизировать. В качестве примера мы использовали придуманное название SomeAppRouteConfiguration. В реальном приложении этот тип должен описывать состояние навигации, с которым необходимо оперировать для построения стека страниц.

Далее мы приведём пример такого состояния.

Разберём методы, которые необходимо переопределить в данном классе:

- Методы `addListener()` и `removeListener()` предназначены для того, чтобы регистрировать прослушивание уведомлений со стороны `Router` и в дальнейшем сообщать ему об изменении состояния навигации. В большинстве случаев для простоты достаточно подмешать в класс `ChangeNotifier` — и необходимость в ручной реализации этих методов отпадёт.

```
class SomeAppRouterDelegate extends RouterDelegate<SomeAppRouteConfiguration> with ChangeNotifier {
  ...
}

```

- Метод `setNewRoutePath()` вызывается самим `Router`, когда в него поступает новая конфигурация от `RouteInformationParser` после обработки полученной из `RouteInformationProvider` информации. Метод нужен для реакции самого делегата на такое событие.

- `build()` — возвращает виджет `Navigator` с готовым списком страниц в зависимости от текущего состояния. При сообщениях от делегата `Router` вызывает данный метод. Далее полученный в результате выполнения `Navigator` встраивается в дерево виджетов.
- `popRoute()` — вызывается самим `Router` в тот момент, когда `BackButtonDispatcher` сообщает, что операционная система запрашивает закрытие текущего пути. Сам метод должен возвращать `Future<bool>` значение и указывать, обработал ли делегат запрос. Возврат `false` приведёт к сворачиванию всего приложения. Мы также можем избавиться от необходимости имплементировать данный метод, подмешав в наш делегат миксин `PopNavigatorRouterDelegateMixin<T>`. Данный миксин сам вызывает `Navigator.maybePop()`, но для успешной работы мы обязаны переопределить параметр `navigatorKey`, который будет идентифицировать наш встраиваемый виджет `Navigator`.

```
class SomeAppRouterDelegate extends RouterDelegate<SomeAppRouteConfiguration> with ChangeNotifier, PopNavigatorRouterDelegateMixin<SomeAppRouteConfiguration> {
  @override
  final GlobalKey<NavigatorState> navigatorKey;

  @override
  Widget build(BuildContext context) => Navigator(
     key: navigatorKey,
     ...
  );
}

```

- Также стоит упомянуть о необязательном для реализации методе `setInitialRoutePath()`. Этот метод вызывается виджетом `Router` после получения информации о первоначальном пути `initalRouteInformation`.
    

Таким образом, данный класс является основой навигации при использовании `Router`. Переопределяя перечисленные методы, мы задаём управление навигацией в приложении.

### **Взаимодействие компонент**

Разобрав основные компоненты представленного API Navigator 2.0, рассмотрим схему их взаимодействия.

![[5_7_1_920d72d0ba.svg]]

Какие выводы тут можно сделать:

- `AppState` — состояние приложения (фичи/компоненты/сущности), которое определяет стек навигации.
    
- `BackButtonDispatcher`, `RouteInformationProvider` и `RouteInformationParser` нужны для общения с платформой (в общем случае).
    
- Можно провести аналогию с виджетами во Flutter: `Router` — это `StatefulWidget`, `RouterDelegate` — `State` этого виджета.

### **RouterConfig**

Это интерфейс, который позволяет инкапсулировать создание раннее перечисленных компонент в единую сущность и передавать их в `Router` как один объект.

__
###Zero-Links
-[[00 Flutter]]
__
###Links
-