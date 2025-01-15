---
order: 96
---

# Документация по API

Документация по API находится по ссылке: `https://ВАШ_ЛИЧНЫЙ_КАБИНЕТ.sf-cloud.ru/api/docs`  

---

## Получить Авторизационный токен (срок жизни - 12 часов)

`POST` `https://auth.sf-cloud.ru/realms/factory/protocol/openid-connect/token`

+++ Curl

```bash
curl --location --request POST 'https://auth.sf-cloud.ru/realms/factory/protocol/openid-connect/token' \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'client_id=<Поддомен вашего ЛК без .sf-cloud.ru>' \
  --data-urlencode 'username=<Имя пользователя, например, username@example.com>' \
  --data-urlencode 'password=<Ваш пароль>' \
  --data-urlencode 'grant_type=password'
```

+++ Python

```python
import requests

login='test@test.com'
password='password'

auth_token = requests.post(
  'https://auth.sf-cloud.ru/realms/factory/protocol/openid-connect/token',
  data={
      "client_id": "<Поддомен вашего ЛК без .sf-cloud.ru>",
      "username": login,
      "password": password,
      "grant_type": "password"
  },
  headers={
      "Accept": "application/json",
      "Content-Type": "application/x-www-form-urlencoded"
  }
).json().get("access_token", None)
```

+++ Ruby

```ruby
require 'uri'
require 'net/http'
require 'json'

login = 'test@test.com'
password = 'password'
address_prefix = '<Поддомен вашего ЛК без .sf-cloud.ru>'
url = URI("https://auth.sf-cloud.ru/realms/factory/protocol/openid-connect/token")

response = Net::HTTP.start(url.host, url.port, use_ssl: true) do |http|
  request = Net::HTTP::Post.new(url)
  request.set_form_data({
    client_id: address_prefix,
    username: login,
    password: password,
    grant_type: 'password'
  })
  http.request(request)
end

auth_token = JSON.parse(response.body)['access_token'] || nil
```

+++

## Получить Годовой Авторизационный токен (например, для Telegram-бота)

+++ Curl

```bash
curl https://<CLIENT>.sf-cloud.ru/api/admin/agent-token -H "Accept: application/json"  -H "Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_12_ЧАСОВОЙ_ТОКЕН_С_ПРЕДЫДУЩЕГО_ШАГА>"
```

+++

## Получить список живых хостов

`GET` `/api/hosts/?alive=1&hidden=0`

+++ Curl

```bash
curl -X 'GET' \
  'https://<CLIENT>.sf-cloud.ru/api/hosts/?alive=1&hidden=0' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'
```

+++ Python

```python
import requests

url = 'https://<CLIENT>.sf-cloud.ru/api/hosts/'
params = {'alive': 1, 'hidden': 0}

headers = {'Accept': 'application/json', 'Authorization': 'Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'}
alive_hosts = requests.get(url, params=params, headers=headers).json()
```

+++ Ruby

```ruby
require 'net/http'
require 'uri'
require 'json'

url = URI('https://<CLIENT>.sf-cloud.ru/api/hosts/')
params = { alive: 1, hidden: 0 }

url.query = URI.encode_www_form(params)

headers = { 'Accept': 'application/json', 'Authorization': '<АВТОРИЗАЦИОННЫЙ_ТОКЕН>' }
response = Net::HTTP.get_response(url, headers)

alive_hosts = JSON.parse(response.body)
```

+++

==- Параметры

Полный список параметров находится в Swagger в Вашем Личном Кабинете. Ссылка на Swagger: `https://ВАШ_ЛИЧНЫЙ_КАБИНЕТ.sf-cloud.ru/api/docs`  

Ниже приведён краткий пример, какие параметры возможно передать  

| Parameter           | Value         | Description                                                                                                                                                                                         |
| ------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `project_id`        | string        | Поиск хостов с указанным ID проекта                                                                                                                                                                 |
| `alive`             | 1 or 0        | Поиск живых или мертвых хостов                                                                                                                                                                      |
| `hidden`            | 1 or 0        | Отображение скрытых хостов - всегда нужно передавать 0                                                                                                                                              |
| `address`           | IP string     | Поиск хостов с указанным IP. Формат: `127.0.0.1`                                                                                                                                                    |
| `domain`            | domain string | Поиск хостов с указанными доменными именами. Формат: 'google.com'                                                                                                                                   |
| `port_last_seen`    | int seconds   | Включить в ответ хосты, порты которых были замечены в период от указанной даты до текущего момента                                                                                                  |
| `ports.port_number` | int           | Поиск хостов с указанными портами                                                                                                                                                                   |
| `sort`              | string        | Сортировка хостов. Варианты: `host`, `last_seen`, `mdate`, `alert_count`. По умолчанию `host`. Каждое значение может быть с перфиксом `-`, что означает сортировку в обратном порядке: `-last_seen` |
| `limit`             | int           | Количество хостов, которое будет показано на одной странице                                                                                                                                         |
| `skip`              | int           | Количество хостов, которое будет пропущено для пагинации                                                                                                                                            |
| `all`               | 1             | Если указано - `skip` и `limit` игнорируются, запрашивает все хосты                                                                                                                                 |

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
    "count": 1,
    "items": [
        {
            "id": "78aaf796-690d-11ef-97d2-ff886da63cd5",
            "ipv4": "1.1.1.1",
            "domains": [],
            "ports": [
                {
                    "tlp": "tcp",
                    "port_number": 443,
                    "open": true,
                    "found_by": [
                        "infrascan-1057",
                        "nmap-1051"
                    ],
                    "protocol": "tcpwrapped",
                    "info": {
                        "conf": "8",
                        "name": "tcpwrapped",
                        "method": "probed",
                        "cpelist": []
                    },
                    "last_seen": "2024-12-13T15:17:36.202215+00:00",
                    "history": [
                        {
                            "open": true,
                            "at": "2024-09-03T09:50:14.810007+00:00",
                            "task_id": null
                        }
                    ],
                    "banned": false,
                    "cdate": "2024-09-03T09:50:14.810007+00:00",
                    "mdate": "2024-12-13T15:17:36.202215+00:00"
                }
            ],
            "project_id": "77ce5908-690d-11ef-97d2-5bfbdf422a92",
            "found_by": [],
            "hidden": false,
            "scope_status": 2,
            "cdate": "2024-09-02T09:26:43.064509+00:00",
            "mdate": "2024-12-13T04:17:27.370275+00:00",
            "last_seen": "2024-12-13T15:17:36.202215+00:00",
            "ping_ok": true,
            "whois": {
                "HUGE WHOIS SKIPPED": "EXAMPLE"
            },
            "alert_stats": {
                "medium": 0,
                "high": 0,
                "critical": 0
            },
            "latest_infrascan_report": "/link/report.pdf",
            "pin": null,
            "comment": null,
            "capacity": null,
            "scope_history": [],
            "storing_reason": "in root scope",
            "originator": "user"
        }
    ]
}
```

===

## Получить уязвимости за неделю

`GET` `/api/alerts/?active=1&limit=1000`  

+++ Curl

```bash
curl -X GET "https://<CLIENT>.sf-cloud.ru/api/alerts/?active=1&limit=1000&gt-last_seen=$(date +%s --date '7 days ago')&sort=-last_seen" \
  -H "accept: application/json" \
  -H "Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>"

```

+++ Python

```python
import requests
import time
from datetime import datetime, timezone, timedelta

from_ = int((datetime.now(tz=timezone.utc) - timedelta(days=7)).timestamp())
url = f"https://<CLIENT>.sf-cloud.ru/api/alerts/?active=1&gt-last_seen={from_}&limit=10000&sort=-last_seen"
headers = {'accept': 'application/json', 'Authorization': 'Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'}
response = requests.get(url, headers=headers)

alerts = response.json()
```

+++ Ruby

```ruby
require 'net/http'
require 'json'
require 'time'

url = URI('https://<CLIENT>.sf-cloud.ru/api/alerts/?active=1&limit=1000&gt-last_seen=' + (Time.now.to_i - 7 * 24 * 60 * 60).to_s + '&sort=-last_seen')
headers = { 'Accept': 'application/json', 'Authorization': '<АВТОРИЗАЦИОННЫЙ_ТОКЕН>' }
response = Net::HTTP.get_response(url, headers)

data = JSON.parse(response.body)
```

+++

==- Параметры

Полный список параметров находится в Swagger в Вашем Личном Кабинете. Ссылка на Swagger: `https://ВАШ_ЛИЧНЫЙ_КАБИНЕТ.sf-cloud.ru/api/docs`  

Ниже приведён краткий пример, какие параметры возможно передать  

| Parameter       | Value                  | Description                                                                                                                                                                                         |
| --------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `project_id`    | string(uuid)           | Поиск язвимостей по указанному проекту                                                                                                                                                              |
| `active`        | 1 or 0                 | Поиск активных уязвимостей                                                                                                                                                                          |
| `$gt-last_seen` | string(unix_timestamp) | Поиск уязвимостей, найденных, после указанной даты                                                                                                                                                  |
| `sort`          | string                 | Сортировка хостов. Варианты: `host`, `last_seen`, `mdate`, `alert_count`. По умолчанию `host`. Каждое значение может быть с перфиксом `-`, что означает сортировку в обратном порядке: `-last_seen` |

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
  "count": 1,
  "items": [
    {
      "id": "4bb2361c-9fb0-11ef-be26-d785a4534483",
      "title": "Open Port Re-check",
      "severity": 10,
      "current_generators": [
        {
          "id": "8e1f7ac0-80b4-11ef-b80d-a77d69e3e153",
          "template": "infrascan-1057",
          "tool": "infrascan",
          "hidden": false
        }
      ],
      "all_generators": [
        {
          "id": "8e1f7ac0-80b4-11ef-b80d-a77d69e3e153",
          "template": "infrascan-1057",
          "tool": "infrascan",
          "hidden": false
        }
      ],
      "description": "detailed description",
      "project": {
        "id": "77ce5908-690d-11ef-97d2-5bfbdf422a92",
        "name": "Project Name"
      },
      "last_seen": "2024-12-13T02:52:16.886892+00:00",
      "last_rediscovery": "2024-12-13T02:52:16.886892+00:00",
      "comment": "example",
      "content_digest": "60afe804b722ad4919f51c41f590ec9c",
      "issue_detail": "",
      "recommendation": "Steps to resolve this issue include : steps. . .",
      "recommendation_detail": "",
      "extra": {
        "port": "0 / tcp / ",
        "vuln_hostname": "1.1.1.1",
        "severity": 0,
        "pluginname": "Open Port Re-check",
        "description": "description",
        "plugin_raw_output": "Port 443 was detected as being open but is now unresponsive\n",
        "cve_details": [],
        "solution": "Steps to resolve this issue include : steps. . .",
        "risk_information": {
          "risk_factor": "None"
        },
        "ref_information": {
          "reference_values_EXAMPLE": []
        },
        "plugin_information": {
          "example": 1
        },
        "vuln_information": null,
        "see_also": null,
        "vuln_ip": "1.1.1.1"
      },
      "status": "VERIFIED",
      "verified": true,
      "active": true,
      "masked": false,
      "recurring": false,
      "masks": [],
      "host": "1.1.1.1",
      "affected_ipv4": [],
      "affected_domains": [],
      "netloc": null,
      "categories": [],
      "cvss3": null,
      "cvss2": null,
      "cvss2_vector": null,
      "cvss3_vector": null,
      "exploit_available": null,
      "cve_id": null,
      "bdu_id": null,
      "cdate": "2024-11-10T22:08:20.357296+00:00",
      "mdate": "2024-12-13T02:52:16.886892+00:00",
      "unfound_by": [],
      "deactivation_reason": null,
      "deactivation_reason_detail": null
    }
  ]
}
```

===

## Добавление хостов в проект

`POST` `https://<CLIENT>.sf-cloud.ru/api/projects/<project_UUID>/scope-extension/`  
Новые домены будут дописываться в конец уже существующего скоупа.

### Body запроса

+++ Домены

```json
{
    "root_domains": ["domain.com", "some.domain.com"]
}
```

+++ IP-адреса

```json
{
    "root_ips": ["25.65.9.0", "44.43.1.0"]
}
```

+++ Домены и IP

```json
{
    "root_domains": ["domain.com", "some.domain.com"],
    "root_ips": ["25.65.9.0", "44.43.1.0"]
}
```

+++

### Код

+++ Curl

```bash
curl -X POST \
  https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid str>/scope-extension/ \
  -H 'Content-Type: application/json' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>' \
  -d '{"root_domains": ["domain.com", "some.domain.com"]}'
```

+++ Python

```python
import requests
import json

url = 'https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/'
headers = {'Content-Type': 'application/json', 'accept': 'application/json', 'Authorization': 'Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'}
payload = {"root_domains": ["domain.com", "some.domain.com"]}
response = requests.post(url, headers=headers, data=json.dumps(payload))

scope_settings = response.json()
```

+++ Ruby

```ruby
require 'net/http'
require 'json'

url = URI('https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/')
headers = {'Content-Type': 'application/json', 'Accept': 'application/json', 'Authorization': '<АВТОРИЗАЦИОННЫЙ_ТОКЕН>' }
data = {"root_domains": ["domain.com", "some.domain.com"]}

response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::Post.new(url)
  headers.each do |key, value|
    request[key] = value
  end
  request.body = data.to_json
  http.request(request)
end

data = JSON.parse(response.body)
```

+++

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
    "root_domains": [
        "example.domain"
    ],
    "root_ips": [
        "1.1.1.1"
    ],
    "blacklist": {
        "domains": [],
        "ipv4": [],
        "ipv6": [],
        "url": [],
        "netlocs": [],
        "url_exceptions": [],
        "domains_exceptions": [],
        "ipv4_exceptions": []
    },
    "manual_ip_approve": false,
    "hide_unreachable": false,
    "number_of_domains": 1,
    "number_of_wildcards": 0,
    "number_of_ips": 1,
    "number_of_subnets": 0,
    "number_of_ips_in_subnets": 0
}
```

[!badge size="m" text="Status code: 400" variant="danger"]

```json
{
    "detail": [
        {
            "type": "ip_v4_address",
            "loc": [...],
            "msg": "Input is not a valid IPv4 address",
            "input": "1"
        },
        {
            "type": "ip_v4_network",
            "loc": [...],
            "msg": "Input is not a valid IPv4 network",
            "input": "1"
        }
    ]
}
```

===

## Удаление хостов из проекта

`POST` `https://<CLIENT>.sf-cloud.ru/api/projects/<project_UUID>/scope-deletion`  
Тело запроса точно такое же, как и у Scope extension

### Код

+++ Curl

```bash
curl -X POST \
  https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid str>/scope-deletion/ \
  -H 'Content-Type: application/json' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>' \
  -d '{"root_domains": ["domain.com", "some.domain.com"]}'
```

+++ Python

```python
import requests
import json

url = 'https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/'
headers = {'Content-Type': 'application/json', 'accept': 'application/json', 'Authorization': 'Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'}
payload = {"root_domains": ["domain.com", "some.domain.com"]}
response = requests.post(url, headers=headers, data=json.dumps(payload))

scope_settings = response.json()
```

+++ Ruby

```ruby
require 'net/http'
require 'json'

url = URI('https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/')
headers = {'Content-Type': 'application/json', 'Accept': 'application/json', 'Authorization': '<АВТОРИЗАЦИОННЫЙ_ТОКЕН>' }
data = {"root_domains": ["domain.com", "some.domain.com"]}

response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::Post.new(url)
  headers.each do |key, value|
    request[key] = value
  end
  request.body = data.to_json
  http.request(request)
end

data = JSON.parse(response.body)
```

+++

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
    "root_domains": [
        "example.domain"
    ],
    "root_ips": [
        "1.1.1.1"
    ],
    "blacklist": {
        "domains": [],
        "ipv4": [],
        "ipv6": [],
        "url": [],
        "netlocs": [],
        "url_exceptions": [],
        "domains_exceptions": [],
        "ipv4_exceptions": []
    },
    "manual_ip_approve": false,
    "hide_unreachable": false,
    "number_of_domains": 1,
    "number_of_wildcards": 0,
    "number_of_ips": 1,
    "number_of_subnets": 0,
    "number_of_ips_in_subnets": 0
}
```

===

## Добавление swager endpoint

`POST` `https://<CLIENT>.sf-cloud.ru/api/httpreqs/`  

### Код

+++ Curl

```bash

curl -X 'POST' \
    'https://<CLIENT>.sf-cloud.ru/api/httpreqs/' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>' \
    -d '{
    "project id": "<project-id>",
    "url": "https://your.domain.com/api/openapi.json",
    "method": "GET",
    "request_headers": {},
    "request_body": ""
}'
```

+++ Python

```python
import requests
import json

url = 'https://<CLIENT>.sf-cloud.ru/api/httpreqs/'
headers = {'Content-Type': 'application/json', 'accept': 'application/json', 'Authorization': 'Bearer <АВТОРИЗАЦИОННЫЙ_ТОКЕН>'}
payload = {"project id": "<project-id>", "url": "https://your.domain.com/api/openapi.json", "method": "GET", "request_headers": {}, "request_body": ""}
response = requests.post(url, headers=headers, data=json.dumps(payload))

scope_settings = response.json()
```

+++ Ruby

```ruby
require 'net/http'
require 'json'

url = URI('https://<CLIENT>.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/')
headers = {'Content-Type': 'application/json', 'Accept': 'application/json', 'Authorization': '<АВТОРИЗАЦИОННЫЙ_ТОКЕН>' }
data = {"project id": "<project-id>", "url": "https://your.domain.com/api/openapi.json", "method": "GET", "request_headers": {}, "request_body": ""}

response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::Post.new(url)
  headers.each do |key, value|
    request[key] = value
  end
  request.body = data.to_json
  http.request(request)
end

data = JSON.parse(response.body)
```

+++

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

===
