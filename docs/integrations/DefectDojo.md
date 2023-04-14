# Интеграция с Defect Dojo

Scanfactory позволяет экспортировать отчеты по уязвимостям из нашей системы в ваш Defect Dojo  

## Установка и запуск

При работе с программой может потребоваться дополнительная информация о работе скрипта

### [Документация по `nessus_importer.py`](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md)  

Для работы необходим `Python 3.10+`, менеджер пакетов `pip`  

1. Выполните эту команду, она клонирует [репозиторий](https://github.com/scanfactory/scanfactory-to-defectdojo) и установит зависимости:  

    ```bash
    git clone https://github.com/scanfactory/scanfactory-to-defectdojo && \
        cd "$(basename $_)" && \
        python3 -m pip install -r requirements.txt
    ```

    При неудаче проверьте наличие `python3` и `pip/pip3` и попробуйте снова  
    Можно выполнить все команды по отдельности:  
    1. `git clone https://github.com/scanfactory/scanfactory-to-defectdojo`  
    2. `cd scanfactory-to-defectdojo`  
    3. python3 -m pip install -r requirements.txt
2. Инициализируйте ваш [`nessus_importer.env`](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md) необходимыми переменными  
Можете поместить его в другую папку и переименовать  
По умолчанию для запуска будет использован путь /root/.env  
3. Добавить в `crontab` строку запуска скрипта ежедневно  
`0 0 * * * python3 /root/nessus_importer.py --log-path=/path/to/.log --env-path=/root/.env`  
4. Проверьте работу скрипта, запустив его в ручном режиме, выберите уровень логирования и стриминг в консоль  

[Дополнительная информация и мелочи](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md)
