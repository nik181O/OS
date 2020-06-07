# Лабораторная работа №2 
## Часть №1: Установка ос ,создание и настройка RAID и LVM
- Создаем виртуалку по данной спецификации (1 gb ram, 1 cpu, 2 диска : ssd1, ssd2) , sata на 4 порта
- При установке ос выбираем ручную настройку разметки дисков. 
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_45_10.png)
- Создаем новый раздел для boot. 
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_47_25.png)
- Выбираем размер.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_47_41.png)
- Вот результат наших действий
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_48_46.png)
- Создав второй раздел на диске и использовав всю оставшуюся память диска, делаем этот раздел физическим томом для RAID'а.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_49_24.png)
- Делаем аналогичные предыдущим двум операциям действия на другом диске и получаем данный результат и заходим в "Configure software RAID".
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_49_51.png)
- После этого выбираем "create MD device" , чтобы создать raid массивы.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_50_40.png)
- Дальше выбираем RAID 1. Т.к. не смотря на то ,что этот тип RAID'ов довольно "медленный" ,у него очень сильная отказаустойчивость. Это достигается тем ,что каждый диск в этом рэйде идентичен другим, поэтому если откажет один диск все данные будут на другом и их можно легко копировать на другой диск.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_50_54.png)
- Потом мы выбираем те разделы диска , которые являются физическими томами для RAID'а.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_51_43.png)
- И заканчиваем создание raid массива.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_52_00.png)
- Смотрим на результат наших действий.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_52_23.png)
- Теперь мы выбираем "Configure the Logical Volume Manager", чтобы создать LVM.LVM - это такая абстракция которая позволяет собрать разные диски а затем разбить получившуюся солянку так как мы хотим.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_52_52.png)
- Здесь нам нужно создать volume groop - группу томов , которая обьединет физические тома PV в группу , который мы будем разбивать так как захотим.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_53_32.png)
- Называем ее 'system'
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_55_47.png)
- Здесь мы выбираем наше RAID устройство '/dev/md0'
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_56_18.png)
- Дальше создаем LV для VG. LV - это логические разделы нашей VG, который впоследствии сформатируются и будут использоваться как обычные разделы наших дисков. Первый LV - root, размер 2/5 от рэйд устройства.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_57_12.png)
- Потом создаем еще два lv:log(размер 1\5) и var(размер 2\5).
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_58_53.png)
- Смотрим на результат наших действий.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_19_59_21.png)
- После этого мы закончили настройку разметок дисков. Но установщик скажет что у нас не смонтирован отдельный раздел в диске sdb и спросит , хотим ли мы вернуться и смонтировать. Отвечаем нет.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_20_01_17.png)
- Потом еще одна рекомендация от системы создать свап пространство. Тоже отвечаем нет.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_20_01_25.png)
- И последний вопрос , записать ли изменения на диск. Отвечаем положительно.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_Debian-raid_23_05_2020_20_01_34.png)
- После установки ос открывается ее терминал. И тут мы должны ввести команду dd if=/dev/sda1 of=/dev/sdb1. Чтобы скопировать boot  на второй диск.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_deb-met_23_05_2020_21_10_25.png)
- Дальше ищем диск на котором не стоит grub  и ставим его туда.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_deb-met_23_05_2020_21_14_32.png)
- Смотрим данные по lvm и монтируем системные файлы плюс lv к pv.
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_deb-met_23_05_2020_21_23_52.png)
![Image alt](https://github.com/Metamfitamin/MetLab/blob/master/lab2/part1/VirtualBox_deb-met_23_05_2020_21_24_05.png)
- Это все , теперь мы создали работающий RAID1, который будет служить нам верой и правдой в следующих частях этой лабы.

