# Домен: Программирование на C

## Описание области
C — мощный низкоуровневый язык программирования общего назначения. Используется для системного программирования, разработки ОС, драйверов, встроенных систем и высокопроизводительных приложений.

## Ключевые компетенции
- Системное программирование
- Управление памятью
- Указатели и адресная арифметика
- Работа с файлами и потоками
- Структуры данных
- Алгоритмы
- Многопоточность
- Оптимизация кода

## Основы языка C
### Стандарты:
- **C89/C90** (ANSI C): первый стандарт
- **C99**: новые типы данных, комментарии //
- **C11**: многопоточность, атомарные операции
- **C17/C18**: технические исправления
- **C23** (upcoming): новые возможности

### Базовые типы данных:
```c
// Целочисленные
char, short, int, long, long long
unsigned char, unsigned int, ...

// С плавающей точкой
float, double, long double

// Другие
_Bool (или bool с stdbool.h)
void
```

### Структура программы:
```c
#include <stdio.h>  // Препроцессорная директива

// Прототипы функций
int add(int a, int b);

int main(void) {
    // Точка входа
    printf("Hello, World!\n");
    return 0;
}

int add(int a, int b) {
    return a + b;
}
```

## Указатели и память
### Указатели:
```c
int x = 10;
int *ptr = &x;      // ptr указывает на x
*ptr = 20;          // Изменяем x через указатель
printf("%d", x);    // Выведет 20

// Указатель на указатель
int **ptr2 = &ptr;

// Указатель на функцию
int (*func_ptr)(int, int) = &add;
int result = (*func_ptr)(5, 3);
```

### Динамическая память:
```c
#include <stdlib.h>

// Выделение памяти
int *arr = (int*)malloc(10 * sizeof(int));
if (arr == NULL) {
    // Обработка ошибки
    return -1;
}

// Использование
arr[0] = 100;

// Освобождение
free(arr);
arr = NULL;  // Хорошая практика

// Альтернативы
int *arr2 = (int*)calloc(10, sizeof(int));  // Инициализирует нулями
arr = (int*)realloc(arr, 20 * sizeof(int));  // Изменение размера
```

### Распространённые ошибки:
- **Memory leak**: забыли free()
- **Dangling pointer**: использование после free()
- **Buffer overflow**: запись за границы массива
- **Double free**: освобождение дважды
- **Use after free**: использование после освобождения

## Структуры и объединения
### Структуры:
```c
// Определение
struct Person {
    char name[50];
    int age;
    float height;
};

// Использование
struct Person p1;
strcpy(p1.name, "John");
p1.age = 30;

// С указателем
struct Person *ptr = &p1;
ptr->age = 31;  // Эквивалентно (*ptr).age = 31

// typedef для удобства
typedef struct {
    int x;
    int y;
} Point;

Point p = {10, 20};
```

### Объединения (Union):
```c
union Data {
    int i;
    float f;
    char str[20];
};

union Data data;
data.i = 10;      // Теперь используется как int
data.f = 3.14;    // Перезаписывает те же байты
```

## Массивы и строки
### Массивы:
```c
// Статический массив
int arr[10];
int arr2[] = {1, 2, 3, 4, 5};  // Размер автоматически

// Многомерные массивы
int matrix[3][4];
int matrix2[2][3] = {{1,2,3}, {4,5,6}};

// Передача в функцию
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
}
```

### Строки:
```c
#include <string.h>

char str1[] = "Hello";           // {'H','e','l','l','o','\0'}
char str2[20] = "World";
char *str3 = "Constant";         // Строковый литерал (read-only)

// Основные функции
strlen(str1);                    // Длина
strcpy(str2, str1);             // Копирование
strcat(str2, " World");         // Конкатенация
strcmp(str1, str2);             // Сравнение
strstr(str1, "llo");            // Поиск подстроки
```

## Файловый ввод-вывод
```c
#include <stdio.h>

// Открытие файла
FILE *fp = fopen("file.txt", "r");  // "r","w","a","rb","wb"
if (fp == NULL) {
    perror("Error opening file");
    return -1;
}

// Чтение
char buffer[100];
fgets(buffer, 100, fp);          // Построчно
fscanf(fp, "%d", &num);          // Форматированное

// Запись
fprintf(fp, "Number: %d\n", 42);
fputs("Hello\n", fp);

// Бинарное чтение/запись
fread(buffer, sizeof(char), 100, fp);
fwrite(buffer, sizeof(char), 100, fp);

// Закрытие
fclose(fp);
```

## Препроцессор
### Директивы:
```c
// Включение файлов
#include <stdio.h>      // Системные заголовки
#include "myheader.h"   // Локальные заголовки

// Макросы
#define PI 3.14159
#define MAX(a,b) ((a) > (b) ? (a) : (b))
#define SQUARE(x) ((x) * (x))

// Условная компиляция
#ifdef DEBUG
    printf("Debug mode\n");
#endif

#ifndef HEADER_H
#define HEADER_H
// Содержимое заголовка
#endif

// Предопределённые макросы
__FILE__    // Имя файла
__LINE__    // Номер строки
__DATE__    // Дата компиляции
__TIME__    // Время компиляции
```

## Функции и модульность
### Объявление и определение:
```c
// Прототип (в .h файле)
int factorial(int n);

// Определение (в .c файле)
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// Статические функции (только в текущем файле)
static int helper(int x) {
    return x * 2;
}

// Inline функции (C99+)
inline int add(int a, int b) {
    return a + b;
}
```

### Указатели на функции:
```c
// Определение типа
typedef int (*operation)(int, int);

// Использование
int add(int a, int b) { return a + b; }
int sub(int a, int b) { return a - b; }

operation ops[] = {add, sub};
int result = ops[0](10, 5);  // add(10, 5)
```

## Структуры данных
### Связный список:
```c
typedef struct Node {
    int data;
    struct Node *next;
} Node;

// Добавление элемента
Node* createNode(int data) {
    Node *newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

void insert(Node **head, int data) {
    Node *newNode = createNode(data);
    newNode->next = *head;
    *head = newNode;
}

// Освобождение списка
void freeList(Node *head) {
    Node *temp;
    while (head != NULL) {
        temp = head;
        head = head->next;
        free(temp);
    }
}
```

### Стек:
```c
#define MAX_SIZE 100

typedef struct {
    int data[MAX_SIZE];
    int top;
} Stack;

void initStack(Stack *s) {
    s->top = -1;
}

int push(Stack *s, int value) {
    if (s->top == MAX_SIZE - 1) return 0;  // Переполнение
    s->data[++s->top] = value;
    return 1;
}

int pop(Stack *s, int *value) {
    if (s->top == -1) return 0;  // Пустой стек
    *value = s->data[s->top--];
    return 1;
}
```

## Многопоточность (C11)
```c
#include <threads.h>

int thread_func(void *arg) {
    int *num = (int*)arg;
    printf("Thread: %d\n", *num);
    return 0;
}

int main() {
    thrd_t thread;
    int data = 42;
    
    thrd_create(&thread, thread_func, &data);
    thrd_join(thread, NULL);
    
    return 0;
}

// Мьютексы
mtx_t mutex;
mtx_init(&mutex, mtx_plain);

mtx_lock(&mutex);
// Критическая секция
mtx_unlock(&mutex);

mtx_destroy(&mutex);
```

## Системное программирование
### POSIX API (Unix/Linux):
```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>

// Процессы
pid_t pid = fork();
if (pid == 0) {
    // Дочерний процесс
    execl("/bin/ls", "ls", "-l", NULL);
}

// Работа с файловой системой
struct stat st;
stat("file.txt", &st);
printf("Size: %ld\n", st.st_size);

// Сокеты, pipe, signals и т.д.
```

## Best Practices
1. **Всегда проверяйте возвращаемые значения** (malloc, fopen и т.д.)
2. **Освобождайте память**: каждый malloc → free
3. **Инициализируйте переменные**: избегайте неопределённого поведения
4. **Используйте const**: где возможно
5. **Проверяйте границы массивов**: избегайте buffer overflow
6. **Компилируйте с предупреждениями**: -Wall -Wextra
7. **Используйте статический анализ**: valgrind, clang-tidy
8. **Документируйте код**: особенно сложные части

## Компиляция и сборка
### GCC:
```bash
# Простая компиляция
gcc main.c -o program

# С предупреждениями
gcc -Wall -Wextra -Werror main.c -o program

# Отладочная информация
gcc -g main.c -o program

# Оптимизация
gcc -O2 main.c -o program  # -O0, -O1, -O2, -O3

# Несколько файлов
gcc main.c utils.c -o program

# С библиотекой
gcc main.c -lm -o program  # math library
```

### Makefile:
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -std=c11
LDFLAGS = -lm

TARGET = program
SRCS = main.c utils.c
OBJS = $(SRCS:.c=.o)

$(TARGET): $(OBJS)
	$(CC) $(OBJS) $(LDFLAGS) -o $(TARGET)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)
```

## Отладка
### GDB:
```bash
# Компиляция с отладочной информацией
gcc -g program.c -o program

# Запуск GDB
gdb ./program

# Основные команды
(gdb) break main      # Точка останова
(gdb) run            # Запуск
(gdb) next           # Следующая строка
(gdb) step           # Войти в функцию
(gdb) print var      # Вывести переменную
(gdb) backtrace      # Стек вызовов
(gdb) quit           # Выход
```

### Valgrind (поиск утечек памяти):
```bash
valgrind --leak-check=full ./program
```

## Безопасность
### Распространённые уязвимости:
- **Buffer Overflow**: переполнение буфера
- **Format String**: уязвимость в printf
- **Integer Overflow**: переполнение целых
- **Use After Free**: использование после освобождения
- **NULL pointer dereference**: разыменование NULL

### Защита:
```c
// Вместо gets() (небезопасно)
fgets(buffer, sizeof(buffer), stdin);

// Вместо strcpy() (может переполнить)
strncpy(dest, src, sizeof(dest) - 1);
dest[sizeof(dest) - 1] = '\0';

// Проверка указателей
if (ptr != NULL) {
    *ptr = value;
}

// Безопасное преобразование
if (value > INT_MAX / multiplier) {
    // Переполнение!
}
```

## Применение
### Области использования C:
- Операционные системы (Linux, Windows kernel)
- Встроенные системы
- Драйверы устройств
- Высокопроизводительные приложения
- Системное ПО
- Библиотеки низкого уровня
- Сетевое программирование

## Применение в агентах
Агенты по C могут:
- Писать и объяснять код на C
- Помогать с отладкой
- Оптимизировать производительность
- Разрабатывать алгоритмы и структуры данных
- Консультировать по управлению памятью
- Помогать с системным программированием
- Обучать лучшим практикам

## Ресурсы для изучения
### Книги:
- "The C Programming Language" — Kernighan & Ritchie (K&R)
- "C Programming: A Modern Approach" — K.N. King
- "Expert C Programming" — Peter van der Linden
- "Modern C" — Jens Gustedt

### Онлайн:
- cppreference.com
- GeeksforGeeks C programming
- Beej's Guides

### Практика:
- LeetCode, HackerRank (алгоритмы)
- Project Euler
- Open Source проекты на C
