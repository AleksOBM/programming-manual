![linux.png](../files/linux.png)

# Linux

#### Показать структуру каталога

Команда

```bash
tree
```

Пример

```bash
$ tree ./Проект
./Проект
├── content
│   └── 05aed662b64e6.nwd
├── meta
│   ├── builds.json
│   ├── index.json
│   ├── options.json
│   └── sessions.jsonl
├── nwProject.nwx
└── ToC.txt
```

#### Посмотреть процессы на порту

Команда

```bash
sudo lsof -i :8080
```

Пример

```bash
$ sudo lsof -i :8080
COMMAND   PID  USER  FD   TYPE DEVICE SIZE/OFF NODE NAME
java    59559 aleks 145u  IPv4 695194      0t0  TCP *:http-alt (LISTEN)
```

#### Узнать имя приложения по его PID

Команда

```bash
ps -p 59559 -o comm=
```

Пример

```bash
$ ps -p 59559 -o comm=
java
```

### Дать права на выполнение

Команда

```bash
$ sudo chmod +x <file_path>
```

### Настройки

```bash
----------Управление пакетами: APT (высокоуровневый)------------
# Обновить списки пакетов из репозиториев
apt update

# Обновить все установленные пакеты
apt upgrade

# Обновить с разрешением конфликтов (может удалять пакеты)
apt full-upgrade

# Установить пакет из репозитория
apt install package

# Удалить пакет (с сохранением конфигурации)
apt remove package

# Удалить пакет (с конфигурацией)
apt purge package

# Удалить пакеты, не требуемые другими пакетами
apt autoremove

# Найти пакет по имени или описанию
apt search string

# Показать информацию о пакете
apt show package

# Показать доступные версии пакета
apt list -a package

# Показать зависимости пакета
apt depends package

# Показать пакеты, зависящие от указанного
apt rdepends package

# Показать установленные пакеты
apt list --installed

----------Управление пакетами: DPKG (низкоуровневый)------------
# Установить локальный .deb пакет
dpkg -i package.deb

# Удалить пакет (с сохранением конфигурации)
dpkg -r package

# Удалить пакет (с конфигурацией)
dpkg -P package

# Показать информацию о пакете
dpkg -s package

# Показать файлы, установленные пакетом
dpkg -L package

# Найти пакет, которому принадлежит файл
dpkg -S /path/to/file

# Показать содержимое .deb файла
dpkg -c package.deb

# Настроить распакованный пакет
dpkg --configure package

# Сравнить версии пакетов
dpkg --compare-versions v1 gt v2

----------Управление пакетами: очистка------------
# Удалить загруженные .deb файлы
apt clean

# Удалить устаревшие .deb файлы
apt autoclean

----------Управление сервисами: systemctl------------
# Запустить сервис
systemctl start service

# Остановить сервис
systemctl stop service

# Перезапустить сервис
systemctl restart service

# Перезагрузить конфигурацию без остановки
systemctl reload service

# Показать статус сервиса
systemctl status service

# Включить автозапуск сервиса при загрузке
systemctl enable service

# Отключить автозапуск сервиса
systemctl disable service

# Проверить, активен ли сервис
systemctl is-active service

# Проверить, включён ли автозапуск
systemctl is-enabled service

# Список запущенных сервисов
systemctl list-units --type=service

# Список всех юнитов
systemctl list-unit-files

# Показать зависимости юнита
systemctl list-dependencies service

# Сбросить статус failed юнита
systemctl reset-failed service

----------Управление сервисами: питание------------
# Выключить систему
systemctl poweroff

# Перезагрузить систему
systemctl reboot

# Приостановить систему
systemctl suspend

# Гиbernация системы
systemctl hibernate

# Переключиться в multi-user (CLI)
systemctl isolate multi-user

# Переключиться в graphical (GUI)
systemctl isolate graphical

----------Управление сервисами: загрузка------------
# Показать время загрузки
systemd-analyze time

# Показать, что замедляло загрузку
systemd-analyze blame

# Проверить юнит на ошибки
systemd-analyze verify service

# Перезагрузить конфигурацию systemd
systemctl daemon-reload

# Показать цель по умолчанию
systemctl get-default

# Установить цель по умолчанию (multi-user)
systemctl set-default multi-user

# Установить цель по умолчанию (graphical)
systemctl set-default graphical

----------Сеть: диагностика (ip)------------
# Показать все сетевые интерфейсы
ip address show

# Показать адреса конкретного интерфейса
ip address show dev eth0

# Добавить IP-адрес интерфейсу
ip address add 192.168.1.10/24 dev eth0

# Удалить IP-адрес с интерфейса
ip address delete 192.168.1.10/24 dev eth0

# Показать таблицу маршрутизации
ip route show

# Добавить маршрут по умолчанию
ip route add default via 192.168.1.1

# Показать IPv6 маршруты
ip -6 route show

# Показать ARP-таблицу
ip neigh show

# Показать статистику интерфейсов
ip -s link show

----------Сеть: диагностика (утилиты)------------
# Проверить доступность хоста
ping host

# Трассировка маршрута
traceroute host

# Проверить открытые порты
ss -tulpn

# Проверить DNS-разрешение
nslookup domain

# Показать сетевые соединения
netstat -tulpn

----------Сеть: конфигурация (ifupdown)------------
# Поднять интерфейс
ifup eth0

# Опустить интерфейс
ifdown eth0

# Перезапустить сеть
systemctl restart networking

# Показать статус сети
systemctl status networking

# Файл конфигурации интерфейсов
/etc/network/interfaces

# Настроить DHCP для интерфейса
# allow-hotplug eth0
# iface eth0 inet dhcp

# Настроить статический IP
# iface eth0 inet static
#     address 192.168.1.10
#     netmask 255.255.255.0
#     gateway 192.168.1.1

----------Сеть: Wi-Fi------------
# Установить wpa_supplicant
apt install wpa_supplicant

# Создать конфигурацию Wi-Fi
wpa_passphrase "SSID" > /etc/wpa_supplicant/wpa_supplicant.conf

# Защитить файл конфигурации
chmod 0600 /etc/wpa_supplicant/wpa_supplicant.conf

----------Файловые системы: монтирование------------
# Смонтировать файловую систему
mount /dev/sdb1 /mnt

# Размонтировать
umount /mnt

# Показать смонтированные файловые системы
mount

# Показать использование диска
df -h

# Показать размер директории
du -sh /path

# Файл конфигурации монтирования
/etc/fstab

# Перемонтировать fstab
mount -a

# Перезагрузить systemd после изменения fstab
systemctl daemon-reload

----------Файловые системы: swap------------
# Включить swap
swapon /dev/sdb2

# Отключить swap
swapoff /dev/sdb2

# Показать активные swap
swapon --show

----------Ядро: sysctl------------
# Показать все параметры ядра
sysctl -a

# Прочитать параметр
sysctl kernel.hostname

# Записать параметр
sysctl -w kernel.domainname="example.com"

# Загрузить настройки из файла
sysctl -p /etc/sysctl.conf

# Загрузить все системные настройки
sysctl --system

# Файлы конфигурации
/etc/sysctl.d/*.conf
/run/sysctl.d/*.conf
/usr/local/lib/sysctl.d/*.conf
/usr/lib/sysctl.d/*.conf
/etc/sysctl.conf

----------Ядро: параметры------------
# Показать версию ядра
uname -r

# Показать всю информацию о системе
uname -a

# Показать загруженные модули
lsmod

# Загрузить модуль
modprobe module_name

# Выгрузить модуль
modprobe -r module_name

----------Системная информация------------
# Показать uptime
uptime

# Показать информацию о памяти
free -h

# Показать информацию о CPU
lscpu

# Показать информацию о блочных устройствах
lsblk

# Показать информацию о PCI
lspci

# Показать информацию о USB
lsusb

# Показать информацию о железе (DMI)
dmidecode

----------Пользователи и группы------------
# Добавить пользователя
adduser username

# Удалить пользователя
deluser username

# Добавить в группу
adduser username group

# Удалить из группы
deluser username group

# Показать группы пользователя
groups username

# Изменить пароль
passwd username

# Переключиться на пользователя
su - username

# Выполнить команду от root
sudo command

----------Права доступа------------
# Изменить права
chmod 755 file

# Изменить владельца
chown user:group file

# Изменить владельца рекурсивно
chown -R user:group /path

# Показать права
ls -la

# Установить SUID
chmod u+s file

# Установить SGID
chmod g+s directory

# Sticky bit
chmod +t directory

----------Процессы------------
# Показать все процессы
ps aux

# Показать дерево процессов
ps auxf

# Показать процессы в реальном времени
top

# Улучшенный top
htop

# Найти процесс по имени
pgrep process_name

# Убить процесс
kill PID

# Убить по имени
pkill process_name

# Убить принудительно
kill -9 PID

# Показать открытые файлы процесса
lsof -p PID

# Показать, кто использует файл
lsof /path/to/file

----------Логи------------
# Логи systemd
journalctl

# Логи конкретного сервиса
journalctl -u service

# Логи с отслеживанием
journalctl -f

# Логи за сегодня
journalctl --since today

# Логи за последний час
journalctl --since "1 hour ago"

# Логи с уровнем ошибок
journalctl -p err

# Логи загрузки
journalctl -b
```