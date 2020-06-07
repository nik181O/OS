# Лабораторная работа №2 
## Часть №3:Добавление новых дисков и перенос раздела

От себя скажу что это самая сложная часть, достойная отдельной лабы. По условиям задачи у нас произошел отказ диска и начальство сказало перенести все данные на новые диски ,а старые выбросить. На это дело нам дали 4 диска:ssd4,ssd5(6,25gb) и HDD1,hdd2(12,5gb).

- Как видите на данном скрине. Я уже отмонтировал "сломанный" диск и примонтировал новый ssd ,чтобы перенести на него рэйд.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_49_01.png)

- Дальше я копирую таблицы раздела на новый диск.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_49_52.png)

- Потом копируем boot на новый диск и монтируем его на новый диск ,т.к. мы потом удалим старый диск.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_52_11.png)

- Теперь устанавливаем загрузчик grub на новый диск и создаем новое рэйд устройство на новом диске.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_53_57.png)

- Создаем физический том для нового raid устройства
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_55_01.png)

- Дальше через команду vgextend system /dev/md63 мы добавляем наш физический том в vg system.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_19_56_04.png)

- Потом перемещаем логические тома из /dev/md0 в /dev/md63
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_20_00_59.png)

- Теперь , когда нам уже не нужен старый pv мы убираем его из vg system.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_20_01_36.png)

- Проверяем бут , пустой он или нет. Видим тут grub и файлы ос под архитектуру amd64.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_20_02_15.png)

- Настало время отмонтировать старый диск и смонтировать новые диски.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_20_03_11.png)

- Копируем таблицы разметок и boot на второй ssd. Устанавливаем на него grub.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_26_05_2020_20_06_03.png)

- Теперь нам нужно увеличить размер нашего рэйда , так как скопированный таблицы разметок занимают не весь обьем ssd5. Это делается через утилиту fdisk. Команды и так известны. В итоге мы получим от утилиты ответ: "Чтобы изменения пришли в силу ,надо перезагрузить пк или использовать другие утилиты." Игнорируем это тк изменения уже пришли в силу. В итоге мы увеличили раздел под raid  для диска. Пишем команду partx -u /dev/sde и смотрим результат.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_50_16.png)

- Добавляем второй диск в raid массив. Расширяем колво дисков до двух. Но раздел первого диска под рэйд меньше раздела второго.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_51_31.png)

- Пользуемся утилитой fdisk для изменения размера раздела.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_53_26.png)

- Поскольку сам Рэйд массив не увеличился , мы увеличиваем его через команду mdadm --grow /dev/md63 --size=max.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_54_24.png)

- К сожалению размер lv в vg  не поменялся поэтому мы вручную увеличиваем pv.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_54_54.png)

- И для lv root и var величиваем место для разметки.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_55_44.png)

- Переходим к hdd. Ищем их названия.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_20_57_13.png)

- Создаем новый рэйд массив. И создаем новый физический том  + vg для нового рэйда.(Я в этом шаге сделал опечатку и дважды вместо data написал datd поэтому все команды будут немного отличаться). Создаем lv для хранения логов var_log
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_21_01_09.png)

- Форматируем раздел в ext4.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_21_02_08.png)

- Монтируем var_log ,ставим rsync, чтобы синхронизировать разделы.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_21_03_12.png)

- Смотрим процессы в var/log  и останавливаем их.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_21_05_36.png)

- Меняем местами разделы, чтобы логи монтировались в hdd диски .
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_%20deb-met3_26_05_2020_21_06_40.png)

- Теперь через nano меняем путь монтирования var/log в файле /etc/fstab.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_27_05_2020_12_03_19.png)

- Потом нам надо изменить таблицу разделов чтобы уведомить файловую систему об изменения размера раздела.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_27_05_2020_11_55_29.png)

- Перезагружаем и делаем проверку.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_27_05_2020_12_03_49.png)
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part3/VirtualBox_deb-met3_27_05_2020_12_04_00.png)

Поздравляю мы перенесли и адаптировали raid на новые диски.




