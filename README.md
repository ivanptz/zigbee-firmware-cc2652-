* [JetHome JetHub H1 - обновляем прошивку Zigbee модуля с сохранением NVRAM, без перепривязки устройств](https://youtu.be/QaKIUPci67w)

# Текстовая версия - ссылки и команды из урока

* [Репозиторий zigpy-znp](https://github.com/zigpy/zigpy-znp/)

* NVRAM
```yaml
pip3 install git+https://github.com/zigpy/zigpy-znp/
```
* Чтение NVRAM
```yaml
python3 -m zigpy_znp.tools.nvram_read /dev/ttyAML2 -o backup.json
```

* Прошивка
* [Репозиторий cc2538-bsl](https://github.com/JelmerT/cc2538-bsl)

* Качаем утилиту для прошивки
```yaml
git clone https://github.com/JelmerT/cc2538-bsl.git
```
* Устанавливаем дополнительные пакеты
```yaml
pip3 install pyserial intelhex python-magic
```
* Установка unzip
```yaml
sudo apt-get install -y unzip
```
* Репозитории с прошивками - Рекомендация jethome
* [jethome](https://jethome.ru/wiki-cc2652p-firmware)



* Качаем и распаковываем прошивку
```yaml
wget -O CC1352P2_CC2652P_launchpad_coordinator_20210218.zip https://github.com/jethome-ru/zigbee-firmware/blob/master/ti/coordinator/cc2652/CC1352P2_CC2652P_launchpad_coordinator_20210218.zip?raw=true && unzip CC1352P2_CC2652P_launchpad_coordinator_20210218.zip
```
* Список файлов 
```yaml
ls
```
* Переходим в режим root
```yaml
sudo bash
```
* Переводим модуль в режим SBL (serial bootloader):
```yaml
echo 0 > /sys/class/gpio/gpio510/value
echo 1 > /sys/class/gpio/gpio507/value
echo 0 > /sys/class/gpio/gpio507/value
```
* Прошиваем
```yaml
python3 cc2538-bsl/cc2538-bsl.py -p /dev/ttyAML2 -e -w CC1352P2_CC2652P_launchpad_coordinator_20210218.hex
```
* Возвращаем в рабочий режим
```yaml
echo 1 > /sys/class/gpio/gpio510/value
echo 1 > /sys/class/gpio/gpio507/value
echo 0 > /sys/class/gpio/gpio507/value
```
* Выходим из режима root
```yaml
exit 
```
* Записываем NVRAM
```yaml
python3 -m zigpy_znp.tools.nvram_write /dev/ttyAML2 -i backup.json
```

