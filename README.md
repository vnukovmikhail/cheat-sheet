# Information

### Main commands for Linux & Windows [Part 1]

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

### Main commands for Linux & Windows [Part 2]

### ...
