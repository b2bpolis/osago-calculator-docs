# Создаём участников полиса

URL `http://ibg.dev.b2bpolis.ru/rest/default/client/natural-person-create`
METHOD `POST`
FIELDS

```JSON
{
    "birth_date": "1986-12-14", 
    "check_driver": "true", 
    "credential": [
        {
            "credential_type": 3,
            "issue_point": "РФ", 
            "number": "191882", 
            "series": "16ТН"
        }
    ], 
    "driving_experience_started": "2008-11-08", 
    "first_name": "Марина", 
    "last_name": "Бердникова", 
    "patronymic": "Николаевна"
}
```
Обязательные поля:
"check_driver": "true"
"credential_type": 3

