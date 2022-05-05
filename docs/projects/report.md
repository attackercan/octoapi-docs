# Отчеты по проекту
----

Отчет по проекту включающий в себя оценку угрозы проекту, 
статистику по разных видам тревог, список всех хостов, а также список всех тревог.

**Важно:** этот путь способен выдавать отчет в виде JSON документа или в виде HTML страницы (по умолчанию).
Для того чтобы получить JSON, необходимо добавить значение `application/json` в заголовок запроса `Accept`.

* **URL** `/projects/<id>/report/`
  Важно
* **Метод:**  `GET`
* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
    ```json
      {
        "project_name": "project-test-1324",
        "rating": 237.52,
        "rating_letter_grade": "A",
        "alert_group_stats": {
          "critical": 0,
          "high": 2,
          "medium": 0,
          "low": 0,
          "info": 4,
          "new_data": 0,
          "unknown": 0
        },
        "hosts": [
          {
            "host": "33.34.55.121",
            "ports": [
              {
                "port_number": 8000,
                "protocol": "http",
                "last_seen": "2022-01-30T07:23:06"
              },
              {
                "port_number": 443,
                "protocol": "http",
                "last_seen": "2022-01-30T07:23:06"
              }
            ]
          }
        ],
        "alerts": [
          {
            "title": "Some Title 1",
            "count": 1,
            "tool": "burp",
            "recommendation": "Some Recommendation",
            "http_request": null,
            "http_response": null,
            "severity": "high",
            "description": "Descr"
          },
          {
            "title": "Some Title 12",
            "count": 2,
            "tool": "burp",
            "recommendation": "Some Recommendation 2",
            "http_request": null,
            "http_response": null,
            "severity": "high",
            "description": "Descr"
          },
          {
            "title": "Some Title xxx",
            "count": 1,
            "tool": "burp",
            "recommendation": "Some Recommendation",
            "http_request": null,
            "http_response": null,
            "severity": "info",
            "description": "Descr"
          },
          {
            "title": "Some Title 2",
            "count": 1,
            "tool": "burp",
            "recommendation": "Some Recommendation 2",
            "http_request": null,
            "http_response": null,
            "severity": "info",
            "description": "Descr"
          },
          {
            "title": "Some Title 3",
            "count": 1,
            "tool": "nuclei",
            "recommendation": "Some Recommendation 2",
            "http_request": null,
            "http_response": null,
            "severity": "info",
            "description": "Descr"
          },
          {
            "title": "Some Title 4",
            "count": 1,
            "tool": "burp",
            "recommendation": "Some Recommendation 2",
            "http_request": null,
            "http_response": null,
            "severity": "info",
            "description": "Descr"
          }
        ]
      }
    ```
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    **Тело:** `{ error : "Project 624b2cad2c04e8f0bdad7efc not found" }`
    