# Типы данных в Java

### Примитивные типа данных

* **byte (1 байт):**
Представляет целочисленное значение от -128 до 127.

* **short (2 байта):**
Представляет целочисленное значение от -32,768 до 32,767.

* **int (4 байта):**
Представляет целочисленное значение от -2^31 до 2^31 - 1.

* **long (8 байт):**
Представляет целочисленное значение от -2^63 до 2^63 - 1.

* **float (4 байта):**
Представляет число с плавающей точкой (одинарной точности).

* **double (8 байт):**
Представляет число с плавающей точкой (двойной точности).

* **char (2 байта):**
Представляет символ Unicode.

* **boolean:**
Представляет логическое значение true или false.

### Ссылочные типы данных

* **String**

String в Java представляет собой класс, который используется для работы с текстовыми строками. Вот основные особенности String:

1. _Неизменяемость (Immutability):_

Строки в Java являются неизменяемыми, что означает, что их значение не может быть изменено после создания. Все операции, модифицирующие строку, создают новый объект строки.

2. _Создание строк:_

Строки можно создавать с использованием литералов (например, "Hello"), конструктора (new String("Hello")) или методов, предоставляемых классом String.

3. _Конкатенация:_

Строки можно объединять с использованием оператора + или метода concat(). Оператор + также автоматически преобразует другие типы в строки.

4. _Методы работы со строками:_

   * length(): Возвращает длину строки.
   * charAt(int index): Возвращает символ по указанному индексу.
   * substring(int beginIndex): Возвращает подстроку, начиная с указанного индекса.
   * substring(int beginIndex, int endIndex): Возвращает подстроку в заданном диапазоне.

5. _StringBuilder, StringBuffer_

   * **StringBuilder** является изменяемым (mutable) классом для работы со строками.
   Он предоставляет методы для добавления, изменения и удаления символов без создания новых объектов.
   Не является потокобезопасным
```java
StringBuilder stringBuilder = new StringBuilder("Hello");
stringBuilder.append(" World");
stringBuilder.insert(5, " Java");
stringBuilder.delete(5, 6);
String result = stringBuilder.toString();
```

  * **StringBuffer** аналогичен StringBuilder, но является потокобезопасным (thread-safe) за счет синхронизации методов. 
```java
StringBuffer stringBuffer = new StringBuffer("Hello");
stringBuffer.append(" World");
stringBuffer.insert(5, " Java");
stringBuffer.delete(5, 6);
String result = stringBuffer.toString();
```
Из-за синхронизации StringBuffer может быть медленнее в многозадачных сценариях по сравнению с StringBuilder.

**String**: В памяти хранится в пуле строк и управляется сборщиком мусора. Неизменяемость строк позволяет им быть общими между несколькими объектами, что экономит память.

**StringBuilder** и **StringBuffer**: Хранятся в обычной области памяти и могут менять свое содержимое.

**Строковый пул (String Pool)** в Java - это механизм для управления строками и оптимизации использования памяти. 
Он представляет собой специальное место в памяти, где хранятся уникальные литеральные строки. Когда вы создаете строку литералом (например, "Hello"), Java сначала проверяет, есть ли такая строка уже в пуле. 
Если строка с таким содержанием уже существует, то новая строка не создается, а используется ссылка на существующую. Это позволяет сэкономить память и сделать работу с строками более эффективной.

* **Objects**

### '==' и 'equals()'

Оператор '==' и метод 'equals()' имеют разные семантики, и их использование зависит от контекста и типа данных.

**Оператор '=='**

* При использовании с примитивными типами (int, char, double, и т.д.), == сравнивает значения. 
  Если значения равны, условие возвращает true, в противном случае - false.
* При использовании с объектами (ссылочными типами), == сравнивает значения ссылок, а не содержимое объектов. 
  То есть, == возвращает true, если обе переменные ссылки указывают на один и тот же объект в памяти.

**equals()**

* Метод equals() предоставляется классом Object и может быть переопределен в подклассах. 
  В реализации по умолчанию в классе Object он эквивалентен оператору == (сравнение ссылок).

* Многие классы, такие как String, Integer, и другие, переопределяют метод equals(), чтобы сравнение происходило не по ссылкам, а по содержимому объектов.

_Для объектов классов, не переопределяющих equals(), это приведет к использованию дефолтной реализации из класса Object, что будет эквивалентно оператору ==. В контексте классов, переопределяющих equals(), важно удостовериться, что реализация метода корректна и соблюдает условия равенства для объектов._


### Boxing & Unboxing

В Java упаковка (boxing) и распаковка (unboxing) примитивных типов данных происходит автоматически благодаря механизму автоупаковки (autoboxing) и автораспаковки (autounboxing), введенному в Java 5.

**Упаковка(boxing)** — это процесс преобразования примитивного типа данных в соответствующий класс-оболочку. Например, преобразование int в Integer.
```java
int primitiveInt = 42;

// Автоупаковка
Integer wrappedInt = primitiveInt; // Преобразование int в Integer
```

**Распаковка(Unboxing)** — это процесс извлечения значения из объекта-оболочки и преобразование его в примитивный тип данных.
```java
Integer wrappedInt = 42;

// Автораспаковка
int primitiveInt = wrappedInt; // Преобразование Integer в int
```

_Эти механизмы облегчают работу с примитивными типами данных в контексте использования объектов и коллекций, где требуются объекты. Java автоматически выполняет упаковку и распаковку в таких ситуациях, что упрощает код и делает его более читаемым._