# Errors

Когда какой-либо из запросов не выполнен в результате возникновения ошибки, в теле ответа содержится детализация ошибки в <font color="#14CB79">JSON</font> формате.

```json

{"result": "fail", "message": "Permission denied", "body": []}



{"result": "fail", "message": "{'phone_number': ['Enter a valid mobile number.']}"}



{"result": "fail", "message": "ActivationCode matching query does not exist."}


{"result": "fail", "message": "Incorrect phone_number or credentials", 
"sessionid": ""}

{"result": "fail", "message": "['ActivationCode does not exist or expire']"}


{"result": "fail", "message": "That page contains no results", 
"paging": {"pages": 1, "page": "2"}, "body": []}


{"result": "fail", "message": "Device is blocked"}

{"result": "fail","message": "Device has no mobile app"}
```



Ошибка | Значение
---------- | -------
Permission denied | У пользователя недостаточно прав при обращении к ресурсу или истекло время жизни сессии.
Enter a valid mobile number | Указан некорректный номер телефона.
ActivationCode matching query does not exist | Код активации мобильного устройства не найден.
Incorrect phone_number or credentials | При попытке аутентификации указан не верный номер телефона или пароль в поле credentials.
ActivationCode does not exist or expire | Не валидный код подтверждения.
That page contains no results | Попытка запросить не существующую страницу.
Device is blocked | На любой запрос авторизованного устройства, если он был заблокирован администратором.
Device has no mobile app | Попытка выполнить запрос с удаленного приложения.