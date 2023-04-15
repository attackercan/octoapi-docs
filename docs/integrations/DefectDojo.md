# Интеграция с Defect Dojo

Scanfactory позволяет экспортировать отчеты по уязвимостям из нашей системы в ваш Defect Dojo  

## Установка и запуск

Для работы необходим `Python 3.10+` и менеджер пакетов `pip`  

1. Выполните команды:  

    ```bash
    git clone https://github.com/scanfactory/scanfactory-to-defectdojo
    cd scanfactory-to-defectdojo
    python3 -m pip install -r requirements.txt
    ```

    При неудаче проверьте наличие и версии `python3` и `pip/pip3` и попробуйте снова  
2. Добавьте ваши логины-пароли от ScanFactory и DefectDojo в файл [`nessus_importer.env`](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md)  
Переместите env-файл в /root/.env, или в любое другое место  
3. Создайте `crontab` на ежедневный запуск скрипта:  
`0 8 * * * python3 /root/nessus_importer.py --log-path=/path/to/.log --env-path=/root/.env`  
4. Проверьте работу скрипта, запустив его в ручном режиме 

[Дополнительная информация](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md)

## Возможные проблемы

1. Проблемы при первом запуске - взгляните на логи  
2. Проблемы с дедупликацией отчетов в Defect Dojo  
    Возможные варианты решения  
    [Первый](https://github.com/DefectDojo/django-DefectDojo/issues/6407) [Второй](https://github.com/DefectDojo/django-DefectDojo/issues/2772)  
