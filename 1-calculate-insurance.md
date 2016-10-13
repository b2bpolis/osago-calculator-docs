# 1. Рассчитайте страховку

Выбираем марку и модель автомобиля из списка.

## Проверка в РСА

После заполнения полей личных данных водителя создается объект с персоной и в это же время идет проверка в РСА.

Рест: `http://megaruss-client.cmios.ru/rest/default/client/natural-person-create`

Пример отправленных значений:

```
{
  "birth_date": "1985-09-18",
  "driving_experience_started": "2009-07-14",
  "first_name": "Юрий",
  "gender": "M",
  "last_name": "Хомяков",
  "patronymic": "Александрович",
  "credential": [{
    "credential_type": 3,
    "expiration_date": "",
    "issue_date": "",
    "issue_point": "РФ",
    "number": "188766",
    "series": "77ОЕ"
  }],
  "address": [],
  "check_driver": true,
  "external_id": 3758
}
```

Пример полученных данных

```
{
  "credential": [{
    "credential_type": 3,
    "issue_date": null,
    "issue_point": "РФ",
    "expiration_date": null,
    "number": "188766",
    "series": "77ОЕ"
  }],
  "address": [],
  "person": 3337,
  "id": 3655,
  "created": "2016-10-13T10:26:49.978",
  "modified": "2016-10-13T10:26:49.978",
  "external_id": 3758,
  "first_name": "Юрий",
  "last_name": "Хомяков",
  "patronymic": "Александрович",
  "birth_date": "1985-09-18",
  "gender": "M",
  "driving_experience_started": "2009-07-14"
}
```

После нажатия кнопки "расчитать" отправляются собранные данные на умный полис:

`http://megaruss-client.cmios.ru/rest/default/client/car-create`

```
{
  "car_mark": 303,
  "car_model": 36094,
  "car_modification": null,
  "manufacturing_date": "2016-01-01",
  "engine_power": 150,
  "cost": "0",
  "vin_number": null,
  "credential": [{
    "credential_type": 4,
    "series": "Не указана",
    "number": "Не указан",
    "issue_date": "2015-01-01",
    "issue_point": "Не указан"
  }]
}
```

`http://megaruss-client.cmios.ru/rest/default/client/insured-object`

```
{
  "drivers": [3655],
  "beneficiary": 3337,
  "insurant": 3337,
  "owner": 3337,
  "object_id": 4827,
  "object_type": "car"
}
```

`http://megaruss-client.cmios.ru/rest/full/calculation/`

```
{
  "id": 6651462,
  "car_cost": "0",
  "car_mark": 303,
  "car_model_group": 36091,
  "car_model": 36094,
  "car_manufacturing_year": 2016,
  "exploitation_area": 46217,
  "owner_registration": 46239,
  "drivers_count": 1,
  "driver_set": [{
    "age": 31,
    "expirience": 7
  }],
  "drivers_minimal_age": 31,
  "drivers_minimal_experience": 7,
  "is_new_car": false,
  "is_multidrive": false,
  "target_of_using": 46596,
  "insurance_duration": 46227,
  "contributory_scheme": 46209,
  "payment_type": 46593,
  "insurable_risk": 46219,
  "claim_form": 46206,
  "payment_form": 46591,
  "car_park_size": null,
  "deal_count": null,
  "deductible_value": null,
  "civil_liability_voluntary_cost": null,
  "is_prolongation": false,
  "signalization_type": [],
  "car_type": 46202,
  "insured_object": 4633,
  "count_years_break_even_insurance": 0,
  "calculation_at_date": "2016-10-14",
  "insurant_type": 53666,
  "insurance_type": 54191,
  "is_without_registration_car": false,
  "has_foreign_registration": false,
  "is_seasonal_using": false,
  "maximum_mass": null,
  "seats_number": null,
  "engine_power": 150,
  "is_legal_entity": false,
  "has_trailer": false,
  "is_aerographed": false,
  "casualty_cost": null,
  "antitheft": null,
  "term_of_credit": null,
  "is_accident_insured": false,
  "car_currency": 46211,
  "discount_for_vip": false,
  "earlier_insurance_cars_count": 1,
  "is_cmr_insured": false,
  "offers": [],
  "osago_losses_count": null,
  "car_maximum_mass": null,
  "has_autostart": false,
  "client_district": "",
  "deductible_type": null,
  "deductible_currency": null,
  "help_in_casualty": null,
  "is_armoured": false,
  "antitheft_model": [],
  "is_right_wheel": false,
  "vehicle_category": null,
  "policy_number": "",
  "manufacturing_country": null,
  "client_name": "",
  "maximum_speed": null,
  "post_code": "",
  "earlier_icompany": null,
  "date_of_exploitation_period": null,
  "insurance_duration_value": null,
  "optional_equipment": [],
  "car_modification": null,
  "special_program": null,
  "is_calculate_osago": true,
  "contract_duration": null,
  "external_id": "6201804552174126a87911faac156c3a",
  "credit_bank": null,
  "policy_series": "",
  "on_logic_version_date": "2100-01-01",
  "year_of_exploitation_period": null,
  "is_earlier_insurance": false,
  "cbm_casco": null,
  "is_calculate_gap": false,
  "car_park_size_relatives": false,
  "help_in_casualty_cost": null,
  "created_by_user": null,
  "car_weight": null,
  "packet_calculation": null,
  "company": null,
  "is_logic_version_date": false,
  "policy_area": 46595,
  "is_civil_liability_voluntary_insured": false,
  "is_under_warranty": false,
  "client_email": "",
  "is_deductible": false,
  "with_wear": false,
  "client_phone": "",
  "sattelite_model": null,
  "casualty": null,
  "av_help_in_casualty": null,
  "agency": null,
  "av_casualty": null,
  "mileage": 0,
  "engine_displacement": null,
  "is_credit": false,
  "osago_region": null,
  "help_in_casualty_duration": null,
  "prolongation": 51028,
  "created_at": "2016-10-13T11:00:58.140",
  "agent_commision": null,
  "using_api": true,
  "is_optional_equipment_insured": false,
  "osago_city": null,
  "passenger_place": null
}
```

### Получаем результаты расчета

`http://megaruss-client.cmios.ru/rest/v3/default/calculation/6651460/result/54188/?insurance_company=1&messages=1&vars=1`

```
[{
  "program": {
    "theft_sum": null,
    "title": "Нет",
    "sum": "7494.760000000000218278728425502777099609375",
    "tarif": null,
    "discount": 0,
    "option_set": [],
    "id": 298985,
    "harm_sum": null
  },
  "ins_dir_car_name": [],
  "insurance_type": "KASKO",
  "installment": {},
  "extra_parameters": [],
  "calculation": {
    "id": 6651463
  },
  "available_programs": [{
    "id": 298985,
    "title": "Нет"
  }],
  "variables": {
    "SEASONAL_USING_COEFF": 1,
    "kbm": "0.65",
    "BASE_RATE": "4118.0000",
    "requires_approval": null,
    "Susherb": null,
    "Tdo": null,
    "KBM": "0.65",
    "rsa_car_id": 36021873,
    "Skasko": null,
    "Sns": null,
    "TRAILER_COEFF": 1,
    "Sdo": null,
    "checked": ["76807343"],
    "Tusherb": null,
    "EXPLOTATION_AREA_COEFF": 2,
    "SHORT_INSURANCE_COEFF": 1,
    "Tns": null,
    "Tgo": null,
    "programm_setelem": null,
    "S": "7494.76",
    "T": null,
    "AGE_AND_EXPERIENCE_COEFF": 1,
    "program_pref": null,
    "kbm_list": ["0.65"],
    "NONOBSERVANCE_COEFF": 1,
    "LIMIT_CONTROL_COEFF": 1,
    "Sdgo": null,
    "CAR_POWER_COEFF": "1.4000",
    "Sgod": null
  },
  "is_selected": true,
  "messages": {
    "print_msg": [],
    "errors": [],
    "messages": [],
    "warnings": []
  },
  "add_risks": {
    "accident_sum": null,
    "cmr_sum": null,
    "optional_equipment_sum": null,
    "civil_liability_voluntary_sum": null,
    "cmr_tariff": null,
    "gap_sum": null
  },
  "is_default": true,
  "insurance_company": {
    "code": "osago_client",
    "logo": "media/logo_main_ins/megarus_1.png",
    "version": 54188,
    "id": 50918,
    "title": "Мегарусс-Д"
  },
  "insurer_data": {
    "response_url": null,
    "url_of_calculator": "",
    "b2b_id": null,
    "request_url": null,
    "quote_doc": null,
    "b2b_url": null
  },
  "bank": {},
  "sysinfo": {
    "calculation_error_message": null,
    "ctime": "2016-10-13T11:01:16.618",
    "result_counter": 1,
    "calculation_time": 8.694329,
    "value_rating_position": null,
    "value_rating": null,
    "is_recalculated": false
  },
  "id": 68306679,
  "available_installments": []
}]
```

[Назад](0-main.md)
