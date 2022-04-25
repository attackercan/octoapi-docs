# Проверка области ответственности проекта
----

С помощью этого эндпойнта можно проверить попадает ли тот или иной хост либо ссылка
в область ответственности проекта.



* **URL** `/projects/<id>/check/`

* **Метод:**  `POST`
  
*  **Параметры тела запроса**
   * **Обязательные:**
     * `urls` - список проверяемых ссылок (может быть пустым)
     * `domains` - список проверяемых доменных имен (может быть пустым)
     * `ipv4` - список проверяемых IPv4 адресов (может быть пустым)

<br>

* **Успешное выполнение:**
  * **Код:** `200` <br />
    
    ***Коды вердиктов***:
      * `0` - не в области ответственности
      * `1` - в области ответственности, но IP не одобрен явно (см. [параметры настройки проекта](/projects/modify-project/))
      * `2` - в области ответственности и может подлежать сканированию

  *  **Тело:**  
      ```json
      {
        "url_verdicts": {
          "https://definitely-yes.foo.bar.com/robots.txt?whatever=1234": 0,
          "https://definitely-yes.foo.bar.com/another.txt?whatever=1234": 2,
          "https://sub.foo.bar.com/another.txt": 1
        },
        "domain_verdicts": {
          "out.of.scope.com": 0,
          "foo.bar.com": 1,
          "432.foo.bar.com": 0,
          "sub.foo.bar.com": 1,
          "definitely-yes.foo.bar.com": 2,
          "ip.out.of.scope": 0
        },
        "ipv4_verdicts": {
          "11.22.44.6": 0,
          "11.111.44.6": 0,
          "11.22.44.55": 0
        }
      }
      ```

 
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    
  *  **Тело:** `{ error : "Project 624b2cad2c04e8f0bdad7efc not found" }`
    

* **Пример тела запроса:**
  ```json
    {
      "urls": [
        "https://definitely-yes.foo.bar.com/robots.txt?whatever=1234",
        "https://definitely-yes.foo.bar.com/another.txt?whatever=1234",
        "https://sub.foo.bar.com/another.txt"
      ],
      "domains": [
        "out.of.scope.com",
        "foo.bar.com",
        "432.foo.bar.com",
        "sub.foo.bar.com",
        "definitely-yes.foo.bar.com",
        "ip.out.of.scope"
      ],
      "ipv4": [
        "11.22.44.6",
        "11.111.44.6",
        "11.22.44.55"
      ]
    }
  ```
