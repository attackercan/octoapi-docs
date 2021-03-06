# Статистика уязвимостей по временному отрезку
----

Получение статистики уязвимостей по каждому из уровней опасности
за `n` равных промежутков между двумя точками времени.


* **URL** `/projects/<id>/timerange/alerts`
* **Метод:**  `GET`
    
*  **Параметры URL запроса**
   * **Обязательные:**
     * `from` - UTC время начала периода в ISO формате (например `2022-04-05T13:22:44.463795`)
     * `to` - UTC время конца периода в ISO формате
   * **Опциональные:**
     * `steps` - целое число 
       временных отрезков между `from` и `to`. По умолчанию равно `1`


* **Успешное выполнение:** <br>
  Пример для `from=2022-04-05T14:24:56.651383&to=2022-04-05T13:24:56.651383&steps=4`
  
  * **Код:** `200` <br />
    **Тело:**  
    ```json
    [
       {
          "by_severity":{
             "0":0,
             "1":0,
             "10":0,
             "2":0,
             "3":0,
             "5":0,
             "8":1
          },
          "to":1649155196.651383,
          "from":1649154296.651383
       },
       {
          "by_severity":{
             "0":0,
             "1":0,
             "10":0,
             "2":1,
             "3":0,
             "5":0,
             "8":1
          },
          "to":1649156096.651383,
          "from":1649155196.651383
       },
       {
          "by_severity":{
             "0":0,
             "1":0,
             "10":0,
             "2":0,
             "3":0,
             "5":0,
             "8":0
          },
          "to":1649156996.651383,
          "from":1649156096.651383
       },
       {
          "by_severity":{
             "0":0,
             "1":0,
             "10":0,
             "2":2,
             "3":0,
             "5":0,
             "8":1
          },
          "to":1649157896.651383,
          "from":1649156996.651383
       }
    ]
    ```

 
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    **Тело:** `{ "error" : "Project 624b2cad2c04e8f0bdad7efc not found" }`
        
  * **Код:** `400` Пропущены обязательные параметры <br>
    **Тело:** `{ "error": "Query must contain 'to' and 'from' params" }`
    