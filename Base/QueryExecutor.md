2025-07-291818
Tags: #
__
**`QueryExecutor`** - это абстрактный интерфейс в Drift ORM, который определяет способ выполнения sql запросов к базе данных. Это слой абстракции между Drift и конкретной реализацией базы данных. 

## Платформо-специфичные реализации:

### **1. Нативные платформы (Android/iOS):**

```
// db_executor_native.dart
QueryExecutor createExecutor() {
  return LazyDatabase(() async {
    final file = File(path.join(dbFolder.path, 'small.sqlite'));
    return NativeDatabase.createInBackground(file); // ← Реализует QueryExecutor
  });
}
```

**Используется:** `NativeDatabase` (FFI + SQLite)

### **2. Веб-платформа:**

```
// db_executor_web.dart

QueryExecutor createExecutor() => DatabaseConnection.delayed(

Future(() async {

final result = await WasmDatabase.open(...);

return result.resolvedExecutor; // ← Реализует QueryExecutor

}),

);
```

**Используется:** `WasmDatabase` (WebAssembly + IndexedDB)

### **3. Тестирование:**

```
// Для тестов

QueryExecutor createTestExecutor() {

return NativeDatabase.memory(); // ← In-memory SQLite

}
```

## Как это работает в AppDatabase:

### **Создание подключения:**

```
@DriftDatabase(tables: [RequestLogsTable])

class AppDatabase extends _$AppDatabase {

AppDatabase([QueryExecutor? executor]) : super(executor ?? createExecutor());

// ↑

// Получает QueryExecutor

}
```
### **Выполнение запросов через Drift:**

```
// Когда вы пишете:

final logs = await database.select(database.requestLogsTable).get();

  

// Drift генерирует SQL и вызывает:

executor.runSelect(

'SELECT * FROM request_logs_table',

[]

);
```


## Типы QueryExecutor в Drift:

### **1. NativeDatabase (мобильные/десктоп):**

```
// Прямое подключение к SQLite файлу

final executor = NativeDatabase(File('database.sqlite'));

  

// Background isolate для лучшей производительности

final executor = NativeDatabase.createInBackground(File('database.sqlite'));

  

// In-memory база для тестов

final executor = NativeDatabase.memory();
```

### **2. WasmDatabase (веб):**

```
// WebAssembly SQLite в браузере

final result = await WasmDatabase.open(

databaseName: 'db.sqlite',

sqlite3Uri: Uri.parse('/sqlite3.wasm'),

);

final executor = result.resolvedExecutor;
```

### **3. WebDatabase (веб, устаревший):**

```
// Устаревший способ для веб (sql.js)

final executor = WebDatabase('database_name');
```

### **4. LazyDatabase (обертка):**

```
// Ленивая инициализация любого executor'а

final executor = LazyDatabase(() async {

// Тяжелая инициализация происходит при первом обращении

return NativeDatabase(File('database.sqlite'));

});
```

### **5. DatabaseConnection (продвинутая обертка):**
```
/ Для сложных сценариев подключения

final executor = DatabaseConnection.delayed(

Future(() async {

// Асинхронная инициализация

return await createComplexConnection();

}),

);
```

## Практическое использование:

### **Dependency Injection:**

```
// Можно инжектить разные executor'ы
class AppDatabase extends _$AppDatabase {
  AppDatabase(QueryExecutor executor) : super(executor);
}

// Продакшн
final database = AppDatabase(createExecutor());

// Тесты
final testDatabase = AppDatabase(NativeDatabase.memory());

// Моки
final mockDatabase = AppDatabase(MockQueryExecutor());
```

### **Переключение между платформами:**

```
QueryExecutor createExecutor() {

if (kIsWeb) {

return createWebExecutor();

} else if (Platform.isIOS || Platform.isAndroid) {

return createNativeExecutor();

} else {

throw UnsupportedError('Unsupported platform');

}

}
```

## Преимущества абстракции:

### ✅ **Платформо-независимость:**

```
// Один код AppDatabase работает везде

// Только executor меняется
```

### ✅ **Тестируемость:**

```
// Легко мокать для тестов

class MockQueryExecutor implements QueryExecutor {

@override

Future<List<Map<String, Object?>>> runSelect(String sql, List<Object?> args) async {

return [{'id': 1, 'name': 'test'}];

}

}
```

### ✅ **Гибкость конфигурации:**

```
// Можно настраивать под разные сценарии

// - Разные файлы БД

// - In-memory для тестов

// - Удаленные подключения
```

### ✅ **Изоляция зависимостей:**

```
// AppDatabase не знает как именно выполняются SQL запросы

// Только что должно быть выполнено
```

## Жизненный цикл:

```
createExecutor() → QueryExecutor → AppDatabase → SQL Operations
     ↓                ↓               ↓              ↓
Platform-specific  Abstract      Drift ORM    runSelect()
implementation     interface     generated    runUpdate()
                                 methods      runCustom()
```

## Итог:

```
**`QueryExecutor`** - это **ключевая абстракция** в Drift ORM, которая:

- ✅ **Определяет интерфейс** для выполнения SQL операций
- ✅ **Обеспечивает платформо-независимость** - один код работает везде
- ✅ **Позволяет подменять реализации** - для тестов, разных платформ
- ✅ **Скрывает детали** конкретных баз данных от Drift ORM
- ✅ **Поддерживает различные типы подключений** - файлы, память, веб
```


__
###Zero-Links
-[[00 Flutter]]
-[[00 Drift]]
__
###Links
-