# 2 - Технический обзор ScanFactory

----

## Вступление

На этой странице подробно описаны используемые инструменты и возможные параметры запуска.

## Этапы сканирования

Платформа проводит сканирование внешнего периметра в 3 логических этапа:

 № | Описание этапа                | Результат работы  
--- | --- | ---  
 1 | Сбор активов (OSINT)          | Перечень доменов и IP, принадлежащих Заказчику  
 2 | Сканирование сетевых сервисов | Определение открытых портов и сервисов; поиск известных CVE; брутфорс аутентификации на сетевых сервисах (FTP, SSH, Telnet, и др)  
 3 | Сканирование веб-приложений   | Краулинг веб-сайтов; поиск скрытых файлов и папок; поиск уязвимостей  

Для примера рассмотрим сканирование Компании со следующими входными данными:  
Домены, принадлежащие компании: `*.company.ru`  
IP, принадлежащие компании: `11.22.33.44/23`  

==- <h3>Этап 1. Сбор активов (OSINT)</h3>

Доступен следующий набор инструментов:  

[!badge amass] и [!badge subfinder] - Пассивный поиск поддоменов по открытым источникам  
[!badge altdns] - Рекурсивный брутфорс поддоменов по словарю (словарь по умолчанию - 1000 слов)  
[!badge sdto] - Проверка перехватов поддомена. Opensource инструмент собственной разработки  

Пример работы:

- [!badge amass] нашёл домен 2 домена: `www.company.ru` (3 уровня) и домен `sub.dev.company.ru` (4 уровня). Оба домена добавлены в БД, и отображаются на вкладке Активы  
- [!badge altdns] запустит брутфорс поддоменов на 3 уровень `*.company.ru` (т.к. в настройках был указан wildcard 3 уровня), а также `*.dev.company.ru` (т.к. домен 4 уровня был найден с помощью [!badge amass]). Для этого создаётся 2 списка потенциальных доменов, и осуществляется резолв доменов с помощью инструмента [!badge massdns], **публичных резолверов Yandex и Google**, со скоростью 1000 запросов в секунду.  
- На каждый найденный поддомен запускается проверка перехвата поддоменов с помощью инструмента [!badge sdto]

===

==- <h3>Этап 2. Сканирование сетевых сервисов</h3>

Выбранные шаблоны инструментов будут запущены на все IP из диапазона `11.22.33.44/23`, а также на все IP, в которые резолвятся обнаруженные домены из Этапа 1.  

Доступен следующий набор инструментов:  
[!badge nmap] - поиск открытых портов, а также определение версий сервисов  
[!badge infrascan] - поиск известных CVE. База данных CVE является самой большой в мире, содержит более 65000 проверок, и обновляется каждый день  
[!badge patator] - брутфорс паролей к сетевым сервисам по дефолтным словарям  

При старте проекта создаётся 2 шаблона [!badge nmap]: на топ-1000 портов; и на все 65535 портов. Также вы можете создать свой шаблон с нужными опциями [!badge nmap]  
На каждый открытый порт определяется версия запущенного сервиса, и запускается поиск известных уязвимостей к этой версии сервиса с помощью сервиса [!badge Vulners]  

!!!
Так как и все наши заказчики, и мы находимся в России - а значит, ответы о том, закрыт порт или нет, мы будем получать быстро, то мы используем разумные настройки, которые выполняют скан даже быстрее, чем `nmap timing option -Т4`, и при этом выдают достоверный результат
!!!

Мы используем следующие настройки для nmap:

`--max-rtt-timeout 600ms` - максимальное время получения ответа от порта  
`--min-rtt-timeout 10ms` - минимальное время получения ответа от порта  
`--initial-rtt-timeout 300ms` - первоначальное время получения ответа (нмап сам меняет это значение, в рамках min/max опций выше)  
`--host-timeout 110m` - максимальное время сканирования IP = 1 час 50 минут  
`--max-retries 3` - количество повторных попыток на сканирование порта  
`--max-scan-delay 100ms` - максимальная задержка между отправкой повторных пакетов  

С этими настройками 1000 портов на 1 сервере сканируются за 25 секунд. Таким образом, подсеть размера /23 (512 IP) на топ 1000 портов мы можем просканировать за 4,5 часа (имея 1 запущенную копию nmap)

**Пример работы**  
[!badge nmap] обнаруживает открытый порт `22`, и опредляет версию запущенного сервиса как `OpenSSH 7.9`. Сервер помечается как "активный", и информацию по этому сервису теперь можно увидеть на вкладке Активы  
Сервис [!badge Vulners] сообщает, что версия OpenSSH устарела и имеет перечень уязвимостей, поэтому создаёт новую запись об этой Уязвимости  
[!badge infrascan] запускает анализ каждого IP  
[!badge patator] запускает брутфорс авторизаций на каждый открытый порт

!!!success Полезный совет  
При сканировании больших подсетей (более, чем /23) рекомендуем включать дополнительную настройку «Предварительный ping IP». В этом случае на каждый сервер отправляется команда `nmap -sn IP`, и на основе результата сервер будет отмечен как «Активный» или «Не активный». Далее, шаблоны инструментов будут запущены только на IP, определённые как «Активные».
!!!
===

==- <h3>Этап 3. Сканирование веб-приложений</h3>

Сканирование веб-приложений проходит в 3 этапа: сбор информации о веб-приложении, отсев «неинтересных» запросов, и поиск уязвимостей  

+++ Этап 3.1.

#### Сбор информации о веб-приложении

Выбранные шаблоны инструментов будут запущены **на все открытые порты**:

[!badge crawler] - обход веб-приложения с помощью headless Chrome краулера собственной разработки, а также сохранение скриншотов главной страницы  
[!badge crawlergo] - обход страниц веб-приложений  
[!badge dirsearch] - поиск скрытых файлов и папок на веб-сервере  
Результат работы – набор GET/POST запросов, которые передаются на следующий этап

&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  

+++ Этап 3.2.

#### Отсев «неинтересных» запросов

Все обнаруженные запросы проходят через двухуровневую систему отсева «неинтересных» запросов:  
**Уровень 1. Отсев ссылок с похожим форматом**  
Пример. Во время краулинга интернет-магазина были найдены следующие ссылки:  

```text
http://client.ru/products/book-1/
http://client.ru/products/book-2/
http://client.ru/products/book-3/
...
http://client.ru/products/book-31337/
```

Все имеющиеся ссылки имеют одинаковый паттерн:  `http://client.ru/products/book-[\d]/`  

Как только в базу данных будет добавлено 50 (значение по умолчанию) ссылок с одинаковым паттерном, далее ссылки с таким же паттерном сохраняться не будут. В нашем примере будут сохранены только запросы к книгам с 1 по 50.  

**Уровень 2. Отсев страниц с похожим содержимым**  
ScanFactory использует собственную технологию, основанную на [SimHash](https://en.wikipedia.org/wiki/SimHash), которая анализирует контент каждой веб-страницы перед сохранением в базу данных.  
В случае, если большое количество страниц имеет одинаковую HTML-структуру (например, страницы блога, или карточки товаров), такие страницы не сохраняются  

+++ Этап 3.3.

#### Поиск уязвимостей

На финальном этапе запускаются следующие шаблоны инструментов:

[!badge webscan] - сканер OWASP Top 10 уязвимостей. Инструмент работает в режиме proxy  
[!badge nuclei] - opensource сканер веб-приложений с набором публичных и приватных шаблонов.  
[!badge ffuf] - поиск скрытых GET/POST параметров путём поиска аномалий в ответах сервера. На каждый имеющийся HTTP-запрос в базе данных инструмент отправляет более 1000 запросов (вида `http://client.ru/api/?DEBUG=1` , `http://client.ru/api/ADMIN?=1` , и другие). Мы рекомендуем включать этот инструмент, только если у вас в скоупе 1-5 веб-приложений, и НЕ включать его, если веб-приложений больше  

**Сканирование веб-приложений методом «серого ящика»**  
Авторизация в веб-приложениях работает путём добавления авторизационных HTTP-заголовок к краулерам  
Для этого создайте новый шаблоны [!badge crawler] и [!badge crawlergo] с параметрами:  
`"extra_headers": {"Cookie": "a=b"}` (подставить сюда хэдеры, по которому приложение авторизует пользователя)  

Так как указанные хэдеры будут добавляться ко всем запросам краулера, то нужно создавать отдельный проект под каждое веб-приложение, для которого настраивается авторизация  

&nbsp;&nbsp;  
&nbsp;&nbsp;  
&nbsp;&nbsp;  

+++

===