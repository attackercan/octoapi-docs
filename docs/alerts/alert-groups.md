# Тревоги по группам
----

Получение списка хранящихся в базе тревог сгруппированных по заглавиям и опасности.


* **URL** `/alert-groups/`
* **Метод:**  `GET`

*  **Параметры URL запроса**
   *  **Опциональные:**
      - `project_id` - идентификатор проекта которому должны принадлежать тревоги
      - `skip` - пропустить `n` записей (для пагинации). По умолчанию `skip=0`
      - `limit` - ограничить количество записей числом `n` (для пагинации). По умолчанию `limit=20`
      - `sort` - параметр сортировки. Может быть одним из:
        - `title` - алфавитная сортировка по заглавиям (от меньшего)
        - `-title` - алфавитная сортировка по заглавиям (от большего)
        - `severity` - сортировка по опасности (от меньшего)
        - `-severity` - сортировка по опасности (от большего)
        - `last_seen` - сортировка по времени последнего обнаружения (от меньшего)
        - `-last_seen` - сортировка по времени последнего обнаружения (от большего)
      - `substring` - искать тревоги с заданной подстрокой в заглавии.
      - `severity` - искать тревоги с заданным уровнем опасности в заглавии. Может принимать несколько значений сразу.
      - `status` - искать тревоги с заданным статусом в заглавии. Может принимать несколько значений сразу. 
      

* **Успешное выполнение:**
  * **Код:** `200` <br />
    **Тело:**  
   ```json
    {
        "total_groups_count": 3,
        "groups": [
            {
                "id": "Some Title 12__6242e65364096281adcb7b02__8",
                "count": 2,
                "title": "Some Title 12",
                "severity": 8,
                "last_seen": "2022-03-29T10:20:53.349000",
                "project_id": "6242e65364096281adcb7b02",
                "project_name": "Some project name"
            },
            {
                "id": "Some Title 2__6242e65364096281adcb7b02__8",
                "count": 1,
                "title": "Some Title 2",
                "severity": 2,
                "last_seen": "2022-03-29T10:20:53.349000",
                "project_id": "6242e65364096281adcb7b02",
                "project_name": "Some project name"
            },
            {
                "id": "Some Title xxx__6242e65364096281adcb7b02__8",
                "count": 1,
                "title": "Some Title xxx",
                "severity": 2,
                "last_seen": "2022-03-29T10:20:53.349000",
                "project_id": "6242e65364096281adcb7b02",
                "project_name": "Some project name"
            }
        ]
    }

    ```
    * `total_groups_count` - общее количество групп соответствущих запросу
   