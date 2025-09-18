# Information

### Main commands for Linux & Windows [Part 1].

| Windows                                        | Linux                                               | Action                                        |
| ---------------------------------------------- | --------------------------------------------------- | --------------------------------------------- |
| `md FOLDER_NAME`                               | `mkdir FOLDER_NAME`                                 | Создание папки                                |
| `md FOLDER_NAME_1 FOLDER_NAME_2 ...`           | `mkdir FOLDER_NAME_1 FOLDER_NAME_2 ...`             | Создать несколько папок                       |
| `mkdir FOLDER\IN\ANOTHER\FOLDER`               | `mkdir -p FOLDER/IN/ANOTHER/FOLDER`                 | Создать вложенную папку (и все промежуточные) |
| `md FOLDER_CREATED_AT_%date%`                  | `mkdir "FOLDER_CREATED_AT_$(date +%Y-%m-%d)"`       | Создание папки с датой                        |
| `for /l %i in (1,1,10) do md FOLDER_NUMBER_%i` | `for i in {1..10}; do mkdir FOLDER_NUMBER_$i; done` | Цикл для создания папок                       |
| `net user USER_NAME PASSWORD /add /active:yes` | `sudo useradd USER_NAME && sudo passwd USER_NAME`   | Добавить пользователя                         |
| `netsh advfirewall set allprofiles state off`  | `sudo ufw disable`                                  | Выключить брандмауэр                          |
| `cd PATH`                                      | `cd PATH`                                           | Перейти в директорию                          |
| `cd ..`                                        | `cd ..`                                             | На уровень выше                               |
| `cd /?`                                        | `help cd` или `man cd`                              | Справка по `cd`                               |
| `cd /d PATH`                                   | `cd /PATH/TO/DIR`                                   | Перейти на другой диск/раздел                 |
| `dir PATH`                                     | `ls PATH`                                           | Список файлов и папок                         |
| `dir /w PATH`                                  | `ls PATH` *(широкий формат по умолчанию)*           | Список файлов в широком формате               |
| `dir /a PATH`                                  | `ls -a PATH`                                        | Скрытые и системные файлы                     |
| `cls`                                          | `clear`                                             | Очистить экран                                |
| `dir PATH /P`                                  | `ls PATH \| less`                                   | Список постранично                            |
| `tree`                                         | `ls -R PATH` *(или `tree`, если установлен)*        | Дерево папок                                  |
| `tree /f`                                      | `ls -R PATH`                                        | Дерево папок с файлами                        |
| `tree /a`                                      | `ls -R PATH -a`                                     | Дерево включая скрытые                        |
| `rd FOLDER_NAME`                               | `rmdir FOLDER_NAME`                                 | Удалить пустую папку                          |
| `rd FOLDER_NAME_1 FOLDER_NAME_2 ...`           | `rmdir FOLDER_NAME_1 FOLDER_NAME_2 ...`             | Удалить несколько пустых папок                |
| `rd FOLDER_NAME /s /q`                         | `rm -rf FOLDER_NAME`                                | Удалить папку с содержимым                    |
| `rmdir FOLDER_NAME /s /q`                      | `rm -rf FOLDER_NAME`                                | То же самое                                   |
| `del FILE_NAME`                                | `rm FILE_NAME`                                      | Удалить файл                                  |
| `del DISK:\*.*`                                | `rm /PATH/TO/DIR/*`                                 | Удалить все файлы в папке                     |
| `del DISK:\*.EXT`                              | `rm /PATH/TO/DIR/*.EXT`                             | Удалить файлы по расширению                   |

---

### Main commands for Linux & Windows [Part 2].

| Windows                        | Linux                                                  | Action                                                                   |
| ------------------------------ | ------------------------------------------------------ | ------------------------------------------------------------------------ |
| `copy FROM TO`                 | `cp FROM TO`                                           | Копировать файл                                                          |
| `copy DISK:\*.* DEST`          | `cp /PATH/TO/DIR/* DEST`                               | Копировать все файлы в папке                                             |
| `copy DISK:\a*.* DEST`         | `cp /PATH/TO/DIR/a* DEST`                              | Копировать все файлы, начинающиеся на `a`                                |
| `copy DISK:\*.EXT DEST`        | `cp /PATH/TO/DIR/*.EXT DEST`                           | Копировать все файлы с расширением `.EXT`                                |
| `xcopy /?`                     | `man cp` или `man rsync`                               | Справка по расширенному копированию                                      |
| `xcopy LOCATION DEST /e /y`    | `cp -r LOCATION DEST` *(или `rsync -a LOCATION DEST`)* | Копировать директорию с подпапками (**/e**) и без подтверждений (**/y**) |
| `robocopy /?`                  | `man rsync`                                            | Справка по мощному копированию                                           |
| `robocopy LOCATION DEST`       | `rsync -av LOCATION DEST`                              | Надёжное копирование папки (robocopy аналог rsync)                       |
| `move LOCATION DEST`           | `mv LOCATION DEST`                                     | Переместить или переименовать файл/папку                                 |
| `list disk` *(в `diskpart`)*   | `lsblk` или `fdisk -l`                                 | Показать список дисков                                                   |
| `list volume` *(в `diskpart`)* | `lsblk -f` или `df -h`                                 | Показать список томов/разделов                                           |

---

### Main commands for Linux & Windows [Part 3].

| Windows                                                       | Linux                                                                            | Action                                                          |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `echo %PATH%`                                                 | `echo $PATH`                                                                     | Показать текущие пути поиска исполняемых файлов                 |
| `set PATH=C:\Folder;%PATH%`                                   | `export PATH=/home/user/my_programs:$PATH`                                       | Временно добавить путь для запуска программ из любого места     |
| `setx PATH "C:\Folder;%PATH%"`                                | Добавить `export PATH=/home/user/my_programs:$PATH` в `~/.bashrc` или `~/.zshrc` | Добавить путь навсегда                                          |
| Папки по умолчанию: `C:\Windows\System32`, `C:\Program Files` | Папки по умолчанию: `/usr/local/bin`, `/usr/bin`, `/bin`, `~/bin`                | Пути, где терминал ищет исполняемые файлы                       |
| `where PROGRAM_NAME`                                          | `which PROGRAM_NAME`                                                             | Показать полный путь к исполняемому файлу, который будет вызван |

> Стоит учесть что после добавления любой программы в исполняемые, программу[или скрипт] можно будет `удобно` и `быстро` вызвать через терминал просто вписав `название`.

| Windows                                                       | Linux                                                                            | Action                                                          |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| `type FILE_NAME`                                              | `cat FILE_NAME`                                   | Показать информацию из файла                 |
| `find VALUE DEST_WHERE_IS_VALUE`                              | `grep [options] PATTERN [FILE...]`                                   | Фильтрация                |

> Используя `>` можно записать файл, так команда `ls > text.txt` запишет все файлы директория в файл `text.txt`, а `>>` добавит информацию а не перезапишет как `>`.

---

### Disk management & Managing volumes and partitions

##### ...
