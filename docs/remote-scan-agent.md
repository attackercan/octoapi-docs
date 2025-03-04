---
order: 94
---
# Remote Scan Agent

Пользователям ScanFactory Cloud доступна функциональность **Удалённого сканирования**.

Используя свой Личный Кабинет в ScanFactory Cloud, Вы сможете сканировать не только ресурсы, доступные на периметре (в сети Internet), но также и ресурсы, расположенные в изолированой корпоративной среде.

## Требования для установки

Remote Scan Agent устанавливается на виртуальной машине под управлением OS Debian 12.9 со следующими техническими характеристиками:
  
```
- 2 vCPU
- 2 GB Ram
- 20 GB Disk 
```

Разместите виртуальную машину в закрытом сегменте сети, который планируется к сканированию, без использования межсетевого экрана между узлами. Важно проверить сетевой доступ с этой ВМ до сканируемых ресурсов. 

## Установка Remote Scan Agent

  
1. Для установки Remote Scan Agent необходимо обратиться в техническую поддержку:

Сообщить:
- сканируемую подсеть, например, *192.168.0.0/16*

В ответ от поддержки Вы получаете: 
- IP-адрес и порт в облаке SF Cloud для подключения к VPN-серверу;
- файл client.conf;
- файл run.sh;
- имя вашего Remote Scan Agent (подробнее - в пункте 6).  
  
2. Следующим шагом необходимо разрешить сетевой доступ виртуальной машины до полученного в поддержке IP-адреса и порта SF Cloud и проверить его доступность.   

3. Для работы Remote Scan Agent, на вашей локальной ВМ, следует установить OpenVPN с помощью команды:
  
```
sudo apt install openvpn -y
```

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfL_WIc6hF4-qHb07BVcY16c6GClsR5kJ23Euldsstq-kbSasaW5tI10ee0Fz3HvgLy8aXEVuXzQ8VbwPoA0AyDAzqfNDTgl6OJ4UXlyuXf40vJU3d2ey7vWK6Xn88meH4tVHv8Yg?key=oRYn6Vwi1nD2B82tlMycVG7I). 
*Рисунок 1. Установка OpenVPN*. 

4. Разместить полученный у поддержки файл client.conf в директории /etc/openvpn/ на виртуальной машине Remote Scan Agent. 

5. Запускаем полученный у поддержки скрипт run.sh и убеждаемся, что он выполнился успешно. Команда:  

```
sudo bash ./run.sh
```
  
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcJVw8h6nt35Svd5NDKeQWu1TOstyC0rswriFWefSHzWq2-E9mdTNy9aDaH1OymBwikj6r7hBmgW6Z-eenPLRSkfCF_RYc-kAchH7LHSrdfegwFziFCwAMMCCzr9w_PTFSZHwpgHA?key=oRYn6Vwi1nD2B82tlMycVG7I)  
*Рисунок 2. Настройка VM с OpenVPN завершена успешно*. 

6. В Личном Кабинете, создаем новый Проект, вводим скоуп (IP или домен), и имя сканирующей ноды:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcmSR25ripXSteil0X6hV4ERNP78P9JzIrljcheZDAPGHODctiO5nJLSWCRV1m5EMVl8lSh_SK09lmvyLkjZ53W7w1TEZiVJD8vjdfWmg2-B1qBYaF_gEPBiFu5oPUyVuSmHQ95Bg?key=oRYn6Vwi1nD2B82tlMycVG7I)  
*Рисунок 3. Создание сканирования закрытого сегмента корпоративной сети*  

После создания проекта, наименование сканирующей ноды будет указано рядом с Проектом, далее работа с SF Cloud продолжается полностью аналогично сканированию внешнего периметра: настраивается проект и пул сканеров, запуск сканирования, работа с результатами сканирования.  

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdxaIv3pA547ubFLvoLZ_cIpX_liQ1rknd_60ixOW0oZ6d6TPM484FLuy0RbIGkGSdBbZLfVhBlNzgvjImBV5F4nxDuPDZqemU42l8cEg0SUp8nXJMbalwNiheRr9A89ji2h4NMvQ?key=oRYn6Vwi1nD2B82tlMycVG7I)  
*Рисунок 4. Обозначение проекта, работающего через Remote Scan Agent в общем пуле проектов*.  
