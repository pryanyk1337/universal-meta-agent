# Домен: Программирование на Java

## Описание области
Java — объектно-ориентированный язык программирования общего назначения. Известен принципом "Write Once, Run Anywhere" благодаря JVM. Широко используется в enterprise-разработке, Android, веб-приложениях.

## Ключевые компетенции
- Объектно-ориентированное программирование (ООП)
- Многопоточность и concurrency
- Работа с коллекциями
- Stream API и функциональное программирование
- Exception handling
- JVM и garbage collection
- Фреймворки (Spring, Hibernate)
- Архитектурные паттерны

## Основы Java
### Версии Java:
- **Java 8** (LTS): Lambdas, Stream API
- **Java 11** (LTS): HTTP Client, var
- **Java 17** (LTS): Records, Sealed Classes, Pattern Matching
- **Java 21** (LTS): Virtual Threads, Pattern Matching for switch

### Структура программы:
```java
package com.example;

import java.util.*;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### Типы данных:
```java
// Примитивы
byte, short, int, long
float, double
char
boolean

// Обёртки (Wrapper classes)
Byte, Short, Integer, Long
Float, Double
Character
Boolean

// Autoboxing/Unboxing
Integer obj = 10;     // autoboxing
int num = obj;        // unboxing
```

## Объектно-ориентированное программирование
### Классы и объекты:
```java
public class Person {
    // Поля
    private String name;
    private int age;
    
    // Конструктор
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Методы (Getters/Setters)
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    // Метод
    public void introduce() {
        System.out.println("Hi, I'm " + name);
    }
}

// Использование
Person person = new Person("John", 30);
person.introduce();
```

### Наследование:
```java
public class Employee extends Person {
    private String position;
    
    public Employee(String name, int age, String position) {
        super(name, age);  // Вызов конструктора родителя
        this.position = position;
    }
    
    @Override  // Переопределение метода
    public void introduce() {
        super.introduce();
        System.out.println("I work as " + position);
    }
}
```

### Интерфейсы:
```java
public interface Drawable {
    void draw();  // abstract method
    
    // Default method (Java 8+)
    default void print() {
        System.out.println("Drawing...");
    }
    
    // Static method
    static void info() {
        System.out.println("Drawable interface");
    }
}

public class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing circle");
    }
}
```

### Абстрактные классы:
```java
public abstract class Shape {
    abstract double area();
    
    // Конкретный метод
    public void displayArea() {
        System.out.println("Area: " + area());
    }
}

public class Rectangle extends Shape {
    private double width, height;
    
    @Override
    double area() {
        return width * height;
    }
}
```

## Коллекции (Collections Framework)
### List:
```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
list.get(0);           // "Apple"
list.remove("Apple");
list.size();

// LinkedList
List<Integer> linkedList = new LinkedList<>();
```

### Set:
```java
Set<String> set = new HashSet<>();
set.add("Java");
set.add("Python");
set.add("Java");       // Дубликат, не добавится
set.contains("Java");  // true

// TreeSet (отсортированный)
Set<Integer> treeSet = new TreeSet<>();
```

### Map:
```java
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.get("Alice");      // 25
map.containsKey("Bob");
map.remove("Alice");

// Итерация
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}

// TreeMap, LinkedHashMap
```

### Queue:
```java
Queue<String> queue = new LinkedList<>();
queue.offer("First");
queue.offer("Second");
queue.poll();          // "First"
queue.peek();          // "Second"

// PriorityQueue
Queue<Integer> pq = new PriorityQueue<>();
```

## Stream API (Java 8+)
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

// Фильтрация и маппинг
List<Integer> evenSquares = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * n)
    .collect(Collectors.toList());

// Агрегация
int sum = numbers.stream()
    .reduce(0, Integer::sum);

// Поиск
Optional<Integer> first = numbers.stream()
    .filter(n -> n > 3)
    .findFirst();

// Сортировка
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());

// Группировка
Map<Integer, List<Person>> byAge = people.stream()
    .collect(Collectors.groupingBy(Person::getAge));

// Parallel streams
numbers.parallelStream()
    .forEach(System.out::println);
```

## Lambda выражения
```java
// Синтаксис: (параметры) -> выражение
(int a, int b) -> a + b
x -> x * 2
() -> System.out.println("Hello")

// Functional Interfaces
@FunctionalInterface
interface Operation {
    int apply(int a, int b);
}

Operation add = (a, b) -> a + b;
Operation multiply = (a, b) -> a * b;

// Встроенные функциональные интерфейсы
Predicate<String> isEmpty = s -> s.isEmpty();
Function<String, Integer> length = s -> s.length();
Consumer<String> print = s -> System.out.println(s);
Supplier<Double> random = () -> Math.random();
```

## Исключения (Exceptions)
```java
// Checked exceptions (обязательно обрабатывать)
try {
    FileReader file = new FileReader("file.txt");
    // ...
} catch (FileNotFoundException e) {
    System.err.println("File not found: " + e.getMessage());
} catch (IOException e) {
    e.printStackTrace();
} finally {
    // Выполнится всегда
    // Закрытие ресурсов
}

// Try-with-resources (Java 7+)
try (FileReader file = new FileReader("file.txt")) {
    // Автоматическое закрытие
} catch (IOException e) {
    e.printStackTrace();
}

// Unchecked exceptions (Runtime)
throw new IllegalArgumentException("Invalid input");
throw new NullPointerException("Object is null");

// Создание собственного исключения
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

## Многопоточность
### Thread:
```java
// Способ 1: extends Thread
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running");
    }
}
new MyThread().start();

// Способ 2: implements Runnable
Runnable task = () -> System.out.println("Task running");
new Thread(task).start();

// Способ 3: Callable (с возвращаемым значением)
Callable<Integer> callable = () -> {
    return 42;
};
```

### ExecutorService:
```java
ExecutorService executor = Executors.newFixedThreadPool(5);

// Submit task
Future<Integer> future = executor.submit(() -> {
    Thread.sleep(1000);
    return 123;
});

// Get result (блокирующий)
try {
    Integer result = future.get();
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
}

executor.shutdown();
```

### Синхронизация:
```java
// synchronized метод
public synchronized void increment() {
    counter++;
}

// synchronized блок
public void method() {
    synchronized(this) {
        // критическая секция
    }
}

// Lock (более гибкий)
Lock lock = new ReentrantLock();
lock.lock();
try {
    // критическая секция
} finally {
    lock.unlock();
}

// Atomic classes
AtomicInteger counter = new AtomicInteger(0);
counter.incrementAndGet();
```

## Generics
```java
// Generic класс
public class Box<T> {
    private T value;
    
    public void set(T value) {
        this.value = value;
    }
    
    public T get() {
        return value;
    }
}

Box<Integer> intBox = new Box<>();
Box<String> strBox = new Box<>();

// Generic метод
public <T> T getFirst(List<T> list) {
    return list.isEmpty() ? null : list.get(0);
}

// Wildcards
public void printList(List<?> list) {
    // ? = любой тип
}

public void addNumbers(List<? extends Number> list) {
    // только Number и подклассы
}

public void addIntegers(List<? super Integer> list) {
    // только Integer и суперклассы
}
```

## Аннотации
```java
// Встроенные
@Override
@Deprecated
@SuppressWarnings("unchecked")
@FunctionalInterface

// Создание своей аннотации
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    String value();
    int priority() default 0;
}

// Использование
@MyAnnotation(value = "test", priority = 1)
public void myMethod() {
    // ...
}
```

## Современные возможности Java
### Records (Java 14+):
```java
// Immutable data class
public record Person(String name, int age) {}

// Автоматически генерируются:
// - конструктор
// - getters (name(), age())
// - equals(), hashCode(), toString()

Person person = new Person("Alice", 30);
String name = person.name();
```

### Sealed Classes (Java 17+):
```java
public sealed class Shape 
    permits Circle, Rectangle, Triangle {
}

public final class Circle extends Shape {}
public final class Rectangle extends Shape {}
public non-sealed class Triangle extends Shape {}
```

### Pattern Matching (Java 16+):
```java
// instanceof pattern matching
if (obj instanceof String s) {
    System.out.println(s.toUpperCase());
}

// Switch pattern matching (Java 21+)
String result = switch (obj) {
    case Integer i -> "Integer: " + i;
    case String s -> "String: " + s;
    case null -> "Null";
    default -> "Unknown";
};
```

### Virtual Threads (Java 21+):
```java
// Легковесные потоки
Thread.startVirtualThread(() -> {
    System.out.println("Virtual thread");
});

// С ExecutorService
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> System.out.println("Task"));
}
```

## Spring Framework
### Dependency Injection:
```java
@Component
public class UserService {
    private final UserRepository repository;
    
    @Autowired  // Необязательно для constructor injection
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}

@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }
}
```

### Spring Boot:
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```

## Best Practices
1. **Используйте immutability**: final fields, Collections.unmodifiable
2. **Предпочитайте composition над inheritance**
3. **Программируйте к интерфейсам, не к реализациям**
4. **Используйте try-with-resources для ресурсов**
5. **Override equals() и hashCode() вместе**
6. **Используйте StringBuilder для конкатенации в циклах**
7. **Избегайте null**: Optional, @NonNull аннотации
8. **Следуйте SOLID принципам**
9. **Документируйте с помощью JavaDoc**
10. **Юнит-тесты обязательны**

## Инструменты
### Build Tools:
- **Maven**: pom.xml, dependency management
- **Gradle**: build.gradle, более гибкий

### Testing:
- **JUnit 5**: юнит-тестирование
- **Mockito**: мокирование
- **AssertJ**: fluent assertions

### IDE:
- IntelliJ IDEA
- Eclipse
- VS Code с расширениями

## Применение
### Где используется Java:
- Enterprise приложения (банки, корпорации)
- Android разработка
- Web applications (Spring Boot)
- Микросервисы
- Big Data (Hadoop, Spark)
- Cloud applications

## Применение в агентах
Java-агенты могут:
- Писать и объяснять код на Java
- Помогать с проектированием архитектуры
- Консультировать по Spring Framework
- Оптимизировать производительность
- Разрабатывать REST API
- Помогать с многопоточностью
- Обучать best practices

## Ресурсы для изучения
### Книги:
- "Effective Java" — Joshua Bloch
- "Java: The Complete Reference" — Herbert Schildt
- "Head First Java" — Kathy Sierra
- "Spring in Action" — Craig Walls

### Онлайн:
- Oracle Java Documentation
- Baeldung
- JavaRanch
- JetBrains Academy

### Практика:
- LeetCode, HackerRank
- Codewars
- Spring Pet Clinic (sample project)
