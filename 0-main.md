# Описание работы калькулятора ОСАГО

Документация поможет вам решить вопрос получения расчета по ОСАГО без необходимости работать напрямую с интерфейсом Умного полиса.

## Вход в систему

Перед расчетом необходимо авторизоваться в системе и получить токен.

### Получение токена

Чтобы получить токен, необходимо отправить POST запрос содержащий имя пользователя и пароль.

Адрес для отправки запроса: `http://enter.b2bpolis.ru/rest/v3/default/obtain-token`

## Получение данных из справочников

Справочники нужны для получения идентификаторов данных и их сопоставления с параметрами, которые используются в расчете, т.е. чтобы получить варианты значений и их id в системе.

### Список используемых справочников

Справочник                                                       | Значение
---------------------------------------------------------------- | -------------------
`http://megaruss-client.cmios.ru/rest/full/target_of_using/`     | Цель использования
`http://megaruss-client.cmios.ru/rest/full/insurance_duration/`  | Срок страхования
`http://megaruss-client.cmios.ru/rest/full/exploitation_area/`   | Регион эксплуатации
`http://megaruss-client.cmios.ru/rest/full/insurant_type/`       | Тип страхования
`http://megaruss-client.cmios.ru/rest/full/contributory_scheme/` | Порядок уплаты
`http://megaruss-client.cmios.ru/rest/full/insurable_risk/`      | Страховой риск
`http://megaruss-client.cmios.ru/rest/full/car_type/`            | Тип ТС
`http://megaruss-client.cmios.ru/rest/full/owner_registration/`  | Регион регистрации
`http://megaruss-client.cmios.ru/rest/full/car_mark/`            | Марки автомобилей

## Этапы расчета калькулятора

1. Рассчитайте страховку
2. Заполните заявление
3. Подпишите договор
4. Оплатите
5. Получите полис

[Подробнее][de2dc09a]

[de2dc09a]: 1-calculate-insurance.md "Этапы расчета калькулятора"
