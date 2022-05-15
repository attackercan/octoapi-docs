# Отчеты Dirsearch
----

Выходные данные [dirsearch](https://www.kali.org/tools/dirsearch/) по указанному порту.


**Важно:** этот путь способен выдавать отчет в виде JSON документа или в виде HTML страницы (по умолчанию).
Для того чтобы получить HTML, необходимо добавить значение `text/html` в заголовок запроса `Accept`.

* **URL** `/projects/<id>/dirsearch/<host>/<port>`
  Важно
* **Метод:**  `GET`
* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
    ```json
    [
      {
        "contentLength": "243B ",
        "contentLengthBytes": 243,
        "contentType": "text/html; charset=UTF-8",
        "path": "%2e%2e//google.com",
        "redirect": "https://google-gruyere.appspot.com//google.com",
        "status": 302,
        "statusColorClass": "text-warning",
        "url": "https://google-gruyere.appspot.com:443/%2e%2e//google.com"
      },
      {
        "contentLength": "359B ",
        "contentLengthBytes": 359,
        "contentType": "text/html; charset=utf-8",
        "path": "0",
        "redirect": null,
        "status": 200,
        "statusColorClass": "text-success",
        "url": "https://google-gruyere.appspot.com:443/0"
      },
      {
        "contentLength": "359B ",
        "contentLengthBytes": 359,
        "contentType": "text/html; charset=utf-8",
        "path": "02",
        "redirect": null,
        "status": 200,
        "statusColorClass": "text-success",
        "url": "https://google-gruyere.appspot.com:443/02"
      },
      {
        "contentLength": "359B ",
        "contentLengthBytes": 359,
        "contentType": "text/html; charset=utf-8",
        "path": "05",
        "redirect": null,
        "status": 200,
        "statusColorClass": "text-success",
        "url": "https://google-gruyere.appspot.com:443/05"
      },
      {
        "contentLength": "359B ",
        "contentLengthBytes": 359,
        "contentType": "text/html; charset=utf-8",
        "path": "00",
        "redirect": null,
        "status": 200,
        "statusColorClass": "text-success",
        "url": "https://google-gruyere.appspot.com:443/00"
      },
      {
        "contentLength": "359B ",
        "contentLengthBytes": 359,
        "contentType": "text/html; charset=utf-8",
        "path": "01",
        "redirect": null,
        "status": 200,
        "statusColorClass": "text-success",
        "url": "https://google-gruyere.appspot.com:443/01"
      }
    ]
    ```
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    **Тело:** `{ error : "project not found" }`
    
  * **Код:** `404` Отчет не найден <br>
    **Тело:** `{"error": "no such report"}`
 