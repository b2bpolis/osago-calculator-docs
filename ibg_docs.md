

```python
import json
import requests
import xml.dom.minidom as minidom
import datetime as dt
```


```python
headers = {'Pragma': 'no-cache',
           'Origin': 'http://ibg.dev.b2bpolis.ru/',
           'Accept-Encoding': 'gzip, deflate',
           'Accept-Language': 'en-US,en;q=0.8,ru;q=0.6',
           'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36',
           'Content-Type': 'application/json;charset=UTF-8',
           'Accept': 'application/json, text/plain, */*',
           'Cache-Control': 'no-cache',
           'Connection': 'keep-alive',
           'Authorization': 'Token 840fc8bdabac6c26db9271f4a37906560d23e690',
           'Cookie': 'authToken=%2285e2c6e71b64098dfce40228f4c6169d431873fa%22; sessionid=2j7zgzw8lmd7qz0hwczx09ihq8753xqy'
           }
```

# 1 страница

## Получить id марки и модели авто


```python
url_car_mark = 'http://ibg.dev.b2bpolis.ru/rest/full/car_mark/'
```


```python
car_marks = {}
response_car_mark = requests.get(url=url_car_mark, headers=headers)
parsed_resp_car_mark = json.loads(response_car_mark.text)
for row in parsed_resp_car_mark:
    is_for, idx, title = row.values()
    car_marks[title] = idx
```


```python
car_mark = car_marks['Skoda']
car_mark
```


```python
url_car_models = 'http://ibg.dev.b2bpolis.ru/rest/full/car_mark/{}/car_model'.format(car_mark)
```


```python
car_models = {}
response_car_models = requests.get(url=url_car_models, headers=headers)
parsed_resp_car_models = json.loads(response_car_models.text)
for row in parsed_resp_car_models:
    title, idx, _, _ = row.values()
    car_models[title] = idx
```


```python
car_model = car_models['FABIA']
car_model
```

# Водитель


```python
url_person = 'http://ibg.dev.b2bpolis.ru/rest/default/client/natural-person-create'
```

для каждого свой расчёт

req_backup = u'{"birth_date":"1986-12-14","driving_experience_started":"2008-11-08","first_name":"\u041c\u0430\u0440\u0438\u043d\u0430","gender":"M","last_name":"\u0411\u0435\u0440\u0434\u043d\u0438\u043a\u043e\u0432\u0430","patronymic":"\u041d\u0438\u043a\u043e\u043b\u0430\u0435\u0432\u043d\u0430","credential":[{"credential_type":3,"expiration_date":"","issue_date":"","issue_point":"\u0420\u0424","number":"191882","series":"16\u0422\u041d"}],"address":[],"check_driver":true,"external_id":5884}'

external_id???


```python
req_driver = u'{"birth_date":"1986-12-14","driving_experience_started":"2008-11-08","address":[],"first_name":"\u041c\u0430\u0440\u0438\u043d\u0430","last_name":"\u0411\u0435\u0440\u0434\u043d\u0438\u043a\u043e\u0432\u0430","patronymic":"\u041d\u0438\u043a\u043e\u043b\u0430\u0435\u0432\u043d\u0430","credential":[{"credential_type":3,"issue_date":"","issue_point":"\u0420\u0424","number":"191882","series":"16\u0422\u041d"}], "check_driver":"true"}'
# json.loads(req_person, encoding='utf-8')
parsed_driver = json.loads(req_driver)
print json.dumps(parsed_driver, indent=4, sort_keys=True).decode('unicode-escape')
```

    {
        "address": [], 
        "birth_date": "1986-12-14", 
        "check_driver": "true", 
        "credential": [
            {
                "credential_type": 3, 
                "issue_date": "", 
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



```python
response_driver = requests.post(url=url_person, data=req_driver.encode('utf8'), headers=headers)
parsed_resp_driver = json.loads(response_driver.text)
print json.dumps(parsed_resp_driver, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
person = parsed_resp_driver['person']
driver_idx = parsed_resp_driver['id']
driver_exp = dt.datetime.now().year - int(parsed_resp_driver['driving_experience_started'][:4])
driver_age = (dt.datetime.now() - dt.datetime.strptime(parsed_resp_driver['birth_date'], '%Y-%m-%d')).days/365
print 'person:', person
print 'driver:', driver_idx
print 'driver age:', driver_exp
print 'driver exp:', driver_age
```

## Собственник, страхователь

## адрес


```python
url_addr = 'https://dadata.ru/api/v2/suggest/address'
```


```python
req_addr = u'{"query":"Чувашская республика - Чувашия, г Канаш, ул Комсомольская, д 54"}'
# json.loads(req_addr, encoding='utf-8')
parsed_addr = json.loads(req_addr)
print json.dumps(parsed_addr, indent=4, sort_keys=True).decode('unicode-escape')
```

    {
        "query": "Чувашская республика - Чувашия, г Канаш, ул Комсомольская, д 54"
    }



```python
response_addr = requests.post(url=url_addr, headers=headers, data=req_addr.encode('utf8'))
parsed_resp_addr = json.loads(response_addr.text)
addr = parsed_resp_addr['suggestions'][0]['value']
addr_data = parsed_resp_addr['suggestions'][0]['data']
print addr
addr_dict = {"address_type":1,             
             "country": addr_data['country'],
             "city": addr_data['city'],
             "street": addr_data["street"],
             "house": addr_data["house"],
             "flat": addr_data["flat"],
             "kladr": addr_data["kladr_id"],
             }             
# print json.dumps(addr_data, indent=4, sort_keys=True).decode('unicode-escape')
# print json.dumps(parsed_resp_addr, indent=4, sort_keys=True).decode('unicode-escape')
```

    Чувашская республика - Чувашия, г Канаш, ул Комсомольская, д 54
    {
        "suggestions": [
            {
                "data": {
                    "area": null, 
                    "area_fias_id": null, 
                    "area_kladr_id": null, 
                    "area_type": null, 
                    "area_type_full": null, 
                    "area_with_type": null, 
                    "beltway_distance": null, 
                    "beltway_hit": null, 
                    "block": null, 
                    "block_type": null, 
                    "block_type_full": null, 
                    "capital_marker": "0", 
                    "city": "Канаш", 
                    "city_area": null, 
                    "city_district": null, 
                    "city_district_fias_id": null, 
                    "city_district_kladr_id": null, 
                    "city_district_type": null, 
                    "city_district_type_full": null, 
                    "city_district_with_type": null, 
                    "city_fias_id": "6edd2a33-d03a-4d59-83c3-e14de6890a49", 
                    "city_kladr_id": "2100002300000", 
                    "city_type": "г", 
                    "city_type_full": "город", 
                    "city_with_type": "г Канаш", 
                    "country": "Россия", 
                    "fias_id": "806a5a7c-5c2e-430d-a693-818d4a5cdc35", 
                    "fias_level": "8", 
                    "flat": null, 
                    "flat_area": null, 
                    "flat_price": null, 
                    "flat_type": null, 
                    "flat_type_full": null, 
                    "geo_lat": "55.5091174", 
                    "geo_lon": "47.4850463", 
                    "house": "54", 
                    "house_fias_id": "806a5a7c-5c2e-430d-a693-818d4a5cdc35", 
                    "house_kladr_id": "2100002300000450003", 
                    "house_type": "д", 
                    "house_type_full": "дом", 
                    "kladr_id": "2100002300000450003", 
                    "okato": "97407000000", 
                    "oktmo": "97707000", 
                    "postal_box": null, 
                    "postal_code": "429330", 
                    "qc": null, 
                    "qc_complete": null, 
                    "qc_geo": "2", 
                    "qc_house": null, 
                    "region": "Чувашская республика", 
                    "region_fias_id": "878fc621-3708-46c7-a97f-5a13a4176b3e", 
                    "region_kladr_id": "2100000000000", 
                    "region_type": "Чувашия", 
                    "region_type_full": "Чувашия", 
                    "region_with_type": "Чувашская республика - Чувашия", 
                    "settlement": null, 
                    "settlement_fias_id": null, 
                    "settlement_kladr_id": null, 
                    "settlement_type": null, 
                    "settlement_type_full": null, 
                    "settlement_with_type": null, 
                    "square_meter_price": null, 
                    "street": "Комсомольская", 
                    "street_fias_id": "34bd7735-5cd3-4879-b583-6e1f788e895a", 
                    "street_kladr_id": "21000023000004500", 
                    "street_type": "ул", 
                    "street_type_full": "улица", 
                    "street_with_type": "ул Комсомольская", 
                    "tax_office": "2134", 
                    "tax_office_legal": null, 
                    "timezone": null, 
                    "unparsed_parts": null
                }, 
                "unrestricted_value": "Чувашская республика - Чувашия, г Канаш, ул Комсомольская, д 54", 
                "value": "Чувашская республика - Чувашия, г Канаш, ул Комсомольская, д 54"
            }
        ]
    }


## чел

req_backup = u'{"birth_date":"1984-03-14", "driving_experience_started":null,"first_name":"\u0420\u0430\u0448\u0438\u0434","gender":"M","last_name":"\u0413\u0430\u0444\u0438\u044f\u0442\u0443\u043b\u043b\u0438\u043d","patronymic":"\u041c\u0438\u043d\u0441\u0435\u0438\u0442\u043e\u0432\u0438\u0447","credential":[{"credential_type":1,"expiration_date":"","issue_date":"2006-02-28","issue_point":"\u0423\u0424\u041c\u0421","number":"412777","series":"9705"}],"check_owner":true,"address":[{"address_type":1,"postal_index":"429330","country":"\u0420\u043e\u0441\u0441\u0438\u044f","city":"\u0427\u0443\u0432\u0430\u0448\u0441\u043a\u0430\u044f \u0440\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0430 - \u0427\u0443\u0432\u0430\u0448\u0438\u044f","street":"\u0443\u043b \u041a\u043e\u043c\u0441\u043e\u043c\u043e\u043b\u044c\u0441\u043a\u0430\u044f","house":"54","housing":null,"flat":null,"kladr":"2100002300000450003"}],"external_id":3632}'


```python
req_owner = u'{"birth_date":"1984-03-14","driving_experience_started":null,"first_name":"\u0420\u0430\u0448\u0438\u0434","gender":"M","last_name":"\u0413\u0430\u0444\u0438\u044f\u0442\u0443\u043b\u043b\u0438\u043d","patronymic":"\u041c\u0438\u043d\u0441\u0435\u0438\u0442\u043e\u0432\u0438\u0447","credential":[{"credential_type":1,"expiration_date":"","issue_date":"2006-02-28","issue_point":"\u0423\u0424\u041c\u0421","number":"412777","series":"9705"}],"check_owner":true,"address":[' + json.dumps(addr_dict).decode('unicode-escape') + ']}'
# json.loads(req_owner, encoding='utf-8')
parsed_owner = json.loads(req_owner)
print json.dumps(parsed_owner, indent=4, sort_keys=True).decode('unicode-escape')
```

    {
        "address": [
            {
                "address_type": 1, 
                "city": "Канаш", 
                "country": "Россия", 
                "flat": null, 
                "house": "54", 
                "kladr": "2100002300000450003", 
                "street": "Комсомольская"
            }
        ], 
        "birth_date": "1984-03-14", 
        "check_owner": true, 
        "credential": [
            {
                "credential_type": 1, 
                "expiration_date": "", 
                "issue_date": "2006-02-28", 
                "issue_point": "УФМС", 
                "number": "412777", 
                "series": "9705"
            }
        ], 
        "driving_experience_started": null, 
        "first_name": "Рашид", 
        "gender": "M", 
        "last_name": "Гафиятуллин", 
        "patronymic": "Минсеитович"
    }



```python
response_owner = requests.post(url=url_person, data=req_owner.encode('utf8'), headers=headers)
parsed_resp_owner = json.loads(response_owner.text)
print json.dumps(parsed_resp_owner, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
owner = parsed_resp_owner['person']
owner_idx = parsed_resp_owner['id']
print 'owner:', owner
print 'owner_id:', owner_idx
```

# Машина


```python
url_car = 'http://ibg.dev.b2bpolis.ru/rest/default/client/car-create'
```

req_backup = u'{"car_mark":65176,"car_model":"65190","car_modification":null,"manufacturing_date":"2011-01-01","engine_power":"105","cost":"0","vin_number":null,"credential":[{"credential_type":4,"series":"\u041d\u0435 \u0443\u043a\u0430\u0437\u0430\u043d\u0430","number":"\u041d\u0435 \u0443\u043a\u0430\u0437\u0430\u043d","issue_date":"2015-01-01","issue_point":"\u041d\u0435 \u0443\u043a\u0430\u0437\u0430\u043d"}]}'


```python
req_car = u'{' + '"car_mark":{},"car_model":{},"car_modification":null,"cost":0,"engine_displacement":null,"engine_power":105,'.format(car_mark, car_model) + '"credential":[{"credential_type":4,"series":"77\u0423\u041d","number":"850677","issue_date":"2011-07-19","issue_point":"","expiration_date":null}],"external_id":1638,"vin_number":"TMBED45J2B3209311","number_plate":"\u0415271\u0425\u041c178","manufacturing_date":"2011-01-01","keys_count":null,"mileage":"20","car_body_number":"","engine_power_kilowatt":77.23,"chassis_number":""}'
parsed_car = json.loads(req_car)
print json.dumps(parsed_car, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_car = requests.post(url=url_car, data=req_car.encode('utf8'), headers=headers)
parsed_resp_car = json.loads(response_car.text)
print json.dumps(parsed_resp_car, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
car_idx = parsed_resp_car['id']
car_idx
```

# insured-object


```python
url_ins_obj = 'http://ibg.dev.b2bpolis.ru/rest/default/client/insured-object'
```

'{"drivers":[3432,3442],"beneficiary":3111,"insurant":3111,"owner":3111,"object_id":4404,"object_type":"car"}'


```python
drivers = [driver_idx]
req_ins_obj = u'{' + '"drivers":{0},"beneficiary":{1},"insurant":{1},"owner":{1},"object_id":{2},"object_type":"car"'.format(drivers, person, car_idx) + '}'
# json.loads(req_ins_obj, encoding='utf-8')
parsed_ins_obj = json.loads(req_ins_obj)
print json.dumps(parsed_ins_obj, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_ins_obj = requests.post(url=url_ins_obj, data=req_ins_obj.encode('utf8'), headers=headers)
parsed_resp_ins_obj = json.loads(response_ins_obj.text)
print json.dumps(parsed_resp_ins_obj, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
idx_ins_obj = parsed_resp_ins_obj['id']
idx_ins_obj
```

# calculation


```python
url_calc = 'http://ibg.dev.b2bpolis.ru/rest/full/calculation/'
```

req_calc_backup =  '{"id":6651177,"car_cost":"0","car_mark":65176,"car_model_group":65189,"car_model":"65190","car_manufacturing_year":2011,"exploitation_area":50906,"owner_registration":46239,"drivers_count":2,"driver_set":[{"age":29,"expirience":7},{"age":31,"expirience":7}],"drivers_minimal_age":29,"drivers_minimal_experience":7,"is_new_car":false,"is_multidrive":false,"target_of_using":46596,"insurance_duration":46227,"contributory_scheme":46209,"payment_type":46593,"insurable_risk":46219,"claim_form":46206,"payment_form":46591,"car_park_size":null,"deal_count":null,"deductible_value":null,"civil_liability_voluntary_cost":null,"is_prolongation":false,"signalization_type":[],"car_type":46202,"insured_object":4187,"count_years_break_even_insurance":0,"calculation_at_date":"2016-11-08","insurant_type":53666,"insurance_type":37564,"is_without_registration_car":false,"has_foreign_registration":false,"is_seasonal_using":false,"maximum_mass":null,"seats_number":null,"engine_power":105,"is_legal_entity":false,"is_calculate_osago":true,"created_by_user":null,"external_id":"38d1c57ff0ec40a5878203e959a12ea1","is_aerographed":false,"casualty_cost":null,"antitheft":null,"term_of_credit":null,"is_accident_insured":false,"car_currency":46211,"discount_for_vip":false,"earlier_insurance_cars_count":1,"is_cmr_insured":false,"offers":[],"osago_losses_count":null,"car_maximum_mass":null,"has_autostart":false,"client_district":"","deductible_type":null,"deductible_currency":null,"help_in_casualty":null,"is_armoured":false,"antitheft_model":[],"is_right_wheel":false,"vehicle_category":null,"policy_number":"","manufacturing_country":null,"client_name":"","maximum_speed":null,"post_code":"","earlier_icompany":null,"date_of_exploitation_period":null,"insurance_duration_value":null,"car_modification":null,"special_program":null,"contract_duration":null,"credit_bank":null,"policy_series":"","on_logic_version_date":"2100-01-01","year_of_exploitation_period":null,"is_earlier_insurance":false,"cbm_casco":null,"is_calculate_gap":false,"car_park_size_relatives":false,"help_in_casualty_cost":null,"car_weight":null,"packet_calculation":null,"company":null,"has_trailer":false,"is_logic_version_date":false,"policy_area":46595,"is_civil_liability_voluntary_insured":false,"is_under_warranty":false,"client_email":"","is_deductible":false,"with_wear":false,"client_phone":"","sattelite_model":null,"casualty":null,"av_help_in_casualty":null,"agency":null,"av_casualty":null,"mileage":0,"engine_displacement":null,"is_credit":false,"osago_region":null,"help_in_casualty_duration":null,"prolongation":51028,"created_at":"2016-11-07T18:44:03.047","agent_commision":null,"using_api":true,"is_optional_equipment_insured":false,"osago_city":null,"passenger_place":null}'


```python
req_calc = '{' + '"car_cost":"0","car_mark":{},"car_model":"{}"'.format(car_mark, car_model) + ',"car_manufacturing_year":2011,"exploitation_area":50906,"owner_registration":46239,"drivers_count":1,"driver_set":[{' + '"age":{},"expirience":{}'.format(driver_age, driver_exp) + '}],"drivers_minimal_age":29,"drivers_minimal_experience":7,"is_new_car":false,"is_multidrive":false,"target_of_using":46596,"insurance_duration":46227,"is_prolongation":false,"car_type":46202,' + '"insured_object":{},'.format(idx_ins_obj) + '"calculation_at_date":"2016-11-09","insurant_type":53666,"insurance_type":37564,"is_without_registration_car":false,"has_foreign_registration":false,"is_seasonal_using":false,"engine_power":105,"is_legal_entity":false,"is_calculate_osago":true,"policy_area":46595,"prolongation":51028,"created_at":"2016-11-07T18:44:03.047","using_api":true}'
parsed_calc = json.loads(req_calc)
print json.dumps(parsed_calc, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_calc = requests.post(url=url_calc, data=req_calc.encode('utf8'), headers=headers)
parsed_resp_calc = json.loads(response_calc.text)
idx_calc = parsed_resp_calc['id']
print idx_calc
print json.dumps(parsed_resp_calc, indent=4, sort_keys=True).decode('unicode-escape')
```

# first_final


```python
url_first_final = 'http://ibg.dev.b2bpolis.ru/rest/v3/default/calculation/{}/result/67405/?insurance_company=1&messages=1&vars=1'.format(idx_calc)
```


```python
req_first_final = ''
response_first_final = requests.post(url=url_first_final, headers=headers)
parsed_resp_first_final = json.loads(response_first_final.text)
# print json.dumps(parsed_resp_first_final, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
rsa_car_id = parsed_resp_first_final[0]['variables']['rsa_car_id']
result_idx = parsed_resp_first_final[0]['id']
print rsa_car_id
print result_idx
```

## обновляем тачку

req_car_create_back =  '{"car_mark":65176,"car_model":"65190","car_modification":null,"cost":0,"engine_displacement":null,"engine_power":105,"credential":[{"credential_type":4,"series":"77\u0423\u041d","number":"850677","issue_date":"2011-07-19","issue_point":"","expiration_date":null}],"external_id":1638,"vin_number":"TMBED45J2B3209311","number_plate":"\u0415271\u0425\u041c178","manufacturing_date":"2011-01-01","keys_count":null,"mileage":"20","car_body_number":"","engine_power_kilowatt":77.23,"chassis_number":"","rsa_car_id":"366013810","check_ts":true}' --compressed'


```python
req_car_upd = u'{' + '"car_mark":{},"car_model":{},"car_modification":null,"cost":0,"engine_displacement":null,"engine_power":105,'.format(car_mark, car_model) + '"credential":[{"credential_type":4,"series":"77\u0423\u041d","number":"850677","issue_date":"2011-07-19","issue_point":"","expiration_date":null}],"external_id":1638,"vin_number":"TMBED45J2B3209311","number_plate":"\u0415271\u0425\u041c178","manufacturing_date":"2011-01-01","keys_count":null,"mileage":"20","car_body_number":"","engine_power_kilowatt":77.23,"chassis_number":"",' + '"rsa_car_id":"{}","check_ts":true'.format(rsa_car_id) + '}'
parsed_car_upd = json.loads(req_car_upd)
print json.dumps(parsed_car_upd, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_car_upd = requests.post(url=url_car, data=req_car_upd.encode('utf8'), headers=headers)
parsed_resp_car_upd = json.loads(response_car_upd.text)
car_upd_idx = parsed_resp_car_upd['id']
car_upd_idx
# print json.dumps(parsed_resp_car_upd, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
drivers = [driver_idx]
req_upd_ins_obj = u'{' + '"drivers":{0},"beneficiary":{1},"insurant":{1},"owner":{1},"object_id":{2},"object_type":"car"'.format(drivers, owner, car_upd_idx) + '}'
# json.loads(req_ins_obj, encoding='utf-8')
parsed_upd_ins_obj = json.loads(req_upd_ins_obj)
print json.dumps(parsed_upd_ins_obj, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_upd_ins_obj = requests.post(url=url_ins_obj, data=req_upd_ins_obj.encode('utf8'), headers=headers)
parsed_resp_upd_ins_obj = json.loads(response_upd_ins_obj.text)
# print json.dumps(parsed_resp_ins_obj, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
print json.dumps(parsed_resp_upd_ins_obj, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
idx_upd_ins_obj = parsed_resp_upd_ins_obj['id']
idx_upd_ins_obj
```

# 2 страница

# result_policy


```python
url_res_pol = 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/'
```


```python
req_res_pol = '{' + '"insured_object":{},"status":"saved","valid_from":"2016-11-09T0:00","valid_to":"2017-11-08T23:59","exploitation_period_begin_in_contract_time":"2016-11-09","exploitation_period_end_in_contract_time":"2017-11-08", "result":{}'.format(idx_upd_ins_obj, result_idx) + '}'
parsed_res_pol = json.loads(req_res_pol)
print json.dumps(parsed_res_pol, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_res_pol = requests.post(url=url_res_pol, data=req_res_pol.encode('utf8'), headers=headers)
parsed_resp_res_pol = json.loads(response_res_pol.text)
print json.dumps(parsed_resp_res_pol, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
policy_idx = parsed_resp_res_pol['id']
policy_idx
```


```python
URL = 'http://ibg.dev.b2bpolis.ru#/calculator/osago/{}/formation_policy/step_2?policy={}'.format(idx_calc, policy_idx)
URL
```

# save and print policy


```python
url_save = 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/{}/save/'.format(policy_idx)
```


```python
req_save =  u'{"citizenship":"\u0420\u0424","phone":null,"email":null,"place_of_work":null,"post":null,"relationship_with_official":null,"beneficial_owner":null,"technical_inspection_type":"\u0414\u0438\u0430\u0433\u043d\u043e\u0441\u0442\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043a\u0430\u0440\u0442\u0430","number_eaisto":"201411280939075812119","technical_inspection_issue_date":null,"technical_inspection_period":"2016-11-28","technical_inspection_exists":true,"technical_inspection_message":"\u041d\u0430\u0439\u0434\u0435\u043d\u0430 \u0437\u0430\u043f\u0438\u0441\u044c \u043e \u043f\u0440\u043e\u0445\u043e\u0436\u0434\u0435\u043d\u0438\u0438 \u0422\u041e \u0434\u0430\u043d\u043d\u044b\u043c \u0430\u0432\u0442\u043e\u043c\u043e\u0431\u0438\u043b\u0435\u043c."}'
parsed_save = json.loads(req_save)
print json.dumps(parsed_save, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
response_save = requests.post(url=url_save, data=req_save.encode('utf8'), headers=headers)
response_save
```


```python
parsed_resp_save = json.loads(response_save.text)
print json.dumps(parsed_resp_save, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
url_print = 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/{}/print/'.format(policy_idx)
```


```python
req_print = ''
response_print = requests.post(url=url_print, headers=headers)
parsed_resp_print = json.loads(response_print.text)
print json.dumps(parsed_resp_print, indent=4, sort_keys=True).decode('unicode-escape')
```


```python
print 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/{}/print/1/'.format(policy_idx)
print 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/{}/print/2/'.format(policy_idx)
print 'http://ibg.dev.b2bpolis.ru/policy/rest/result_policy/{}/print/3/'.format(policy_idx)
```
