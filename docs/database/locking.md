# Блокировки в БД

> Блокировка (Locking) - это механизм, используемый в системах управления базами данных (СУБД) для управления параллельным доступом к данным. 
> Он предназначен для предотвращения конфликтов и обеспечения согласованности транзакций. 
> Различные типы блокировок и уровни изоляции используются для балансировки между параллелизмом и согласованностью данных.

Вот некоторые ключевые аспекты блокировок:

* Типы блокировок:
    * Избирательная блокировка (Selective Locking): Блокируются только те ресурсы, которые реально нужны для выполнения транзакции.
    * Пессимистическая блокировка (Pessimistic Locking): Транзакции блокируют ресурсы на протяжении всей транзакции, предотвращая любые другие изменения к этим ресурсам.
    * Оптимистическая блокировка (Optimistic Locking): Транзакции допускаются без блокировок, но проверяются на конфликты перед фиксацией изменений.

* Уровни блокировки:
    * Блокировка на уровне строки (Row-level Locking): Блокировка применяется к отдельным строкам данных. Это обеспечивает более высокий уровень параллелизма, но может привести к более частым конфликтам.
    * Блокировка на уровне страницы (Page-level Locking): Блокировка применяется к группе строк (странице данных). Это улучшает параллелизм по сравнению с блокировкой на уровне строки и снижает количество конфликтов.
    * Блокировка на уровне таблицы (Table-level Locking): Блокировка применяется ко всей таблице. Это обеспечивает простоту реализации, но снижает параллелизм и увеличивает вероятность конфликтов.

* Конфликты блокировок:
    * Чтение-запись (Read-Write): Конфликт возникает, когда одна транзакция читает данные, которые другая транзакция пытается изменить.
    * Запись-запись (Write-Write): Конфликт возникает, когда две транзакции пытаются изменить одни и те же данные.
    * Чтение-чтение (Read-Read): Чтение данных другой транзакцией не является конфликтом, и блокировка может быть снята.

* Время блокировки:
    * Эксклюзивная блокировка (Exclusive Lock): Транзакция получает эксклюзивное право на ресурс, предотвращая другим транзакциям читать или изменять его.
    * Длительная блокировка (Long-duration Lock): Блокировка применяется к ресурсу на длительный срок, например, на всю длительность транзакции.
    * Краткосрочная блокировка (Short-duration Lock): Блокировка применяется к ресурсу только на короткий промежуток времени.

> Блокировки играют ключевую роль в управлении конфликтами между транзакциями и обеспечивают согласованность данных в многозадачных средах баз данных. 
> Однако, неправильное использование блокировок может привести к проблемам с производительностью, поэтому важно тщательно выбирать типы и уровни блокировок в зависимости от конкретных требований приложения.