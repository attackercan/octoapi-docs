# Настройка проекта
----
Изменение расписания, частоты сканирования, статуса и области ответственности проекта.

* **URL** `/projects/<id>`

* **Метод:**  `POST`
  
*  **Параметры тела запроса**

   **Опциональные:**
    - `name` - новое имя проекта
    - `status` - новый статус проекта. <br> Возможные значения:
        - `running` - запускает процесс сканирования проекта.
        - `paused` - ставит сканирование проекта на паузу.
        - `deleted` - безвозвратно удаляет проект (аналогично запросу `DELETE /projects/{id}`)
      
    - `root_domains` - новый список доменных имен с которых начинается скан. Может состоять из IPv4 адресов и сетей в CIDR нотации, доменных имен и вайлдкардов (`*.example.com`).
    Если тот или иной хост обнаруженный в процессе сканирования не является частью области ответственности заданного этим полем, он игнорируется.
    - `blacklist` - новый черный список; исключенные домены, адреса и сети. Может также включать вайлдкарды и регулярные выржения для фильтрации конкретных URI.
    - `allowed_ips` - новое список явно одобренных IP адресов и сетей. Если IP адрес найденного хоста не является явно одобренным,
  такой хост сохраняется в базу данных, но его сканирование не проводится.
    - `rescan_window` - новое окно времени суток для запуска повторных сканов. 
      Может быть равно `null` если нет предпочтительного времени для запуска сканирования.
    - `settings` - объект задающий настройки сканирования. Содержит следующие поля:
      - `tasks_frequencies` - объект хранящий настройки частоты запусков для каждого сканера.
        Эти настройки задают то насколько часто может сканироваться одна и та же сущность (порт, хост, ссылка) 
        и как часто происходят повторные сканирования сущностей проекта уже хранящихся в базе данных.
      -  `pipeline_tasks` - настройка соответствия типов сущностей и сканеров которые для них предназначены.
    - `schedule` - новый график предстоящих повторных сканов.


* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
    ```json
      {
       "id":"624b2cad2c04e8f0bdad7efc",
       "name":"project-name-1324",
       "settings":{
          "tasks_frequencies":{
             "altdns": {"days": 1},
             "amass": {"days": 1},
             "patator": {"days": 7},
             "crawler": {"days": 30},
             "dirsearch": {"days": 1},
             "ffuf": {"days": 7},
             "nessus": {"days": 10},
             "nmap": {"days": 1},
             "nuclei": {"days": 3},
             "phuipfpizdam": {"days": 20},
             "subjack": {"days": 1},
             "subfinder": {"days": 1},
             "waybackurls": {"days": 14},
             "wpscan": {"days": 7},
             "burp": {"days": 1},
             "grep": {"days": 1}
          },
          "pipeline_tasks":{
             "wildcard":[
                {
                   "name":"amass",
                   "options":{
                      "passive":true,
                      "noalts":true
                   }
                },
                {
                   "name":"altdns",
                   "options":{}
                },
                {
                   "name":"subfinder",
                   "options":{}
                }
             ],
             "host":[
                {
                   "name":"nmap",
                   "options":{}
                },
                {
                   "name":"subjack",
                   "options": {}
                },
                {
                   "name":"waybackurls",
                   "options": {}
                },
                {
                   "name":"nessus",
                   "options": {}
                }
             ],
             "port":[
                {
                   "name":"crawler",
                   "options": {}
                },
                {
                   "name":"dirsearch",
                   "options": {}
                },
                {
                   "name":"wpscan",
                   "options": {}
                },
                {
                   "name":"patator",
                   "options": {}
                }
             ],
             "transaction":[
                {
                   "name":"ffuf",
                   "options": {}
                },
                {
                   "name":"nuclei",
                   "options": {}
                },
                {
                   "name":"phuipfpizdam",
                   "options": {}
                },
                {
                   "name":"burp",
                   "options": {}
                },
                {
                   "name":"grep",
                   "options": {}
                }
             ]
          }
       },
       "root_domains":[
          "*.foo.bar.com",
          "xyz.bar.com",
          "11.22.33.44"
       ],
       "blacklist":{
          "host":[
             "baz.foo.bar.com",
             "regexp:[0-9]+\\\\.foo\\\\.bar\\\\.com"
          ],
          "ipv4":["11.22.44.55"],
          "ipv6":[],
          "url":[
             "https://definitely-yes\\\\.foo\\\\.bar\\\\.com/robots\\\\.txt"
          ]
       },
       "status":"new",
       "schedule":[
          {
             "tool":"altdns",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"amass",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"patator",
             "at":"2022-04-11T17:36:45.050000"
          },
          {
             "tool":"crawler",
             "at":"2022-04-05T05:36:45.050000"
          },
          {
             "tool":"dirsearch",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"ffuf",
             "at":"2022-04-11T17:36:45.050000"
          },
          {
             "tool":"nessus",
             "at":"2022-04-14T17:36:45.050000"
          },
          {
             "tool":"nmap",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"nuclei",
             "at":"2022-04-07T17:36:45.050000"
          },
          {
             "tool":"phuipfpizdam",
             "at":"2022-04-24T17:36:45.050000"
          },
          {
             "tool":"subjack",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"subfinder",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"waybackurls",
             "at":"2022-04-18T17:36:45.050000"
          },
          {
             "tool":"wpscan",
             "at":"2022-04-11T17:36:45.050000"
          },
          {
             "tool":"burp",
             "at":"2022-04-05T17:36:45.050000"
          },
          {
             "tool":"grep",
             "at":"2022-04-05T17:36:45.050000"
          }
       ],
       "allowed_ips":[
          "11.22.44.66",
          "11.22.44.0/29"
       ],
       "closest_rescan_at":"2022-04-05T05:36:45.050000",
       "rescan_window":null,
       "cdate":"2022-04-04T17:36:45.050000",
       "mdate":"2022-04-04T17:36:45.050000"
    }  
    ```

 
* **Ошибки:**
  * **Code:** `400` Ошибка в теле запроса <br />
    **Content:** `{ "error" : "Bad request body" }`
  * **Code:** `404` Проект не найден <br />
    **Content:** `{ "error" : "Project 624b2cad2c04e8f0bdad7efc not found" }`


* **Пример тела запроса:**
  ```json
    {
      "name": "project-test-1324",
      "rescan_window": {"from":  "03:20", "to": "04:20"},
      "status": "running",
      "allowed_ips": ["192.168.0.1/24"],
      "root_domains": ["*.foo.bar.com"],
      "blacklist": {
        "host":[
          "baz.foo.bar.com",
          "regexp:[0-9]+\\\\.foo\\\\.bar\\\\.com"
        ],
        "ipv4":["11.22.44.55"],
        "ipv6":[],
        "url":[
          "https://definitely-yes\\\\.foo\\\\.bar\\\\.com/robots\\\\.txt"
        ]
      },
      "settings": {
        "tasks_frequencies":{
          "altdns": {"days": 1},
          "crawler": {"days": 30},
          "dirsearch": {"days": 1},
          "ffuf": {"days": 7},
          "nessus": {"days": 10},
          "nmap": {"days": 1},
          "nuclei": {"days": 3},
          "grep": {"days": 1}
        },
        "pipeline_tasks":{
          "wildcard": [
            {
              "name":"amass",
              "options":{
                "passive":true,
                "noalts":true
              }
            },
            {
               "name": "altdns",
               "options":{}
            },
            {
               "name":"subfinder",
               "options":{}
            }
            ],
          "host":[
            {
               "name":"nmap",
               "options":{}
            },
            {
               "name":"subjack",
               "options": {}
            },
            {
               "name":"waybackurls",
               "options": {}
            },
            {
               "name":"nessus",
               "options": {}
            }
          ],
          "port":[
            {
              "name":"crawler",
              "options": {}
            },
            {
               "name":"dirsearch",
                "options": {}
            },
            {
              "name":"wpscan",
              "options": {}
            },
            {
              "name":"patator",
              "options": {}
            }
          ],
          "transaction":[
            {
               "name":"ffuf",
               "options": {}
            },
            {
               "name":"nuclei",
               "options": {}
            },
            {
               "name":"phuipfpizdam",
               "options": {}
            },
            {
               "name":"burp",
               "options": {}
            },
            {
               "name":"grep",
               "options": {}
            }
          ]
        }
      }
    }
  ```

