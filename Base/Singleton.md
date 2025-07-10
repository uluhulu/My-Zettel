2025-06-251346
Tags: #
__
```
static final BottomNavigationController _instance = BottomNavigationController._internal();

factory BottomNavigationController() {

return _instance;

}
```

Этот код реализует паттер синглтон. Код гарантирует что только один экземпляр может существовать на протяжении всего жизненного цикла приложения. 
```
**`static final BottomNavigationController _instance = BottomNavigationController._internal();`**
```
Создается единственный статический экземпляр контроллера. 
Этот экземпляр создается единожды при первом обращении к классу

**`factory BottomNavigationController()`**

Фабричный конструктор, который служит публичной точкой входа 
**`BottomNavigationController._internal()`**

Приватный именованный конструктор. 
Предотвращает создание новых экземпляров внешним кодом. 
__
###Zero-Links
-
__
###Links
-