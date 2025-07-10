2025-07-091005
Tags: #
__
### F-bounded Polymorphism в Dart

**F-bound (F-bounded) polymorphism** — это техника, используемая в обобщённом (generic) программировании, при которой **обобщённый тип параметризуется своим же подклассом**. Название происходит от **F** в математике — когда тип используется как параметр сам для себя.

---

### 📌 Пример F-bounded polymorphism в Dart:

```dart
abstract class Comparable<T> {
  int compareTo(T other);
}

class MyNumber implements Comparable<MyNumber> {
  final int value;

  MyNumber(this.value);

  @override
  int compareTo(MyNumber other) => value - other.value;
}
```

### Как это работает:

- `Comparable<T>` — это интерфейс с обобщением.
    
- `MyNumber implements Comparable<MyNumber>` — это и есть **F-bound**, потому что `MyNumber` реализует `Comparable`, параметризованный самим `MyNumber`.
    

---

### 🧠 Зачем это нужно?

F-bound полезен, когда вы хотите ограничить типы, которые могут участвовать в определённой логике, и гарантировать типобезопасность. Например, при написании сортировщиков, сравнимых объектов, клонируемых объектов и т. п.

---

### 📋 Ещё пример: клон с F-bound

```dart
abstract class Cloneable<T> {
  T clone();
}

class Person implements Cloneable<Person> {
  final String name;

  Person(this.name);

  @override
  Person clone() => Person(name);
}
```

Теперь `clone()` гарантированно вернёт **тот же тип**, что и объект, который его реализует.

---

### Применимость в Dart

- Dart не поддерживает **типовые ограничения с where (T extends SomeType)** так явно, как C# или Java, но через `extends` или `implements` можно реализовать тот же эффект.
    
- Работает во Flutter и обычном Dart-проекте.
    

---

Если хочешь, могу показать, как использовать F-bound с mixin'ами или generic-фабриками.
__
###Zero-Links
-[[00 Dart]]
__
###Links
-