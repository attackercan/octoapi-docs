---
order: 97
---

# Список используемых сканеров

----

В этом разделе перечислены сканеры, используемые в ScanFactory

### Коммерческие сканеры

- **Коммерческий Веб-сканер #1**
  - Поиск уязвимостей на веб-сайтах методом чёрного ящика
- **Коммерческий Сканер Инфраструктуры**
  - Поиск CVE-уязвимостей, PCI DSS сканирование

### OSINT

- Пассивный поиск:
- **[amass](https://github.com/OWASP/Amass)**
  - Пассивный поиск поддоменов по платным и бесплатным источникам
- **[subfinder](https://github.com/projectdiscovery/subfinder)**
  - Пассивный поиск поддоменов по платным и бесплатным источникам

- Активный поиск:
- **[goaltnds](https://github.com/subfinder/goaltdns)**
  - Активный поиск поддоменов, методом брутфорса по словарю
- **[alterx](https://github.com/projectdiscovery/alterx)**
  - Активный поиск поддоменов, методом пермутаций результатов
- **[dnsx](https://github.com/projectdiscovery/dnsx)**
  - Активный поиск поддоменов, очистка результатов методом резолвинга
- **[massdns](https://github.com/blechschmidt/massdns)**
  - Активный поиск поддоменов, очистка результатов методом резолвинга результатов


### Opensource инструменты

- **[nuclei](https://github.com/projectdiscovery/nuclei)**
  - Поиск популярных уязвимостей. Более 9000 плагинов. Регулярно разрабатываем собственные плагины
- **[nmap](https://nmap.org/)**
  - Поиск открытых портов и определение запущенных сервисов
- **[masscan](https://github.com/robertdavidgraham/masscan)**
  - Поиск открытых портов
- **[OWASP ZAP](https://github.com/zaproxy/zaproxy)**
  - Поиск OWASP Top 10 уязвимостей на веб-сайтах
- **brute**
  - Перебор паролей к веб-формам, а также сетевым сервисам ftp, ftps, mssql, postgres, radmin2, rdp, redis, amqp, smb, snmp, ssh, vnc, telnet
- **[sdto](https://github.com/scanfactory/sdto)**
  - Проверка перехвата поддомена
- **[dirsearch](https://github.com/maurosoria/dirsearch)**
  - Поиск скрытых файлов и папок
- **[crawlergo](https://github.com/Qianlitp/crawlergo)**
  - Рекурсивный обход веб-сайтов, сбор HTTP-запросов
- **[fuzzuli](https://github.com/musana/fuzzuli)**
  - Поиск бэкапов веб-сайтов, на основе динамически генерируемого словаря
- **grep**
  - Анализ веб-страниц: поиск доменов, ссылок на подгружаемые js-файлы, и тд
- **[waybackurls](https://github.com/tomnomnom/waybackurls)**
  - Поиск ссылок на этот домен в [Wayback Machine](https://archive.org/web/)
- **[wpscan](https://github.com/wpscanteam/wpscan)**
  - Поиск уязвимостей Wordpress
- **[x8](https://github.com/sh1yo/x8)**
  - Поиск скрытых параметров в HTTP-запросах
