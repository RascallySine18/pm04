# 1. Установка операционной системы, настройка и конфгурирование:
восстановление копирования
```
sudo sh ./VBoxLinuxAdditions.run
```
Файл -> Менеджер виртуальных носителей -> Свойства
## Настроить параметры операционной системы, драйверов и служб:
## Настройка драйвера
- Устройства -> Подключить образ диска Дополнений гостевой ОС -> autorun.sh ПКМ -> Запустить как приложение -> ENTER -> Перезапуск
- Устройства -> Общий буфер обмена -> `Двунаправленный`
- Устройства -> Функция Drag and Drop -> `Двунаправленный`
- Можно командой:
```
sudo ubuntu-drivers autoinstall
```
- Если ошибка:
```
sudo apt install bzip2
```
- V2 Драйвера:
```
sudo apt update
```
```
sudo apt install -y build-essential dkms linux-headers-$(uname -r)
```
```
cd /media/$USER/VBox_GAs_*
```
- (Замените * на версию, например, VBox_GAs_7.0.12).
- Запустите установку:
```
sudo ./VBoxLinuxAdditions.run
```
- Если возникнет ошибка, попробуйте с ключом --force:
```
sudo ./VBoxLinuxAdditions.run --force
```
- Перезагрузите Ubuntu
## Настройка ядра и просмотр:
```
sudo nano /etc/sysctl.conf
```
- минимизирует использование свопа, что ускоряет работу на SSD.
```
vm.swappiness=10
```
- сохраняет больше файлового кэша в памяти, что ускоряет работу с файлами
```
vm.vfs_cache_pressure=50
```
- нажать ctrl+O и ctrl+X
```
sudo sysctl --system
```
```
sudo sysctl -a
```
## Настройка и установка SSH
## UPD: Удалённый доступ к компьютеру через сеть
```
sudo apt install openssh-server -y
```
```
sudo systemctl enable ssh
```
```
sudo systemctl start ssh
```
```
sudo ufw allow ssh
```
```
sudo systemctl status
```
## Настройка брандмауэра
```
sudo ufw enable
```
```
sudo ufw allow ssh
```
```
sudo ufw status
```
- В настройках виртуальной машины: Сеть - проброс портов - добавить
- Порт хоста: 2222 Порт гостя: 22
- В виндовс в cmd прописать
```
ssh логин_в_линукс@127.0.0.1 -p 2222
```
- в linux
```
ssh логин_в_линукс@127.0.0.1
```
## Проверка сетевого интерфейса:
```
sudo apt install net-tools
```
```
ifconfig -a
```
- Проверка соединения:
```
nmcli device
```
## Настроить интернет соединение
```
ping ya.ru
```
- Данные о соединении с интернетом:
```
ip a
```
- Открыть конфигурационный файл сети
```
sudo nano /etc/netplan/01-netcfg.yaml
```
- Прописать настройки NAT (пример для DHCP):
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
```
Если нужен статический IP (редко для NAT):
```
network:
  version: 2
  ethernets:
    enp0s3:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```
- Применить настройки:
```
sudo netplan apply
```
- Проверить подключение:
```
ping 8.8.8.8
```
## Выполнить установку базового программного обеспечения
```
sudo snap install paintsupreme-3d
```
```
sudo apt install -y clamav clamtk
```
```
sudo apt install cpu-x
```
- Установка БД
```
sudo apt update
```
- Установим БД и сервер:
```
sudo apt install postgresql postgresql-contrib
```
- Проверим статус:
```
service postgresql status
```
```
sudo apt install vim
```
```
sudo snap install code --classic
```
## Выполнить установку виртуального принтера
```
sudo apt install cups printer-driver-cups-pdf
```
- Если вдруг у вас не появится принтер:
```
sudo systemctl enable cups
```
```
sudo systemctl start cups
```
http://localhost:631
# 2. Применение средств защиты компьютерных систем:
## Выполнить резервное копирование установленной операционной системы и создать установочный образ системы
```
sudo apt install timeshift -y
```
## Создание точки восстановления системы
- Создание точек восстановления системы подразумевается в ОС Windows. В Linux это не предусмотрено, только резервное копирование
## Создать группы пользователей, настроить права доступа
- Если через интерфейс:
```
sudo apt install gnome-system-tools
```
- Если только через команды:
- Чтобы создать группу, пишем:
```
sudo groupadd <имя группы>
```
- Чтобы создать пользователя, пишем:
```
sudo useradd <имя пользователя>
```
- Чтобы добавить уже существующего пользователя в существующую группу, пишем:
```
sudo usermod -aG <название группы> <имя пользователя>
```
- Чтобы дать права администратора пользователю, пишем:
```
sudo usermod -aG sudo <имя пользователя>
```
- Чтобы сделать пользователя обычным, пишем:
```
sudo deluser <имя пользователя> sudo
```
- Чтобы добавить право записи для группы в папку, пишем:
```
sudo chmod g+w </путь/к/папке>
```
- Чтобы удалить право чтения для папки, пишем:
```
sudo chmod o-r (для удаления доступа к чтению) </путь/к/папке>
```
- Чтобы установить для владельца права чтения, записи и выполнения (полный доступ), пишем:
```
sudo chmod u=rwx </путь/к/папке>
```
## Настроить аутентификацию и авторизацию, журналов мониторинга
- Чтобы промониторить все логи, пишем:
```
sudo journalctl -xe
```
- Можно показать дерево процессов, пишем:
```
pstree
```
# *Общая папка между виндовсом и линуксом
- Выключаем виртуальную машину
- Заходим в настройки виртуальной машины
- Во вкладке `общие`, выбираем `дополнительно`. Общий буфер обмена и функция Drag'n'Drop должны быть в состоянии двунаправленный
- Далее заходим во вкладку сеть. У нас уже включен `адаптер 1` с типом подключения `NAT`. Нам нужно включить `адаптер 2` и выбираем тип подключения `сетевой мост`
- Нажимаем ОК, чтобы сохранились изменения
- B раздел `общие папки`
- выбираем папку на рабочем столе, имя папки установится автоматом. Ставим галочку на `авто-подключение`
- Нажимаем ОК, чтобы сохранить изменения с общей папкой. Запускаем машину и заходим в терминал
- В терминале пишем:
```
sudo apt install gcc make perl
```
```
sudo usermod -aG vboxsf <имя пользователя>
```
- Перезагружаем и готово
# Совместимость ПО с установленной ОС
- Показать что можно экспортировать, например в PDF и т.п.
- Показать что цвета корректно отображаются, если это граф. редактор, то показать что красный это красный и т.п.
- Выбираем настройку дисплея -> Разрешение -> Ставим дробное масштабирование -> Ставим низкое расширение -> Нажимаем Применить
- Открываем приложение, проверяем корректность работы (Пример: Меню корректно отображается)
- Отключаем дробное масштабирование
- Удаляем лишние программы с рабочего стола
- Отключение композиции рабочего стола
```
sudo apt install gnome-tweaks
```
```
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```
## *Установка сервера и мониторинга (если задание с сервером):
```
sudo apt update
```
```
sudo apt install -y cockpit
```
```
sudo systemctl enable --now cockpit.socket
```
```
ip a
```
- Откройте Cockpit в браузере
- В адресной строке введите:
```
https://<IP_сервера>:9090
```
- 4. Если подключение не работает
- Проверьте, запущен ли Cockpit:
```
sudo systemctl status cockpit.socket
```
```
sudo systemctl restart cockpit.socket
```
- Откройте порт 9090 в брандмауэре:
```
sudo ufw allow 9090/tcp
```
- Проверьте, что сервер доступен с вашего компьютера:
```
ping 192.168.1.100  # Подставьте ваш IP
```
- Если пинг не проходит — проверьте сетевые настройки VirtualBox (режим "Сетевой мост" или "NAT").
