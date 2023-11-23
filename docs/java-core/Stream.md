# Stream API

**Stream API** в Java представляет собой мощный и удобный инструмент для работы с последовательностями элементов (коллекциями, массивами и другими данными). 
Stream API предоставляет декларативный способ выполнения операций над данными и облегчает параллельную обработку данных.

### Основные понятия в Stream API:

1. Stream (Поток):
    * Поток представляет собой последовательность элементов, которую можно обрабатывать с использованием функциональных операций.
2. Операции:
    * В Stream API выделяют промежуточные и терминальные операции.
      Промежуточные операции выполняются над потоком и возвращают новый поток, позволяя цепочку операций. Примеры: filter, map, sorted.
      Терминальные операции заканчивают обработку потока и возвращают результат. Примеры: forEach, collect, reduce.
3. Ленивость:
    * Потоки являются ленивыми, что означает, что операции выполняются только при необходимости, когда требуется результат.
4. Параллельность:
    * Потоки могут быть обработаны параллельно, что улучшает производительность. Для этого используется метод parallel().
5. Источники данных:
    * Потоки могут быть созданы из различных источников данных, таких как коллекции (Collection.stream()), массивы (Arrays.stream()), файлы (Files.lines()), и др.

```java Filter
List<String> words = Arrays.asList("apple", "banana", "grape", "melon");
List<String> filteredWords = words.stream()
                                  .filter(word -> word.length() > 5)
                                  .collect(Collectors.toList());

```

```java Map
List<String> words = Arrays.asList("apple", "banana", "grape", "melon");
List<Integer> wordLengths = words.stream()
                                .map(String::length)
                                .collect(Collectors.toList());
```

```java Sorting
List<String> words = Arrays.asList("apple", "banana", "grape", "melon");
List<String> sortedWords = words.stream()
                               .sorted()
                               .collect(Collectors.toList());
```

```java Reduce
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
                 .reduce(0, Integer::sum);
```

```java Parallel
List<String> words = Arrays.asList("apple", "banana", "grape", "melon");
List<String> parallelFilteredWords = words.parallelStream()
                                         .filter(word -> word.length() > 5)
                                         .collect(Collectors.toList());
```

>map и flatMap - это две операции в Stream API, используемые для преобразования элементов в потоке. 
Однако у них есть различия в том, как они обрабатывают вложенные структуры данных.
**Map** возвращает новый поток, в котором каждый элемент преобразован в соответствии с функцией.
**flatMap** также применяется к каждому элементу потока, но в отличие от map, функция, переданная в flatMap, должна возвращать поток элементов, а не отдельные элементы.


### Stateful и Stateless операции

Stateful операции:

Эти операции сохраняют состояние между элементами потока.
Примеры stateful операций в Stream API включают distinct() и sorted(). 
Обе эти операции должны помнить предыдущие элементы потока, чтобы правильно работать. 
Например, чтобы убедиться, что элементы уникальны (в случае distinct()) или для правильной сортировки (в случае sorted()).

```java
List<Integer> uniqueNumbers = numbers.stream()
                                    .distinct() // stateful
                                    .collect(Collectors.toList());
```

Stateless операции:

Эти операции не сохраняют состояние между элементами потока.
Каждый элемент обрабатывается независимо от других элементов.
Примеры stateless операций включают filter(), map(), forEach() и т. д.

```java
List<String> upperCaseNames = names.stream()
                                   .map(String::toUpperCase) // stateless
                                   .collect(Collectors.toList());
```