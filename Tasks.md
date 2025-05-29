# Настройки

### Установить переменную `base_url`
Environment -> New -> Environment -> `petstore_swagger_io`

|Variable|Type|Initial value|Current value|
|-|-|-|-|
|`base_url`|default|https://petstore.swagger.io/v2|https://petstore.swagger.io/v2|


Установить Environment (под иконкой аккаунта).

### Написание тестов
Перейти в запрос -> Scripts (под строкой для запроса)

<br>

---
---

<br>

## Задания

<br>

### Задание №1:
Отправьте `GET`-запрос на получение информации о конкретном питомце, используя ID существующего питомца. Например, отправьте запрос на `/pet/{id}`, заменив `{id}` на любой доступный идентификатор питомца из примеров документаций.

```
{{base_url}}/pet/1
```

![Задание 1](images/Задание1.png)

---

### Задание №2:
Создайте нового питомца с именем «Рыжик», породой «Кошка». Отправьте POST-запрос с нужным телом JSON. Убедитесь, что созданный питомец отображается в общем списке питомцев.

```
{
  "id": 123123,
  "category": {
    "id": 0,
    "name": "cats"
  },
  "name": "Рыжик",
  "photoUrls": [
    "string"
  ],
  "tags": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "status": "available"
}
```
![Задание 2](images/Задание2.png)
![Задание 2 (GET)](images/Задание2(GET).png)

---

### Задание №3:
Измените имя питомца «Рыжик» на «Барсик» и обновите другие атрибуты (например, статус). Для этого отправьте `PUT`-запрос. После обновления проверьте правильность изменений через `GET`-запрос.

```
{
    "id": 123123,
    "category": {
        "id": 0,
        "name": "cats"
    },
    "name": "Барсик",
    "photoUrls": [
        "string"
    ],
    "tags": [
        {
            "id": 0,
            "name": "string"
        }
    ],
    "status": "sold"
}
```

![Задание 3](images/Задание3.png)
![Задание 3 (GET)](images/Задание3(GET).png)

### Задание №4:
Удалите созданного питомца («Барсик») с помощью `DELETE`-запроса. Затем убедитесь, что питомец действительно удалён из системы, выполнив `GET`-запрос.

![alt text](images/Задание4.png)
![alt text](images/Задание4(GET).png)

### Задание №5:
Напишите набор автоматизированных тестов для проверки основных операций CRUD над питомцами: создание, чтение, обновление и удаление. Каждое действие должно проверяться отдельным тестовым сценарием.


***[Задание 1]***

Post-response 
```
pm.test("Статус ответа 200 Created", function () {
    pm.response.to.have.status(200);
});

pm.test("Ответ содержит данные о питомце", function(){
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("id");
    pm.expect(jsonData.name).to.equal("Рыжик");
    pm.expect(jsonData.category.name).to.equal("cats");
    pm.expect(jsonData.status).to.equal("available");
});
```

***[Задание 2]***

![alt text](images/Задание5(Задание2).png)

Переменные в collection (Pet store):
|||
|-|-|
|pet_name|Рыжик|
|pet_category|cats|
|pet_status|available|

Body (raw):
```
{
  "id": 123123,
  "category": {
    "id": 0,
    "name": "{{pet_category}}"
  },
  "name": "{{pet_name}}",
  "photoUrls": [
    "string"
  ],
  "tags": [
    {
      "id": 0,
      "name": "string"
    }
  ],
  "status": "{{pet_status}}"
}
```

Pre-request:
```
pm.collectionVariables.set("pet_name", "Рыжик");
pm.collectionVariables.set("pet_category", "cats");
pm.collectionVariables.set("pet_status", "available");

pm.test("Установленные данные о питомце", function(){
    pm.expect(pm.collectionVariables.get("pet_name")).to.equal("Рыжик");
    pm.expect(pm.collectionVariables.get("pet_category")).to.equal("cats");
    pm.expect(pm.collectionVariables.get("pet_status")).to.equal("available");
});
```

Post-response:
```
pm.test("Статус ответа 200 Created", function () {
    pm.response.to.have.status(200);
});

pm.test("Ответ содержит данные о питомце", function(){
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("id");
    pm.expect(jsonData.name).to.equal("Рыжик");
    pm.expect(jsonData.category.name).to.equal("cats");
    pm.expect(jsonData.status).to.equal("available");
});
```
