
```table-of-contents
```

#### Описание

Плейбук для настройки сервисов Keepalived и FSFO в среде Oracle Data Guard.
Cостоит из двух ролей `keepalived` и `fsfo`, которые выполняют следующий объём работ:
##### keepalived
- отключение SELinux
- установка keepalived
- конфигурирование keepalived
- поднятие VIP адресов в зависимости от роли БД

##### fsfo
- настройка FRA
- настройка Flashback
- обновление конфигурации Broker
- настройка и запуск Observer
#### Требования

- два VIP адреса для primary и standby БД 
- настроенная конфигурация Data Guard с одним или более standby серверами управляемая через Data Guard Broker
- отдельный сервер с ролью observer с настроенным wallet для хранения подключений к управляемым БД
#### Переменные

[keepalived/vars/main.yml](keepalived/vars/main.yml)
`primary_vip` - vip адрес primary БД
`standby_vip` - vip адрес standby БД

[fsfo/vars/main.yml](fsfo/vars/main.yml)
`ora_fra` - директория для FRA
`flashback_retention_min` - длительность удержания flashback
`script_dir - директория` для временных скрпитов настройки FRA и flashback
`broker_params` - параметры брокера необходимые для работы FSFO
`observer_host` - имя хоста observer
`observer_ora_home` - oracle home на хосте observer
`wallet_pass` - пароль от wallet на хосте observer
`sys_pass` - пароль sys для подключения к БД
#### Примеры запуска

> [!NOTE]  
> Плейбук запускать под пользователем с правами `sudo`

Стандартный запуск для выполнения всего объёма работы

```shell
ansible-playbook keepalived-fsfo.yml --user den --ask-pass --ask-vault-pass --extra-vars \
"\
host_list=angel19,devil19 \
primary_vip=10.10.10.233 \
standby_vip=10.10.10.234 \
"
```

Запуск определенных заданий по `tags`, выполняется настройка только необходимой части
- keepalived 
- fra
- flashback
- broker
- observer

```shell
ansible-playbook keepalived-fsfo.yml --user den --ask-pass --ask-vault-pass --tags "keepalived" --extra-vars \
"\
host_list=angel19,devil19 \
primary_vip=10.10.10.233 \
standby_vip=10.10.10.234 \
"
ansible-playbook keepalived-fsfo.yml --user den --ask-pass --ask-vault-pass --tags "broker,observer" --extra-vars "host_list=angel19,devil19"
```
