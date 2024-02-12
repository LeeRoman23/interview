# JPA

JPA - это стандарт Java EE, определяющий интерфейсы и поведение для работы с объектно-реляционным отображением (ORM).
Hibernate является одной из реализаций этого стандарта.

## Из чего состоит JPA

1. Entity

**Аннотация @Entity**: Класс, который отмечен аннотацией @Entity, представляет объектную модель, которая будет сохранена в базе данных. 
Каждый объект этого класса соответствует записи в таблице базы данных.

```java
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class User {
    @Id
    private Long id;
    private String username;
    private String email;
    // Геттеры и сеттеры
}
```

2. EntityManager:

**EntityManager** предоставляет методы для выполнения операций с базой данных, таких как вставка, обновление, удаление и выполнение запросов. 
Он также отвечает за управление жизненным циклом сущностей.

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("com.test");
```

3. JPQL (Java Persistence Query Language):

**Язык запросов JPQL:** JPQL - это объектный язык запросов, аналогичный SQL, но работающий с объектами вместо таблиц базы данных. 
Он позволяет выполнять запросы к базе данных с использованием сущностей и их свойств.

```java
TypedQuery<User> query = entityManager.createQuery("SELECT u FROM User u WHERE u.username = :username", User.class);
query.setParameter("username", "john_doe");
List<User> users = query.getResultList();
```

4. Transaction Management:

Транзакции: JPA предоставляет возможности управления транзакциями при выполнении операций с базой данных. 
Используйте аннотации @Transactional или методы begin, commit, rollback для управления транзакциями.

```java
entityManager.getTransaction().begin();
// Выполнение операций с базой данных
entityManager.getTransaction().commit();
```

## Типы Fetch стратегий в JPA

В Java Persistence API (JPA) существуют различные стратегии загрузки (fetch strategies), которые определяют, когда и как ассоциированные объекты или коллекции будут извлечены из базы данных. 
Fetch-стратегии влияют на то, как JPA загружает связанные данные при выполнении запросов. Вот основные типы fetch-стратегий:

1. Eager Fetch (Энергичная загрузка):

В стратегии eager (энергичной загрузки), связанные объекты или коллекции извлекаются из базы данных одновременно с основным объектом, к которому они относятся. 
Это означает, что все связанные данные будут доступны сразу.

```java
@Entity
public class Order {
    // Другие поля

    @OneToMany(fetch = FetchType.EAGER)
    private List<OrderItem> items;
}
```

2. Lazy Fetch (Ленивая загрузка):

В стратегии lazy (ленивой загрузки), связанные объекты или коллекции не загружаются из базы данных до тех пор, пока они явно не будут запрошены. 
Это позволяет отложить загрузку данных до момента, когда они реально будут использованы.

```java
@Entity
public class Order {
    // Другие поля

    @OneToMany(fetch = FetchType.LAZY)
    private List<OrderItem> items;
}
```

3. Subselect Fetch (Загрузка по подзапросу):

Subselect fetch strategy предназначена для оптимизации запросов на загрузку коллекции. 
Она использует подзапрос для загрузки коллекции объектов, что может снизить количество выражений SQL и улучшить производительность.

```java
@Entity
public class Order {
    // Другие поля

    @OneToMany(fetch = FetchType.SUBSELECT)
    private List<OrderItem> items;
}
```

4. Batch Fetch (Групповая загрузка):

Batch fetch strategy позволяет загружать несколько объектов или коллекций с использованием одного SQL-запроса, что может уменьшить количество запросов к базе данных и улучшить производительность.

```java
@Entity
public class Order {
    // Другие поля

    @OneToMany(fetch = FetchType.LAZY)
    @BatchSize(size = 10)
    private List<OrderItem> items;
}
```

5. Select Fetch (Выборочная загрузка):

В стратегии select fetch strategy, загрузка ассоциированных объектов или коллекций происходит посредством дополнительных запросов SELECT. 
Каждый объект или коллекция загружается отдельно.

```java
@Entity
public class Order {
    // Другие поля

    @OneToMany(fetch = FetchType.SELECT)
    private List<OrderItem> items;
}
```
