# Домашнее задание к занятию "`Система мониторинга Zabbix»`" - `Стрельников Александр`

### Задание 1

Установите Zabbix Server с веб-интерфейсом

#### Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

#### Требования к результатам

1. Прикрепите в файл README.md скриншот авторизации в админке.
2. Приложите в файл README.md текст использованных команд в GitHub.

### Решение:

1. Ниже представлен скриншот авторизации в админке.

![Авторизация](https://github.com/Stvrrow/hw-02/blob/main/img/img1.png)

2. Текст использованных команд для настройки zabbix server:

Установка репозитория.

```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian11_all.deb
dpkg -i zabbix-release_latest_6.0+debian11_all.deb
apt update
```

Установка zabbix server и apache.

```
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts
```

Создание пользователя zabbix и базы данных.

```
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```

Импорт исходной схемы данных.

```
cat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```

Необходимо указать пароль zabbix в файле /etc/zabbix/zabbix_server.conf
В строке:

```
DBPassword=password
```

Перезагрузка и включение zabbix server и apache при запуске системы

```
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

### Задание 2

Установите Zabbix Agent на два хоста.

#### Процесс выполнения

1. Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
5. Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

#### Требования к результатам

1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4. Приложите в файл README.md текст использованных команд в GitHub

### Решение:

1. Cкриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу

![Hosts](https://github.com/Stvrrow/hw-02/blob/main/img/img2.png)

2. Cкриншот лога zabbix agent, где видно, что он работает с сервером

![Лог zabbix agent](https://github.com/Stvrrow/hw-02/blob/main/img/img3.png)

3. Скриншоты раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.

![Latest data dev11-slave](https://github.com/Stvrrow/hw-02/blob/main/img/img4.png)

![Latest data dev11-server](https://github.com/Stvrrow/hw-02/blob/main/img/img5.png)

4. Текст использованных команд для настройки zabbix agent:

Установка репозитория.

```
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian11_all.deb
dpkg -i zabbix-release_latest_6.0+debian11_all.deb
apt update
```

Установка zabbix agent

```
apt install zabbix-agent
```

Перезагрузка и включение zabbix agent при запуске системы
```
systemctl restart zabbix-agent
systemctl enable zabbix-agent
```

Необходимо указать адрес zabbix сервера в файле /etc/zabbix/zabbix_agentd.conf
Строка:

```
Server=<server ip>
```