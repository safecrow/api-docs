# Errors

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Неправильный API ключ.
403 | Forbidden -- Нехватает прав для запрашиваемого ресурса.
404 | Not Found -- Запрашиваемый ресурс не существует.
422 | Unprocessable Entity -- Cервер принял запрос, но имеется какая-то логическая ошибка, из-за которой невозможно произвести операцию.
500 | Internal Server Error -- Серверная ошибка. Попробуйте позже.
503 | Service Unavailable -- Сервис временно не доступен.
