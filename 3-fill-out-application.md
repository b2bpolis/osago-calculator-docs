[СОДЕРЖАНИЕ](README.md)

# Заполните заявление

На второй этап мы передаем два реста через запрос GET:

Название запроса      | Пример
--------------------- | ------------------------------------------------------------------------------------
result                | `/rest/v3/default/calculation/6651480/result/?insurance_company=1&messages=1&vars=1`
insured-object-create | `/rest/default/client/insured-object-create/`

Дополним рест `natural-person-create` участниками договора (кто является собственником и страхователем) а также паспортные данные `credential`.
Здесь идет также проверка в РСА. Отправляем данные на:

## Рест natural-person-create

`/rest/default/client/natural-person-create`

### Пример отправленных значений

```JSON
{
  "birth_date": "1987-01-13",
  "driving_experience_started": null,
  "first_name": "Дмитрий",
  "gender": "M",
  "last_name": "Тарасов",
  "patronymic": "Сергеевич",
  "credential": [{
    "credential_type": 1,
    "expiration_date": "",
    "issue_date": "2007-08-08",
    "issue_point": "уава",
    "number": "206141",
    "series": "4007"
  }],
  "check_owner": true,
  "address": [{
    "address_type": 1,
    "postal_index": "198412",
    "country": "Россия",
    "city": "г Санкт-Петербург",
    "street": "ул Швейцарская",
    "house": "8",
    "housing": "2",
    "flat": null,
    "kladr": "7800000600001170028"
  }],
  "external_id": 1162
}
```

## Рест car-create

`/rest/default/client/car-create`

Введем и отправим дополнительные данные по автомобилю.

### Пример отправленных значений:

```JSON
{
  "car_mark": 429,
  "car_model": 1756,
  "car_modification": null,
  "cost": 0,
  "engine_displacement": null,
  "engine_power": 105,
  "credential": [{
    "credential_type": 4,
    "series": "77УН",
    "number": "850677",
    "issue_date": "2011-07-19",
    "issue_point": "",
    "expiration_date": null
  }],
  "external_id": 3504,
  "vin_number": "TMBED45J2B3209311",
  "number_plate": "Е271ХМ178",
  "manufacturing_date": "2011-01-01",
  "keys_count": null,
  "mileage": null,
  "car_body_number": "",
  "engine_power_kilowatt": 77.23,
  "chassis_number": "",
  "rsa_car_id": 366013810,
  "check_ts": true
}
```

## Рест insured-object

После нажатия кнопки "Сохранить" отправляем собранные значения на:

`/rest/default/client/insured-object`

### Пример отправленных значений

```JSON
{
  "owner": 3397,
  "beneficiary": 3397,
  "insurant": 3397,
  "drivers": [3691],
  "object_type": "car",
  "object_id": 4856
}
```

## Рест result_policy

`/policy/rest/result_policy/`

### Пример отправленных значений

```JSON
{
  "external_id": null,
  "insured_object": 4655,
  "status": "saved",
  "valid_from": "2016-10-15T0:00",
  "valid_to": "2017-10-14T23:59",
  "exploitation_period_begin_in_contract_time": "2016-10-15",
  "exploitation_period_end_in_contract_time": "2017-10-14",
  "result": 68306696,
  "policy_id": 4548
}
```

## result_policy

Получаем сформированный полис:

`/policy/rest/result_policy/1391/`

### Пример полученных данных

```JSON
{
  "result": 68306696,
  "insurance_company": 54188,
  "calculation": 6651480,
  "id": 1391,
  "created": "2016-10-14T18:35:01",
  "valid_from": "2016-10-15T00:00:00",
  "valid_to": "2017-10-14T23:59:00",
  "note": null,
  "contract_date": null,
  "external_id": null,
  "b2b_id": null,
  "status": "saved",
  "type_code": "OSAGO",
  "policy_id": "4548",
  "pnumber": null,
  "bso_series": null,
  "bso_number": null,
  "bso_external_id": null,
  "insured_object": 4655,
  "backend": 7,
  "documents": [],
  "requests": [],
  "responses": [],
  "creator": 10634,
  "actions_parameters": {},
  "exploitation_period_begin_in_contract_time": "2016-10-15",
  "exploitation_period_end_in_contract_time": "2017-10-14",
  "is_payed": null,
  "is_verified": false
}
```
## Получение объекта страхования

`rest/default/client/insured-object-create/4655`

### Пример полученных данных

```JSON
{
  "content_type": "car",
  "created": "2016-10-14T18:35:01.402",
  "creator": 10634,
  "drivers": [{
    "credential": [{
      "issue_date": null,
      "creator": 10634,
      "series": "77ОЕ",
      "number": "188766",
      "object_id": 3691,
      "issue_point": "РФ",
      "credential_type": 3,
      "content_type": 482,
      "expiration_date": null,
      "external_id": null,
      "id": 12327
    }],
    "first_name": "Юрий",
    "last_name": "Хомяков",
    "creator": 10634,
    "gender": "M",
    "rsa_check_owner_id": null,
    "driving_experience_started": "2009-07-14",
    "birth_date": "1985-09-18",
    "contact": [],
    "rsa_check_id": "76809675",
    "address": [],
    "patronymic": "Александрович",
    "external_id": "5728",
    "id": 3691
  }],
  "external_id": null,
  "id": 4655,
  "object": {
    "credential": [{
      "issue_date": "2011-07-19",
      "creator": 10634,
      "series": "77УН",
      "number": "850677",
      "object_id": 4856,
      "issue_point": "",
      "credential_type": 4,
      "content_type": 480,
      "expiration_date": null,
      "external_id": null,
      "id": 12362
    }],
    "keys_count": null,
    "manufacturing_date": "2011-01-01",
    "engine_power": "105.00",
    "car_modification": null,
    "creator": 10634,
    "car_model": 1756,
    "engine_displacement": null,
    "chassis_number": "",
    "rsa_car_id": "366013810",
    "car_body_number": "",
    "mileage": 77,
    "rsa_check_id": "76809816",
    "number_plate": "Е271ХМ178",
    "engine_power_kilowatt": "77.23",
    "car_mark": 429,
    "vin_number": "TMBED45J2B3209311",
    "external_id": "632",
    "id": 4856,
    "cost": "0.00"
  },
  "person": [{
    "legal_person": null,
    "natural_person": {
      "credential": [{
        "issue_date": "2007-08-08",
        "creator": 10634,
        "series": "4007",
        "number": "206141",
        "object_id": 3715,
        "issue_point": "уава",
        "credential_type": 1,
        "content_type": 482,
        "expiration_date": null,
        "external_id": null,
        "id": 12361
      }],
      "first_name": "Дмитрий",
      "last_name": "Тарасов",
      "creator": 10634,
      "gender": "M",
      "rsa_check_owner_id": "76809813",
      "driving_experience_started": null,
      "birth_date": "1987-01-13",
      "contact": [],
      "rsa_check_id": null,
      "address": [{
        "postal_index": "198412",
        "city": "г Санкт-Петербург",
        "flat": null,
        "creator": 10634,
        "country": "Россия",
        "region": null,
        "housing": "2",
        "object_id": 3715,
        "street": "ул Швейцарская",
        "kladr": "7800000600001170028",
        "content_type": 482,
        "house": "8",
        "address_type": 1,
        "id": 3441,
        "structure": null
      }],
      "patronymic": "Сергеевич",
      "external_id": "1786",
      "id": 3715
    },
    "role": [
      "insurant",
      "beneficiary",
      "owner"
    ],
    "id": 3397,
    "creator": null
  }]
}
```
