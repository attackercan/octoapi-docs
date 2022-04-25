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
        "project_id": "624b2cad2c04e8f0bdad7efc",
        "tasks_total_count": 1350,
        "tasks_done_count": 1222,
        "tasks_done_15_min": 22,
        "tasks_done_24_h": 1032,
        "hosts_count": 11,
        "urls_count": 3120,
        "critical_level_alerts": 2,
        "high_level_alerts": 3,
        "medium_level_alerts": 1,
        "low_level_alerts": 10,
        "info_level_alerts": 30,
        "new_data_level_alerts": 123,
        "unknown_level_alerts": 0,
        "scans": {
            "altdns": 5,
            "amass": 5,
            "patator": 5,
            "crawler": 5,
            "dirsearch": 5,
            "ffuf": 5,
            "nessus": 5,
            "nmap": 5,
            "nuclei": 5,
            "phuipfpizdam": 5,
            "subjack": 5,
            "subfinder": 5,
            "waybackurls": 5,
            "wpscan": 5,
            "burp": 5,
            "grep": 5
        }
    }

    ```

 
* **Ошибки:**
  * **Код:** `404` Проект не найден <br>
    **Тело:** `{ error : "Project 624b2cad2c04e8f0bdad7efc not found" }`
    