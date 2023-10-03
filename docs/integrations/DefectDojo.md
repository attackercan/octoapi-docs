# Интеграция с Defect Dojo

Scanfactory позволяет экспортировать отчеты по уязвимостям из нашей системы в ваш Defect Dojo  

## Установка и запуск

Для работы необходим `Python 3.11` и менеджер пакетов `pip`  

1. Выполните команды:  

    ```bash
    git clone https://github.com/scanfactory/scanfactory-to-defectdojo
    cd scanfactory-to-defectdojo
    python3 -m pip install -r requirements.txt
    ```

    При неудаче проверьте наличие и версии `python3` и `pip/pip3` и попробуйте снова  
2. Внимательно прочитайте [документацию](https://github.com/scanfactory/scanfactory-to-defectdojo/blob/main/README.md) и следуйте пунктам, указанным в ней

## Возможные проблемы

1. Проблемы при первом запуске - сверьтесь с документацией, проверьте логи. При первом запуске рекомендуется видеть логи в консоли, включаются параметром `--log-to-console`  
2. Проблемы с дедупликацией отчетов в Defect Dojo  
    Настройки дедупликации DD находятся в Боковая панель -> Кнопка настроек -> System settings
    Если дедупликация все еще не работает: [это может помочь](https://github.com/DefectDojo/django-DefectDojo/issues/2772)
