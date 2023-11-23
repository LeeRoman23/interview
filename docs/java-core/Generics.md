# Generics


**Generics** в Java представляют собой механизм, который позволяет создавать классы, интерфейсы и методы, обобщенные по типам данных. 
Это позволяет писать код, который может работать с различными типами данных, обеспечивая безопасность типов во время компиляции.

Можно ограничивать параметры типа, например, указывая, что они должны быть подклассами определенного класса или реализовывать определенный интерфейс.

```java
public class Box<T extends Number> {
    // ...
}
```

```java
public class GenericExample<T> {
    private T value;

    public GenericExample(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }

    public static <U> void printBox(GenericExample<U> box) {
        System.out.println(box.getValue());
    }

    public static void main(String[] args) {
        GenericExample<Integer> integerBox = new GenericExample<>(42);
        printBox(integerBox);

        GenericExample<String> stringBox = new GenericExample<>("Hello, Generics!");
        printBox(stringBox);
    }
}
```

**Wildcard (знак вопроса):**

Wildcard в Java используется для создания более гибких обобщенных типов. Wildcard представляется знаком вопроса (?). Существует два основных типа wildcard: ? extends T и ? super T.

1. **? extends T** (Upper Bounded Wildcard): Позволяет использовать любой тип, который является подтипом T или самим T.

```java
public void processElements(List<? extends Number> elements) {
    for (Number element : elements) {
        // ...
    }
}
```
2. **? super T** (Lower Bounded Wildcard): Позволяет использовать любой тип, который является супертипом T или самим T.

```java
public void addElement(List<? super Integer> elements) {
    elements.add(42);
}
```