# Общая статистика проекта
----

Информация о количестве найденных уязвимостей по уровням критичности,
количестве найденных хостов, портов и ссылок,
статистика выполнения задач сканирования.

* **URL** `/projects/<id>/stats/`
* **Метод:**  `GET`
* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
    ```json
    {
       "hosts_count":5,
       "urls_count": 1345,
       "tasks":{
          "total_count":1344,
          "total_finished":1002,
          "finished_in_last_15_min":15,
          "finished_in_last_24_hours":458
       },
       "alerts":{
          "by_level":{
             "critical":0,
             "high":2,
             "medium":0,
             "low":0,
             "info":4,
             "new_data":0,
             "unknown":0
          },
          "by_status":{
             "done":0,
             "false_positive":0,
             "in_progress":0,
             "new":6,
             "not_interesting":0
          }
       },
       "scans":{
          "altdns":20,
          "amass":20,
          "burp":20,
          "crawler":20,
          "dirsearch":20,
          "ffuf":20,
          "grep":20,
          "nessus":20,
          "nmap":20,
          "nuclei":20,
          "patator":20,
          "phuipfpizdam":20,
          "subfinder":20,
          "subjack":20,
          "waybackurls":20,
          "wpscan":20
       }
    }
    ```

 
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    **Тело:** `{ error : "Project 624b2cad2c04e8f0bdad7efc not found" }`
    