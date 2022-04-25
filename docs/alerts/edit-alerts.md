# Редактирование тревог
----

Редактирование тревог.


* **URL** `/alerts/<id>`
* **Метод:**  `PATCH`

*  **Параметры тела запроса**
   *  **Опциональные:**
      - `severity` - новое значение уровня опасности. Целое число. Возможные значения:
        - `0` - "неизвестный"
        - `1` - "новые данные"
        - `2` - "информация"
        - `3` - "низкий"
        - `5` - "средний"
        - `8` - "высокий"
        - `10` - "критический"
     - `recommendation` - новое значение рекомендации по устранению опасности
      - `recommendation_detail` - новое значение подробной рекомендации по устранению опасности
      - `recommendation_detail` - новое значение подробной рекомендации по устранению опасности
      - `issue_detail` - новое значение подробного описания пердмета тревоги
      - `comment` - новое значение комментария
      - `status` - новое значение статуса. Возможные значения:
        - `NEW`
        - `FALSE_POSITIVE`
        - `NOT_INTERESTING`
        - `IN_PROGRESS`
        - `DONE`
    
* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
    ```json
      {
         "id":"624ca02385387b7ca299cf62",
         "title":"Some Title 4",
         "severity":2,
         "tool":"burp",
         "description":"Descr",
         "project_id":"6242e65464096281adcb7b03",
         "last_seen":"2021-03-29T10:20:53.349000",
         "comment":null,
         "content_digest":"3d29a06974d89a7d07a79221555d6bece035aac0",
         "issue_detail":"Some issue detail 6",
         "recommendation":"Some Recommendation 2",
         "recommendation_detail":"Some Recommendation detail",
         "extra":{
            "foo":"bar"
         },
         "status":"NEW",
         "cdate":"2022-03-29T10:20:53.349000",
         "mdate":"2020-03-29T10:20:53.349000"
      }
    ```
* **Ошибки:**
  * **Code:** `404` Тревога не найдена <br />
    **Content:** `{ "error" : "alert 624ca02385387b7ca299cf61 not found" }`
