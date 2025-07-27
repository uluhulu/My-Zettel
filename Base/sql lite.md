2025-07-171607
Tags: #
__
**Drift** (ранее назывался **moor**) — это **библиотека для работы с базой данных в Flutter**, основанная на **SQLite**, с мощной системой типов, автогенерацией кода и реактивной архитектурой.

Если ты хочешь **локальную базу данных в Flutter** с возможностью писать SQL-запросы и получать данные как стримы — **Drift один из лучших выборов**.

---

### 🚀 Основные возможности Drift

|Возможность|Описание|
|---|---|
|**Использует SQLite**|Под капотом — SQLite. Работает на Android, iOS, macOS, Windows, Linux и web.|
|**Типизированные запросы**|SQL-запросы компилируются в Dart-код с проверкой типов.|
|**Реактивность**|Можно подписаться на изменения данных (`Stream`-подход).|
|**Поддержка SQL и Dart API**|Можно писать как обычные SQL-запросы, так и использовать Dart-синтаксис.|
|**Поддержка миграций**|Позволяет обновлять структуру БД без потери данных.|
|**Работает с Isolate’ами**|Поддерживает выполнение запросов в изолятах (важно для производительности).|

---

### 📦 Установка (в `pubspec.yaml`):

```yaml
dependencies:
  drift: ^2.15.0
  sqlite3_flutter_libs: ^0.5.0

dev_dependencies:
  drift_dev: ^2.15.0
  build_runner: ^2.4.0
```

---

### 🛠️ Простой пример

#### 1. Создание таблицы

```dart
import 'package:drift/drift.dart';

class Todos extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get title => text().withLength(min: 1, max: 50)();
  BoolColumn get completed => boolean().withDefault(const Constant(false))();
}
```

#### 2. Создание базы данных

```dart
import 'package:drift/drift.dart';
import 'package:drift/native.dart';
import 'dart:io';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart' as p;

part 'app_database.g.dart';

@DriftDatabase(tables: [Todos])
class AppDatabase extends _$AppDatabase {
  AppDatabase() : super(_openConnection());

  @override
  int get schemaVersion => 1;

  // Пример DAO-функции
  Future<List<Todo>> getAllTodos() => select(todos).get();
  Stream<List<Todo>> watchTodos() => select(todos).watch();
  Future insertTodo(Todo todo) => into(todos).insert(todo);
}

LazyDatabase _openConnection() {
  return LazyDatabase(() async {
    final dbFolder = await getApplicationDocumentsDirectory();
    final file = File(p.join(dbFolder.path, 'db.sqlite'));
    return NativeDatabase(file);
  });
}
```

Запускаешь `flutter pub run build_runner build` — и генерируется `app_database.g.dart`.

---

### 🔁 Работа с Drift

```dart
final db = AppDatabase();

// Добавить задачу
await db.insertTodo(TodosCompanion(
  title: Value('Почистить проект'),
));

// Подписаться на изменения
db.watchTodos().listen((todos) {
  print('Задачи обновлены: $todos');
});
```

---

### 🌐 Drift Web и другие платформы

Drift поддерживает:

- **Flutter mobile и desktop**
    
- **Web** (использует `sql.js`)
    
- **Server (Dart VM)**
    

---

Хочешь, я покажу полный шаблон проекта с Drift + Flutter UI? Или тебя интересует только конкретный момент (например, миграции, поиск, фильтрация)?
__
###Zero-Links
-[[00 Flutter]]
__
###Links
-