# JDBC

**JDBC (Java Database Connectivity)** - это API (Application Programming Interface) для взаимодействия с реляционными базами данных из приложений, написанных на языке программирования Java.
JDBC обеспечивает стандартизированный способ подключения к базам данных, выполнения SQL-запросов и обработки результатов запросов.

## Этапы работы с базой данных с использованием JDBC

Работа с базой данных с использованием JDBC обычно включает в себя несколько этапов. 
Ниже приведены основные этапы работы с базой данных с использованием JDBC:

1. **Загрузка JDBC драйвера:**

Перед тем, как начать взаимодействие с базой данных, необходимо загрузить драйвер JDBC, который соответствует вашей СУБД. 
Это можно сделать с использованием статического метода Class.forName():
```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

2. **Установление соединения:**

Используйте DriverManager для получения соединения с базой данных. Укажите URL базы данных, имя пользователя и пароль:

```java
String url = "jdbc:mysql://localhost:3306/yourdatabase";
String username = "yourusername";
String password = "yourpassword";

Connection connection = DriverManager.getConnection(url, username, password);
```

3. **Выполнение запросов:**

Используйте Statement или PreparedStatement для выполнения SQL-запросов. 
Statement используется для простых запросов, а PreparedStatement - для параметризованных:

```java
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery("SELECT * FROM yourtable");

// Или используйте PreparedStatement
PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO yourtable (column1, column2) VALUES (?, ?)");
preparedStatement.setString(1, "value1");
preparedStatement.setString(2, "value2");
preparedStatement.executeUpdate();
```

4. **Обработка результатов:**

Если запрос возвращает результаты, используйте ResultSet для обработки данных:

```java
while (resultSet.next()) {
    int id = resultSet.getInt("id");
    String name = resultSet.getString("name");
    // Обработка данных
}
```

5. **Закрытие ресурсов:**
   
Важно закрывать ресурсы, такие как Connection, Statement, и ResultSet, после их использования. Используйте try-with-resources или блок finally:

```java
try (Connection connection = DriverManager.getConnection(url, username, password);
     Statement statement = connection.createStatement();
     ResultSet resultSet = statement.executeQuery("SELECT * FROM yourtable")) {
    // Обработка результатов
} catch (SQLException e) {
    e.printStackTrace();
}
```

6. **Закрытие соединения:**

По завершении работы с базой данных закройте соединение:

```java
if (connection != null && !connection.isClosed()) {
    connection.close();
}
```


## 1) Statement:

**Statement** - это базовый способ выполнения SQL-запросов в JDBC. 
Он создает объект Statement, который отправляет SQL-запрос непосредственно на сервер базы данных без параметров.

Statement подвержен атакам SQL-инъекции, поскольку SQL-запрос формируется непосредственно на основе введенных данных, что делает его уязвимым для вставки злонамеренного SQL-кода.

```java
Statement statement = connection.createStatement();
String sql = "SELECT * FROM employees WHERE department = 'HR'";
ResultSet resultSet = statement.executeQuery(sql);
```

## 2) Prepared statement

**PreparedStatement** - это усовершенствованный способ выполнения SQL-запросов, который предназначен для безопасного выполнения SQL-запросов с параметрами. 
Он предварительно компилирует SQL-запрос и позволяет вставлять параметры в запрос без опасности SQL-инъекции.

PreparedStatement увеличивает безопасность, так как значения параметров автоматически экранируются и обрабатываются, что позволяет избежать вставки злонамеренных данных в SQL-запрос.

```java
String sql = "SELECT * FROM employees WHERE department = ?";
PreparedStatement preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1, "HR");
ResultSet resultSet = preparedStatement.executeQuery();
```

### Отличия Statement от PreparedStatement

1. **Statement** используется для выполнения простых SQL-запросов без параметров. Например, SELECT, INSERT, UPDATE без использования параметров. 
**PreparedStatement** предоставляет предварительно скомпилированный запрос с параметрами. Это позволяет создавать SQL-запросы с заполнителями, что делает код безопасным относительно SQL-инъекций и обеспечивает более эффективную работу с базой данных.

2. Каждый раз, когда вы исполняете SQL-запрос с **Statement**, он компилируется заново. 
Это может сказаться на производительности, особенно при частом выполнении одного и того же запроса с небольшими изменениями.
**PreparedStatement** предварительно компилирует SQL-запрос и сохраняет его структуру, поэтому при многократном выполнении запроса с небольшими изменениями происходит повторное использование предварительно скомпилированного запроса. 
Это может улучшить производительность приложения.

3. При использовании **Statement** более подвержены атакам SQL-инъекций, так как значения напрямую вставляются в строку SQL-запроса без обработки.
Использование **PreparedStatement** с параметрами делает код более безопасным относительно SQL-инъекций, так как значения параметров обрабатываются и экранируются автоматически.

### Преимущества PreparedStatement:

* Защита от SQL-инъекции.
* Повышение производительности при выполнении одного запроса несколько раз с разными параметрами, так как запрос компилируется только один раз.
* Возможность использовать разные типы параметров (строки, числа, даты и др.) без явного преобразования.
* В большинстве случаев рекомендуется использовать PreparedStatement, особенно при выполнении SQL-запросов с параметрами, чтобы обеспечить безопасность и эффективность работы с базой данных.


### ResultSet

>ResultSet в JDBC (Java Database Connectivity) представляет собой интерфейс, который используется для выполнения запросов к базам данных и получения результатов этих запросов. 
> ResultSet представляет собой таблицу данных, возвращенную из базы данных после выполнения SQL-запроса. 
> Эта таблица данных состоит из строк и столбцов, где каждая строка представляет одну запись, а каждый столбец представляет одно поле данных.

Основные операции, которые можно выполнять с ResultSet, включают в себя:

* Перемещение по результатам: Вы можете перемещаться по результатам, используя методы, такие как next(), previous(), first(), last(), absolute(), relative(), чтобы получить доступ к разным записям.

* Получение данных: Вы можете извлекать данные из ResultSet, используя методы, такие как getInt(), getString(), getDouble(), getDate(), и другие, в зависимости от типа данных в столбце.

* Метаданные: ResultSet также предоставляет метаданные о результатах запроса, такие как имена столбцов, типы данных и дополнительную информацию о структуре данных.

* Обновление данных: В зависимости от настроек ResultSet и типа запроса, вы можете также использовать ResultSet для обновления данных в базе данных.

```java
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery("SELECT id, name, age FROM employees");

while (resultSet.next()) {
    int id = resultSet.getInt("id");
    String name = resultSet.getString("name");
    int age = resultSet.getInt("age");
    System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age);
}

resultSet.close();
statement.close();
```

**Connection** - это объект, который представляет собой установленное соединение между Java-приложением и базой данных. 
Он используется для отправки SQL-запросов к базе данных, получения результатов запросов и управления транзакциями. 
Объект Connection обычно создается с использованием DriverManager или DataSource и содержит информацию о параметрах соединения, таких как URL базы данных, имя пользователя и пароль.
После использования соединения, его необходимо закрыть с помощью метода close(), чтобы освободить ресурсы и вернуть его в пул соединений, если используется соединительный пул.

```java Пример создания объекта Connection:
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydb", "username", "password");
```

**Connection Pool** представляет собой механизм, который управляет набором заранее созданных и готовых к использованию соединений с базой данных. 
Это сделано для того, чтобы избежать создания и разрыва соединений при каждом запросе к базе данных, что может быть ресурсоемкой операцией. 
Вместо этого приложение берет соединение из пула, использует его и затем возвращает обратно в пул после завершения операции.

Пул соединений может управляться с помощью специализированных библиотек, таких как Apache Commons DBCP, HikariCP, C3P0, и других. 

```java Пример использования пула соединений с помощью HikariCP:
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
config.setUsername("username");
config.setPassword("password");

HikariDataSource dataSource = new HikariDataSource(config);
Connection connection = dataSource.getConnection();
// Используйте соединение для выполнения SQL-запросов
connection.close(); // Вернуть соединение в пул
```

