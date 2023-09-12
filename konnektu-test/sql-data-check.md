# SQL-запросы

## Регистрация пользователя:

### Проверка отсутствия дублирования идентификаторов (email или номера телефона) перед регистрацией пользователя:
SELECT Value, COUNT(*) AS Count
FROM UserIdentifier
WHERE Type IN (1, 2)
GROUP BY Value
HAVING Count > 1;

### Проверка отсутствия дублирования идентификаторов (email или номера телефона) для конкретного пользователя:
SELECT UserId, Type, Value, COUNT(*) AS Count
FROM UserIdentifier
WHERE UserId = USER_ID
GROUP BY UserId, Type, Value
HAVING Count > 1;

Где USER_ID это наше значение UserID

### Проверка корректности регистрации пользователя в обеих таблицах
SELECT User.Id AS UserId, UserIdentifier.Type, UserIdentifier.Value
FROM User
JOIN UserIdentifier ON User.Id = UserIdentifier.UserId;