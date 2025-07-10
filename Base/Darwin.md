2025-07-071518
Tags: #
__
**Darwin-based operating systems** - это операционные системы, основанные на ядре **Darwin**

## Что такое Darwin:

**Darwin** - это **открытое ядро операционной системы**, разработанное Apple. Это основа для операционных систем Apple.

## Какие системы основаны на Darwin:

### 1. **iOS** 📱

- iPhone операционная система
- iPad операционная система (iPadOS)

### 2. **macOS** 💻

- Операционная система для Mac компьютеров
- Ранее называлась Mac OS X, затем OS X

### 3. **watchOS** ⌚

- Операционная система для Apple Watch

### 4. **tvOS** 📺
- Операционная система для Apple TV

### 5. **visionOS** 🥽

- Операционная система для Apple Vision Pro

## Почему это важно в контексте flutter_local_notifications:

### **Общие API и поведение:**

```
/// Plugin initialization settings for Darwin-based operating systems
/// such as iOS and macOS
class DarwinInitializationSettings {
  // Настройки работают для всех Darwin-систем
}
```

### **Единый подход к уведомлениям:**

```
// Эти настройки применяются к:

// - iPhone (iOS)

// - iPad (iPadOS)

// - Mac (macOS)

// - Apple Watch (watchOS)

// - Apple TV (tvOS)
```

## Практические примеры:

### **iOS и macOS используют одни API:**

```
const DarwinInitializationSettings(

requestAlertPermission: true, // Работает на iOS и macOS

requestSoundPermission: true, // Работает на iOS и macOS

requestBadgePermission: true, // Работает на iOS и macOS

defaultPresentAlert: true, // Работает на iOS и macOS

defaultPresentSound: true, // Работает на iOS и macOS

defaultPresentBadge: true, // Работает на iOS и macOS

);
```

### **Версионная совместимость:**

```
/// On iOS, this property is only applicable to iOS 10 or newer.

/// On macOS, this property is only applicable to macOS 10.14 or newer.

final bool defaultPresentSound;
```

## Противоположность - Android:
```
// Android использует свои настройки

AndroidInitializationSettings('@mipmap/ic_notification');

  

// Darwin (iOS/macOS) использует свои настройки

DarwinInitializationSettings(

requestAlertPermission: true,

requestSoundPermission: true,

);
```
## В коде инициализации:

```
Future<void> _setupFlutterNotifications() async {

// Настройки для Android

const AndroidInitializationSettings initializationSettingsAndroid =

AndroidInitializationSettings('@mipmap/ic_notification');

// Настройки для Darwin (iOS/macOS)

const DarwinInitializationSettings initializationSettingsDarwin =

DarwinInitializationSettings(

requestAlertPermission: true,

requestSoundPermission: true,

requestBadgePermission: true,

);

// Объединение настроек

const InitializationSettings initializationSettings = InitializationSettings(

android: initializationSettingsAndroid,

iOS: initializationSettingsDarwin, // iOS

macOS: initializationSettingsDarwin, // macOS

);

}
```

## Почему Darwin важен для разработчиков:

### ✅ **Единый код для Apple платформ:**
```
/ Один класс настроек для всех Apple устройств

DarwinInitializationSettings settings = DarwinInitializationSettings(...);
```
### ✅ **Консистентное поведение:**

```
// Уведомления работают одинаково на iPhone и Mac
```

### ✅ **Упрощение разработки:**

```
// Не нужно отдельно настраивать для каждой Apple платформы
```

## Эволюция названий:

- **Mac OS X** (2001-2012)
- **OS X** (2012-2016)
- **macOS** (2016-сейчас)
- **iOS** (2007-сейчас)
- **Все основаны на Darwin**

**Итог:** **Darwin-based operating systems** = **все операционные системы Apple** (iOS, macOS, watchOS, tvOS, visionOS), которые используют общее ядро Darwin и имеют схожие API для уведомлений.
__
###Zero-Links
-[[00 Mobile]]
__
###Links
-