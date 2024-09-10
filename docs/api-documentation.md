---
order: 96
---

# Документация по API

Здесь описаны несколько полезных API-эндпоинтов для получения интересующих вас данных

---

## Получить API токен

+++ Curl

```bash
curl --location --request POST 'https://keycloak.sf-cloud.ru/auth/realms/scanfactory/protocol/openid-connect/token' \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'client_id=<Поддомен вашего ЛК>' \
  --data-urlencode 'username=test@test.com' \
  --data-urlencode 'password=password' \
  --data-urlencode 'grant_type=password'
```

+++ Python

```python
import requests

login='test@test.com'
password='password'

auth_token = requests.post(
  'https://keycloak.sf-cloud.ru/auth/realms/scanfactory/protocol/openid-connect/token',
  data={
      "client_id": "<Поддомен вашего ЛК>",  # <prefix>.sf-cloud.ru
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

+++ PHP

```php
<?php

$login = 'test@test.com';
$password = 'password';
$address_prefix = '<Поддомен вашего ЛК>'; // <prefix>.sf-cloud.ru
$url = 'https://keycloak.sf-cloud.ru/auth/realms/scanfactory/protocol/openid-connect/token';

$data = http_build_query([
    'client_id' => $address_prefix,
    'username' => $login,
    'password' => $password,
    'grant_type' => 'password'
]);

$options = [
    'http' => [
        'header'  => "Content-Type: application/x-www-form-urlencoded\r\n" .
                     "Accept: application/json\r\n",
        'method'  => 'POST',
        'content' => $data
    ]
];

$response = file_get_contents($url, false, stream_context_create($options));
$auth_token = json_decode($response, true)['access_token'] ?? null;

?>
```

+++ Ruby

```ruby
require 'uri'
require 'net/http'
require 'json'

login = 'test@test.com'
password = 'password'
address_prefix = '<Поддомен вашего ЛК>'  # <prefix>.sf-cloud.ru
url = URI("https://keycloak.sf-cloud.ru/auth/realms/scanfactory/protocol/openid-connect/token")

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

## Получить список живых хостов

Необходимо авторизоваться, зайти по вашей ссылке scanfactory `/api/hosts/?alive=1&hidden=0`:  

[`https://test.sf-cloud.ru/api/hosts/?alive=1&hidden=0`](https://test.sf-cloud.ru/api/hosts/?alive=1&hidden=0)

Или выполнить следующий код

+++ Curl

```bash
curl -X 'GET' \
  'https://test.sf-cloud.ru/api/hosts/?alive=1&hidden=0&token=<your_api_token>' \
  -H 'accept: application/json'
```

+++ Python

```python
import requests

url = 'https://test.sf-cloud.ru/api/hosts/'
params = {'alive': 1, 'token': '<your_api_token>'}

headers = {'Accept': 'application/json'}
alive_hosts = requests.get(url, params=params, headers=headers).json()
```

+++ PHP

```php
<?php

$url = 'https://test.sf-cloud.ru/api/hosts/?alive=1&hidden=0&token=<your_api_token>';
$options = array(
  'http' => array(
    'header' => 'accept: application/json'
  )
);

$response = file_get_contents($url, false, stream_context_create($options));
$data = json_decode($response, true);
?>
```

+++ Ruby

```ruby
require 'net/http'
require 'uri'
require 'json'

url = URI('https://test.sf-cloud.ru/api/hosts/')
params = { alive: 1, token: '<your_api_token>' }

url.query = URI.encode_www_form(params)

headers = { 'Accept': 'application/json' }
response = Net::HTTP.get_response(url, headers)

alive_hosts = JSON.parse(response.body)
```

+++

==- Параметры

Каждый из них опционален

Parameter | Value | Description
--- | --- | ---
`project_id` | string | Поиск хостов с указанным ID проекта
`alive` | 1 or 0 | Поиск живых или мертвых хостов
`address` | IP string | Поиск хостов с указанным IP. Формат: `127.0.0.1`
`domain` | domain string | Поиск хостов с указанными доменными именами. Формат: 'google.com'
`found_by` | string | Поиск хостов, найденных указанным [сканером](./scanners.md)
`pending` | True/1 or string/int | Поиск ожидающих хостов (True или 1), поиск активных (любое дргугое значение)
`hidden` | 1 or 0 | Поиск скрытых хостов  
`include_dead_ports` | 1 or 0 | Включить в ответ порты, помеченные мертвыми, по умолчанию 0
`ports.port_number` | int | Поиск хостов с указанными портами
`$gt-ports.last_seen` | string(datetime) | Поиск хостов, у которых хотя бы один порт был замечен **после** указанного времени. Формат '`2022-06-14T17:27:25.024141`'
`$lt-ports.last_seen` | string(datetime) | Поиск хостов, у которых хотя бы один порт был замечен **до** указанного времени. Формат '`2022-06-14T17:27:25.024141`'
`sort` | string | Сортировка хостов. Варианты: `host`, `last_seen`, `mdate`, `alert_count`. По умолчанию `host`. Каждое значение может быть с перфиксом `-`, что означает сортировку в обратном порядке: `-last_seen`
`skip` | int | Количество хостов, которое будет пропущено для пагинации
`limit` | int | Количество хостов, которое будет показано на одной странице
`all` | 1 | Если указано `skip` и `limit` игнорируется, запрашивает все хосты

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
  "count": 1,
  "items": [
    {
      "ipv4": "51.32.2.0",
      "domains": [
        {
          "domain": "foo.bar.com",
          "found_by": [
            "altdns-4"
          ],
          "alert_stats": {
            "critical": 0,
            "high": 0,
            "medium": 0
          }
        }
      ],
      "ports": [
        {
          "port_number": 0,
          "found_by": [
            "nmap-5"
          ],
          "open": true,
          "protocol": "string",
          "info": {
            "name": "ssh",
            "product": "OpenSSH",
            "version": "7.9p1 Debian 10+deb10u2",
            "extrainfo": "protocol 2.0",
            "ostype": "Linux",
            "method": "probed",
            "conf": "10",
            "cpelist": [
              "cpe:/a:openbsd:openssh:7.9p1",
              "cpe:/o:linux:linux_kernel"
            ]
          },
          "history": [
            {
              "open": true,
              "at": "2022-06-14T17:27:25.024141",
              "task_id": "<project_id: uuid | str>"
            }
          ],
          "last_seen": "2022-06-14T17:27:25.024141",
          "cdate": "2022-06-14T17:27:25.024141",
          "mdate": "2022-06-14T17:27:25.024141"
        }
      ],
      "project_id": "<project_id: uuid | str>",
      "found_by": "altdns",
      "pending": true,
      "hidden": true,
      "ping_ok": true,
      "cdate": "2022-06-14T17:27:25.024141",
      "mdate": "2022-06-14T17:27:25.024141",
      "last_seen": "2022-06-14T17:27:25.024141",
      "alert_stats": {
        "critical": 0,
        "high": 0,
        "medium": 0
      },
      "whois": {}
    }
  ]
}
```

===

## Получить уязвимости за неделю

Необходимо авторизоваться, зайти по вашей ссылке scanfactory `/api/alerts/?active=1`:  

[`https://test.sf-cloud.ru/api/alerts/?active=1`](https://test.sf-cloud.ru/api/alerts/?active=1)

Или выполнить следующий код

+++ Curl

```curl
curl -X 'GET' \
  https://test.sf-cloud.ru/api/alerts/\?active\=1\&gt-last_seen\="$(date +%s --date '7 days ago')"\&sort=-last_seen&token=<your_api_token> \
  -H 'accept: application/json'
```

+++ Python

```python
import requests
import time

url = 'https://test.sf-cloud.ru/api/alerts/?active=1&gt-last_seen={}&sort=-last_seen&token=<your_api_token>'.format(int(time.time()) - 7*24*60*60)
headers = {'accept': 'application/json'}
response = requests.get(url, headers=headers)

alerts = response.json()
```

+++ PHP

```php
<?php
$url = 'https://test.sf-cloud.ru/api/alerts/?active=1&gt-last_seen=' . (time() - 7 * 24 * 60 * 60) . '&sort=-last_seen&token=<your_api_token>';
$options = array(
    'http' => array(
        'header' => 'accept: application/json'
    )
);
$response = file_get_contents($url, false, stream_context_create($options));
$alerts = json_decode($response, true);
?>
```

+++ Ruby

```ruby
require 'net/http'
require 'json'
require 'time'

url = URI('https://test.sf-cloud.ru/api/alerts/?active=1&gt-last_seen=' + (Time.now.to_i - 7 * 24 * 60 * 60).to_s + '&sort=-last_seen&token=<your_api_token>')
headers = {
  'accept' => 'application/json'
}
response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::Get.new(url)
  headers.each do |key, value|
    request[key] = value
  end
  http.request(request)
end

data = JSON.parse(response.body)
```

+++

==- Параметры

Parameter | Value | Description
--- | --- | ---
`project_id` | string(uuid) | Поиск язвимостей по указанному проекту
`active` | 1 or 0 | Поиск активных уязвимостей
`$gt-last_seen` | string(unix_timestamp) | Поиск уязвимостей, найденных, после указанной даты
`sort` | string | Сортировка хостов. Варианты: `host`, `last_seen`, `mdate`, `alert_count`. По умолчанию `host`. Каждое значение может быть с перфиксом `-`, что означает сортировку в обратном порядке: `-last_seen`

==- Пример ответа

[!badge size="m" text="Status code: 200" variant="success"]

```json
{
  "count": 1,
  "items": [
    {
      "ipv4": "51.32.2.0",
      "domains": [
        {
          "domain": "foo.bar.com",
          "found_by": [
            "altdns-4"
          ],
          "alert_stats": {
            "critical": 0,
            "high": 0,
            "medium": 0
          }
        }
      ],
      "ports": [
        {
          "port_number": 0,
          "found_by": [
            "nmap-5"
          ],
          "open": true,
          "protocol": "string",
          "info": {
            "name": "ssh",
            "product": "OpenSSH",
            "version": "7.9p1 Debian 10+deb10u2",
            "extrainfo": "protocol 2.0",
            "ostype": "Linux",
            "method": "probed",
            "conf": "10",
            "cpelist": [
              "cpe:/a:openbsd:openssh:7.9p1",
              "cpe:/o:linux:linux_kernel"
            ]
          },
          "history": [
            {
              "open": true,
              "at": "2022-06-14T17:27:25.024141",
              "task_id": "<project_id: uuid | str>"
            }
          ],
          "last_seen": "2022-06-14T17:27:25.024141",
          "cdate": "2022-06-14T17:27:25.024141",
          "mdate": "2022-06-14T17:27:25.024141"
        }
      ],
      "project_id": "<project_id: uuid | str>",
      "found_by": "altdns",
      "pending": true,
      "hidden": true,
      "ping_ok": true,
      "cdate": "2022-06-14T17:27:25.024141",
      "mdate": "2022-06-14T17:27:25.024141",
      "last_seen": "2022-06-14T17:27:25.024141",
      "alert_stats": {
        "critical": 0,
        "high": 0,
        "medium": 0
      },
      "whois": {}
    }
  ]
}
```

===

## Добавление хостов в проект

Чтобы добавить хосты в проект, необходимо выполнить `POST` запрос на `https://test.sf-cloud.ru/api/projects/<project_UUID>/scope-extension/`
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
  https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/?token=<your_api_token> \
  -H 'Content-Type: application/json' \
  -H 'accept: application/json' \
  -d 'PAYLOAD FROM "Body запроса" SECTION'

```

+++ Python

```python
import requests
import json

url = 'https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/?token=<your_api_token>'
headers = {'Content-Type': 'application/json', 'accept': 'application/json'}
payload = {'PAYLOAD FROM "Body запроса" SECTION'}
response = requests.post(url, headers=headers, data=json.dumps(payload))

project_config = response.json()

```

+++ PHP

```php
<?php

$url = 'https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/?token=<your_api_token>';
$data = ['PAYLOAD FROM "Body запроса" SECTION'];

$options = [
    'http' => [
        'header' => "Content-type: application/json\r\n" .
                    "Accept: application/json\r\n",
        'method' => 'POST',
        'content' => json_encode($data)
    ]
];

$context = stream_context_create($options);
$result = file_get_contents($url, false, $context);

$response = json_decode($result);
?>

```

+++ Ruby

```ruby
require 'net/http'
require 'json'

url = URI('https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-extension/?token=<your_api_token>')
headers = {
  'Content-Type' => 'application/json',
  'accept' => 'application/json'
}
data = {'PAYLOAD FROM "Body запроса" SECTION'}
response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::POST.new(url)
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
    "scope_settings": {
        "root_domains": ["домен.который.уже.был.в.настройках", "domain.com", "some.domain.com"],
        "root_ips": ["44.0.1.1" "25.65.9.0", "44.43.1.0"],
        "blacklist": {
            "domains": [],
            "ipv4": [],
            "ipv6": [],
            "url": ["http://somedomain.com/?\\?path=([^&=?]+)"],
            "url_exceptions": [],
            "domains_exceptions": [],
            "ipv4_exceptions": []
        },
        "manual_ip_approve": false,
        "exclude_private_ips": true,
        "hide_unreachable": false
    }
}
```

[!badge size="m" text="Status code: 400" variant="danger"]

```json
{"error":"bad request body","field":"scope_settings.root_ips: 127.0.0.1 is not allowed (private IP)"}
```

===

## Удаление хостов из проекта

Чтобы удалить хост из проекта, необходимо выполнить `POST` запрос на `https://test.sf-cloud.ru/api/projects/<project_UUID>/scope-deletion`

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
  https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/?token=<your_api_token> \
  -H 'Content-Type: application/json' \
  -H 'accept: application/json' \
  -d 'PAYLOAD FROM "Body запроса" SECTION'

```

+++ Python

```python
import requests
import json

url = 'https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/?token=<your_api_token>'
headers = {'Content-Type': 'application/json', 'accept': 'application/json'}
payload = {'PAYLOAD FROM "Body запроса" SECTION'}
response = requests.post(url, headers=headers, data=json.dumps(payload))

project_config = response.json()

```

+++ PHP

```php
<?php

$url = 'https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/?token=<your_api_token>';
$data = ['PAYLOAD FROM "Body запроса" SECTION'];

$options = [
    'http' => [
        'header' => "Content-type: application/json\r\n" .
                    "Accept: application/json\r\n",
        'method' => 'POST',
        'content' => json_encode($data)
    ]
];

$context = stream_context_create($options);
$result = file_get_contents($url, false, $context);

$response = json_decode($result);
?>

```

+++ Ruby

```ruby
require 'net/http'
require 'json'

url = URI('https://test.sf-cloud.ru/api/projects/<project_id: uuid | str>/scope-deletion/?token=<your_api_token>')
headers = {
  'Content-Type' => 'application/json',
  'accept' => 'application/json'
}
data = {'PAYLOAD FROM "Body запроса" SECTION'}
response = Net::HTTP.start(url.host, url.port, :use_ssl => true) do |http|
  request = Net::HTTP::POST.new(url)
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
    "scope_settings": {
        "root_domains": ["оставшиеся домены", ...],
        "root_ips": ["оставшиеся айпи", ...],
        "blacklist": {
            "domains": [],
            "ipv4": [],
            "ipv6": [],
            "url": ["http://somedomain.com/?\\?path=([^&=?]+)"],
            "url_exceptions": [],
            "domains_exceptions": [],
            "ipv4_exceptions": []
        },
        "manual_ip_approve": false,
        "exclude_private_ips": true,
        "hide_unreachable": false
    }
}
```

[!badge size="m" text="Status code: 404" variant="danger"]

```json
{"error": "project <project_id> not found"}
```

===
