
**Задача №1
Условия:** Разработан метод API, который осуществляет регистрацию пользователя по двум идентификаторам(email, phone). В массиве идентификаторов может быть только один или два объекта разных типов. "type": 1 –email, "type": 2 - phone.  То есть пользователь не может зарегистрироваться с двумя телефонами или двумя почтами. 
В техническом задании указано предусмотреть валидацию. Для phone “+7\*\*\*\*\*\*\*\*\*\*”, для email “\*@\*.\*” В случае ошибки валидации выводить ошибку - 422 “введен невалидный email” или 422 “введен невалидный phone”. 
Повторная регистрация с уже существующим в базе идентификатором вызывает ошибку 422 “Пользователь уже существует”

|cURL|
| :- |
|<p>curl --location 'https://test/registration' \</p><p>--header 'Content-Type: application/json' \</p><p>--data '{</p><p>`    `"Name": string,</p><p>`    `"Age": int32,</p><p>`    `"Identifiers": [</p><p>`        `{</p><p>`            `"Type": 1,</p><p>`            `"Value": string</p><p>`        `},</p><p>`        `{</p><p>`            `"Type": 2,</p><p>`            `"Value": string</p><p>`        `}</p><p>`    `],</p><p>"Password": string</p><p>}'</p>|

Успешный ответ возвращает Id юзера и его Login – уникальный номер:

200 ОК

{

`    `"UserId": 12,

`     `"Login": "126473",

`    `"Message": "Запрос успешно выполнен."

}

При успешном выполнении метода создаются записи в таблицы:

**"User":** "Id", "Name", "Age"

**"UserIdentifier":** "Id", "UserId"(Ссылка на "User"."Id"), "Type", "Value" 

После регистрации пользователя с помощью комбинации логин пароль  можно залогинить посредством метода:

|cURL|
| :- |
|<p>curl --location 'https://test/login' \</p><p>--header 'Content-Type: application/json' \</p><p>--data '{</p><p>`    `"Login":string,</p><p>`    `"Password": string</p><p>}'</p><p></p>|
Успешный ответ возвращает Token:

200 ОК

{

`    `"Token": "bJQzEQC?AuOt2zuUFyMiimu-Pf9JFkhD0zt=5LLN=t8f47LKWcpzJoOYqJ8p8!XqbM/q?7t8Z7h4j24hsOP743x4yp5mmixue2t0Kjt4ufctH?/yVJ84!wo6yGs8BExU2e7IDD=/CpB7YgTwHY347EU9VknUXeVSAxKPFnYcdjLvj0le?2rFpml?saaYhc6F6Ua9waDuduKdF6e0mQyNrGoz8pcsmF96jvF5kphiLePwGcx?7K1==DIm!4x6hCNa"

`    `"Message": "Запрос успешно выполнен."

}** 

Метод нужно использовать в качестве проверки успешности регистрации при составлении коллекции. Тестировать метод login не требуется

**Задание:**

1. Составить План тестирования, чек-лист, тест-кейсы.
1. Составить SQL-запросы для проверки данных ( join, проверить дублирование и т.п)
1. Составить коллекцию для тестирования в любом удобном инструменте для работы с API.

**Задача №2**

**Условия:** Книжный магазин проводит акцию на своем сайте. За каждую купленную книгу можно получить бонусные баллы. 
Начисление происходит после проверки данных пользователя по трем параметрам:

1. Действует ли на пользователя сейчас другая программа лояльности: true – баллы не начисляются; false – баллы начисляются
1. Стоимость книги: <100- баллы не начисляются; >=100, но < 1000 – начисляется 10 баллов;  >=1000, но <2000 – начисляется 100 баллов; >=2000-начисляется 300 баллов
1. Если книга из перечисленных категорий, то добавляется определенный процент баллов: классическая литература – +50% баллов, нехудожественная литература - +25% баллов, книги для детей - +100% баллов

То есть если пользователь купил книгу за 1500 рублей из категории классической литературы, и при этом на него не действуют другие программы лояльности, то ему зачислится 150 баллов

**Задание:**

Необходимо составить тест-кейсы пользуясь техниками тест-дизайна.
