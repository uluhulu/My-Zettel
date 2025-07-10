2025-07-031415
Tags: #
__

BlocObserver помогает вам определять текущие состояния ваших BLoC и Cubits и реагировать на изменения состояний. Также BlocObserver работает в отдельной зоне, поэтому если вы хотите реагировать на какие-то изменения с помощью некоторых будущих событий или кэширования в фоновом режиме, я бы предпочел это. Также вы можете создать Isolate в Зоне для ваших сложных вычислений, и не дергать приложение, и вы все еще можете работать на 60 кадрах в секунду.

# **BlocObserver after BloC 8.0**

BlocObserver претерпел множество изменений. Во-первых, теперь это абстрактный класс, поэтому его нельзя инстанцировать.

Главное, теперь мы можем использовать несколько Observer. Это улучшает качество, надежность и читаемость нашего кода и дает нам больше гибкости для выполнения этих действий с определенными Bloc. Даже изменять Observer во время выполнения, что действительно здорово.
Мы также можем вкладывать Observer в Observer. Так что один Bloc может отправлять данные в AppBlocObserver1, в то время как другие Bloc могут отправлять данные в AppBlocObserver2.
Я покажу вам, как это весело, но я не могу дать вам реальную жизненную задачу, где я бы вкладывал их друг в друга. (Если кто-то может, пожалуйста, прокомментируйте)

```
void main() {  
BlocOverrides.runZoned(  
() {  
**_/// We can get the current Observer like this_**final overrides = BlocOverrides.current;  
**_/// Blocs in this zone report to BlocObserverA  
_**BlocOverrides.runZoned(  
() {  
final overrides = BlocOverrides.current;  
**_/// Blocs in this zone report to BlocObserverB_**},  
blocObserver: BlocObserverB(),  
);  
  
},  
blocObserver: BlocObserverA(),  
);  
}
```


Также есть небольшое изменение. Теперь мы можем вызывать ошибки из BlocBase и это не будет угрожать как неперехваченное исключение, но наш метод onError выведет его на консоль. Вот небольшой пример.

```
@override  
void onCreate(BlocBase bloc) {  
super.onCreate(bloc);  
if (bloc is Cubit) {  
bloc.addError('Why do we use a Cubit, We should use Bloc');  
} else {  
print("This is a Bloc");  
}  
}
```



__
###Zero-Links
-
__
###Links
-