**bauh** ( ба-оо ), ранее известный как **fpakman**,  представляет собой графический интерфейс для управления приложениями / пакетами Linux. Он способен управлять AUR, AppImage, Flatpak, Snap и нативными веб-приложениями. Когда вы запускаете **bauh** 
появляется панель управления, в которой вы можете искать, обновлять, устанавливать, удалять и запускать приложения ( в некоторых случаях также возможено откат версии).

Он имеет **режим лотка** ( see [Settings](https://github.com/vinifmor/bauh/tree/wgem#general-settings) ) - приложение прикрепляется в виде значка в системном трее. Прикрепленный значок станет красным, когда обновления будут доступны.

У этого проекта есть официальный аккаунт в Twitter ( **@bauh4linux** ) так что пользователи могут оставаться в курсе его новостей.

Чтобы внести свой вклад взгляните на [CONTRIBUTING.md](https://github.com/vinifmor/bauh/blob/master/CONTRIBUTING.md)


![management panel](https://raw.githubusercontent.com/vinifmor/bauh/master/pictures/panel.png)


### Разработан с помощью
- Python3 и Qt5

### Базовое требования

#### Дистрибутивы на основе Debian
- **python3.5** или выше
- **pip3**
- **python3-requests**
- **python-yaml**
- **qt5dxcb-plugin**
- **python3-venv** ( только для [Manual installation](https://github.com/vinifmor/bauh/tree/wgem#manual-installation) )
- **libappindicator3** ( для **режима лотка** в GTK3-окружении )

#### Дистрибутивы на основе Arch
- **python**
- **python-requests**
- **python-pip**
- **python-pyqt5**
- **python-yaml**
- **libappindicator-gtk3** ( для **режима лотка** в GTK3-окружении  )

##### Опционально
- **flatpak**: чтобы иметь возможность обрабатывать приложения Flatpak
- **snapd**: чтобы иметь возможность обрабатывать приложения Snap
- **pacman**: чтобы иметь возможность обрабатывать пакеты AUR 
- **wget**: чтобы иметь скачивать пакеты AUR
- **git**: чтобы иметь возможность окатить версию пакетов AUR
- **aria2**: более быстрая загрузка исходных файлов AUR (сокращает время установки пакетов. Более подробная информация приведена ниже)
- **libappindicator3**: для **режима лотка** в GTK3-окружении

Другие требования зависят от типа приложений, которыми вы хотите управлять ( смотри [Gems](https://github.com/vinifmor/bauh/tree/wgem#gems--package-technology-support-) ).

### Установка

**AUR**

Как пакет [**bauh**](https://aur.archlinux.org/packages/bauh). Также существует т.н. staging-версия ([**bauh-staging**](https://aur.archlinux.org/packages/bauh-staging)), но она предназначена для тестирования и может не работать должным образом.

[**PyPi**](https://pypi.org/project/bauh)

```pip3 install bauh ```

Это может потребовать ввода команды **sudo**, но предпочтительней воспользоваться **Ручной установкой **, как описано ниже (чтобы не испортить системные библиотеки).


### Ручная установка 
- Если вы предпочитаете установку вручную и изолированно, откройте ваше любимое приложение терминала и введите следующие команды:

```
python3 -m venv bauh_env ( creates a virtualenv in a folder called **bauh_env** )
bauh_env/bin/pip install bauh ( installs bauh in the isolated environment )
bauh_env/bin/bauh  ( launches bauh )
```
- P.S: если вы хотите запустить его в системном трее, замените последнюю команду на: 
```
bauh_env/bin/bauh --tray=1
```

- Чтобы обновить изолированный bauh до последней версии:
```
bauh_env/bin/pip install bauh --upgrade
```

- Чтобы удалить его, просто удалите папку **bauh_env** 

- Чтобы создать ярлык (запись на рабочем столе) для него в системном меню ( предполагается, что вы создали изолированную среду в домашней папке с помощью Python 3.7 ):
    - Создайте файл с именем **bauh.desktop** in **~/.local/share/applications** со следующим содержимым:
```
[Desktop Entry]
Type=Application
Name=bauh
Comment=Install and remove applications ( AppImage, AUR, Flatpak, Snap )
Exec=/home/$USER/bauh_env/bin/bauh
Icon=/home/$USER/bauh_env/lib/python3.7/site-packages/bauh/view/resources/img/logo.svg
```

- Если вы хотите создать ярлык для лотка, поместите параметр **--tray=1** в конец строки  **Exec** приведенного выше примера ( например: **Exec=/home/$USER/bauh_env/bin/bauh --tray=1** )
- P.S: Если ярлык не работает, попробуйте заменить переменную **$USER** вашим именем пользователя.

### Автозапуск
Для автоматического запуска приложения используйте настройки среды рабочего стола, чтобы зарегистрировать его как запускаемое приложение / сценарий (**bauh --tray=1**). Или
создайте файл с именем **bauh.desktop** в **~/.config/autostart** со следующим содержимым:
```
[Desktop Entry]
Type=Application
Name=bauh ( tray )
Exec=/path/to/bauh --tray=1
```

### Деинсталляция
Перед удалением bauh с помощью диспетчера пакетов рассмотрите возможность выполнения `bauh --reset` для удаления конфигурационных и кэшированных файлов, хранящихся в вашем каталоге **HOME**.


### Gems ( поддержка пакетных технологий )
#### Flatpak ( flatpak )
- Пользователь может осуществлять поиск, установку, удаление, откат версий, запуск и извлечение истории приложений.

![flatpak_search](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/flatpak/search.gif)

- Файл конфигурации находится по адресу **~/.config/bauh/flatpak.yml** . Он позволяет осуществить следующие настройки:
```
installation_level: null # defines a default installation level: user or system. ( the popup will not be displayed if a value is defined )
```

- Обязательные зависимости:
    - Для всех дстрибутивов: **flatpak**

#### Snap ( snap )
- Пользователь может осуществлять поиск, установку, удаление, обновление, запуск и откат версий приложений.

![snap_search](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/snap/search.gif)

- Обязательные зависимости:
    - Для всех дстрибутивов: **snapd** ( он должен быть включен после его установки. Подробности на сайте https://snapcraft.io/docs/installing-snapd )

#### AppImage ( appimage )
- Пользователь может осуществлять поиск, установку, удаление, откат версий, запуск и извлечение истории приложений

![appimage_search](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/appimage/search.gif)

- Поддерживаемые источники: [AppImageHub](https://appimage.github.io) ( **приложения, не имеющие релизов, опубликованных на GitHub, в настоящее время недоступны** )
- Установленные приложения хранятся по адресу **~/.local/share/bauh/appimage/installed**
- Записи рабочего стола (ярлыки меню ) установленных приложений хранятся по адресу **~/.local/share/applications**
- Загруженные файлы базы данных хранятся по адресу **~/.local/share/bauh/appimage** как **apps.db** и **releases.db**
- Базы данных всегда обновляются при запуске bauh
- Демон обновления баз данных работает каждые 20 минут ( его можно настроить с помощью файла конфигурации, описанного ниже )
- Если установлен **AppImageLauncher**, то во время установки AppImage могут произойти сбои. Рекомендуется удалить его и перезагрузить систему, прежде чем пытаться установить приложение.
- Все поддерживаемые имена приложений можно найти в файле [apps.txt](https://github.com/vinifmor/bauh-files/blob/master/appimage/apps.txt)
- Файл конфигурации находится по адресу **~/.config/bauh/appimage.yml** и позволяет осуществить следующие настройки:
```
db_updater:
  enabled: true  # if 'false': disables the daemon database updater ( bauh will not be able to see if there are updates for your already installed AppImages )
  interval: 1200  # the databases update interval in SECONDS ( 1200 == 20 minutes )
```
- Обязательные зависимости
    - Дистрибутивы на основе Arch: **sqlite**, **wget** ( или **aria2** для более быстрой многопоточной загрузки )
    - Дистрибутивы на основе Debian: **sqlite3**, **wget** ( или **aria2** для более быстрой многопоточной загрузки )
    - [**fuse**](https://github.com/libfuse/libfuse) может потребоваться для запуска AppImages в вашей системе
    - P.S: **aria2 будет использоваться только в том случае, если включена многопоточная загрузка**

#### AUR ( arch )
- Доступно только для **дистрибутивов на основе Arch**
- Пользователь может осуществлять поиск, установку, удаление, откат версий, запуск и извлечение истории приложений

![aur_search](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/aur/search.gif)

- Он обрабатывает конфликты и отсутствующие / необязательные пакеты установки (в том числе с зеркал вашего дистрибутива)
- Автоматически делает простые улучшения компиляции пакетов:

    a) если **MAKEFLAGS** не установлен в **/etc/makepkg.conf**, то копия файла **/etc/makepkg.conf** будет сгенерирована по адресу **~/.config/bauh/arch/makepkg.conf** с определением в MAKEFILE количества процессоров вашей машины (**-j${nproc}**).

    b) такой же, как и предыдущий, но связанный с определениями **COMPRESSXZ** и **COMPRESSZST** ( if '--threads=0' is not defined )
    
    c) **ccache** будет добавлен в **BUILDENV** если он установлен в системе, но не определен 

    Obs: Для получения дополнительной информации о них, взгляните на [Makepkg](https://wiki.archlinux.org/index.php/Makepkg)
- Во время инициализации bauh полный нормализованный индекс AUR сохраняется в  **/tmp/bauh/arch/aur.txt** и будет использоваться только в том случае, если AUR API не может обрабатывать количество совпадений для данного запроса.
- Если некоторые из установленных пакетов не классифицированы, отправьте электронное письмо по адресу **bauh4linux@gmail.com**, информирование их имен и категорий в следующем формате: ```name=category1[,category2,category3,...]```
- Файл конфигурации находится по адресу **~/.config/bauh/arch.yml** и позволяет осуществить следующие настройки:
```
optimize: true  # if 'false': disables the auto-compilation improvements
transitive_checking: true  # this property defines if dependencies of a dependency should be retrieved before the package installation. It avoids interruptions, since it will detect all required dependencies before the process begin.
sync_databases: true # package databases synchronization once a day ( or every device reboot ) before the first package installation / upgrade / downgrade
simple_checking: false  # this property defines how the missing dependencies checking process should be done before installing a package. When set to 'false' an algorithm combining pacman's methods and AUR's API is used ( currently slower, but more accurate ), whereas 'false' relies only on pacman's methods ( faster. but currently not always accurate ) 
``` 
- Обязательные зависимости:
    - **pacman**
    - **wget**
- Необязательные (опциональные) зависимости:
    - **git**: чтобы иметь возможность окатить версию пакетов AUR
    - **aria2**: обеспечивает более быструю многопоточную загрузку необходимых исходных файлов ( в настройках должен быть активирован параметр Многопоточная загрузка )

#### Собственные веб-приложения ( web )
- Позволяет устанавливать собственные веб-приложения, вводя их адреса / URL-адреса в строке поиска

![url_search](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/web/url_search.gif)

- Предлагает возможность настроить созданное приложение так, как вы хотите:

![options](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/web/options.png)

- Предоставляет некоторые предложения, поступающие с предопределенными настройками, они также могут быть получены по их именам. 
Они хранятся по адресу [suggestions.yml](https://github.com/vinifmor/bauh-files/blob/master/web/suggestions.yml) и загружаются во время использования приложения.

![suggestions](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/web/suggestions.gif)

- Полагается на [NodeJS](https://nodejs.org/en/), [Electron](https://electronjs.org/) и [nativefier](https://github.com/jiahaog/nativefier) чтобы сделать все волшебство, но вам не нужно, чтобы они были установлены в вашей системе. Изолированная среда установки
будет генерироваться в **~/.local/share/bauh/web/env**.
- Изолированная среда создается на основе параметров, определенных в разделе [environment.yml](https://github.com/vinifmor/bauh-files/blob/master/web/environment.yml)
 ( загружается во время выполнения ).
- Некоторые приложения требуют исправления Javascript для правильной работы. Если есть известное исправление, bauh загрузит файл из[fix](https://github.com/vinifmor/bauh-files/tree/master/web/fix) и прикрепит его к сгенерированному приложению.
- Установленные приложения находятся по адресу **~/.local/share/bauh/installed**.
- Для установленных приложений на рабочем столе будет сгенерирована запись / ярлык **~/.local/share/application**
- Если режим лотка  **Запускать свёрнутым** задан во время установки, запись рабочего стола также будет сгенерирована в **~/.config/autostart**
что позволяет приложению запускаться автоматически после загрузки системы, прикрепляясь к лотку.

![tray_mode](https://raw.githubusercontent.com/vinifmor/bauh/staging/pictures/web/tray.gif)
 
- Файл конфигурации находится по адресу **~/.config/bauh/web.yml** и позволяет осуществить следующие настройки:
```
environment:
  electron:
    version: null  # set a custom Electron version here ( e.g: '6.1.4' )
  system: false  # set it to 'true' if you want to use the nativefier version globally installed on your system 
```
- Необязательные (опциональные) зависимостиs: 
    - Дистрибутивы на основе Arch: **python-lxml**, **python-beautifulsoup4**
    - Дистрибутивы на основе Debian ( используют pip ): **beautifulsoup4**, **lxml** 

### Основные настройки

#### Переменная окружения / параметры
Некоторые параметры приложения можно изменить с помощью переменных окружения или аргументов (введите  ```bauh --help``` чтобы получить дополнительную информацию).
- `BAUH_TRAY (--tray )`: Если значок в трее и демон проверки обновлений должны быть созданы. Используйте `0` (выключено, по умолчанию) или `1` (включено).
- `BAUH_LOGS (--logs )`: включает журнал **bauh**  (для отладки). Используйте `0` (выключено, по умолчанию) или `1` (включено).
- `--reset`: очищает все конфигурации и кэшированные данные, хранящиеся в домашнем каталоге

#### Общий конфигурационный файл( **~/.config/bauh/config.yml** )
```
disk_cache:
  enabled: true  # allows bauh to save applications icons and data to the disk to load them faster when needed
download:
  icons: true # allows bauh to download the applications icons when they are not saved on the disk
  multithreaded: true  # allows bauh to use a multithreaded download client installed on the system to download applications source files faster ( current only **aria2** is supported )
gems: null  # defines the enabled applications types managed by bauh ( a null value means all available ) 
locale: null  # defines a different translation for bauh ( a null value will retrieve the system's default locale )
memory_cache:
  data_expiration: 3600 # the interval in SECONDS that data cached in memory will live
  icon_expiration: 300  # the interval in SECONDS that icons cached in memory will live
suggestions:
  by_type: 10  # the maximum number of application suggestions that must be retrieved per type
  enabled: true  # if suggestions must be displayed when no application is installed
system:
  notifications: true  # if system popup should be displayed for some events. e.g: when there are updates, bauh will display a system popup
  single_dependency_checking: false  # if bauh should check only once if for the available technologies on the system.
ui:
  style: null  # the current QT style set. A null value will map to 'Fusion' or 'Breeze' ( depending on what is installed )  
  table:
    max_displayed: 50  # defines the maximum number of displayed applications on the table.
  tray:  # system tray settings
    default_icon: null  # defines a path to a custom icon
    updates_icon: null  # defines a path to a custom icon indicating updates
  hdpi: true  # enables HDPI rendering improvements. Use 'false' to disable them if you think the interface looks strange
  auto_scale: false # activates Qt auto screen scale factor (QT_AUTO_SCREEN_SCALE_FACTOR). It fixes scaling issues for some desktop environments ( like Gnome )
updates:
  check_interval: 30  # the updates checking interval in SECONDS
  sort_packages: true  # if the selected applications / packages to upgrade must be sorted to avoid possible issues
  pre_dependency_checking: true  # displays all applications / packages that must be installed before upgrading the selected ones  
```
#### Иконки в трее
Приоритет: 
  1) Пути значков, определенные в **~/.config/bauh/config.yml**
  2) Значки из системы со следующими именами: `bauh_tray_default` и `bauh_tray_updates`
  3) Собственные пакеты значков

### Как улучшить производительность
- Отключите типы приложений, с которыми вы не хотите иметь дело
- Если вы не заботитесь о перезапуске приложения каждый раз, когда устанавливается новая технология поддерживаемых пакетов, включите `Однократная проверка системы`. Это может сократить время отклика приложения, так как ему не нужно будет перепроверять, доступны ли необходимые технологии в вашей системе каждый раз, когда выполняется данное действие.
- Если вы не против посмотреть значки приложений, вы можете отключить в настойках параметр `Скачать иконки`. Приложение может иметь небольшое улучшение отклика, так как это уменьшит IO и параллелизм внутри негоПриложение может иметь небольшое улучшение отклика, так как это уменьшит ввод-вывод и параллелизм внутри него
- Пусть`Дисковый кэш` будет всегда включен, так что чтобы **bauh** не нужно было динамически извлекать данные каждый раз, когда вы его запускаете. 


### Файлы и журналы
- Журналы установки и временные файлы сохраняются по адресу **/tmp/bauh** ( или **/tmp/bauh_root** если вы запустите его как root)
- Некоторые данные об установленных приложениях хранятся в **~/.cache/bauh** чтобы загрузить их быстрее ( поведение по умолчанию ).

### [bauh-files](https://github.com/vinifmor/bauh-files)
- Это отдельный репозиторий с некоторыми файлами, загружаемыми во время выполнения.

### Структура кода
#### Модули

**view**: код, связанный с графическим интерфейсом

**gems**: код, отвечающий за работу с различными технологиями упаковки (каждый подмодуль имеет дело с одним или несколькими типами )

**api**: абстракции кода, представляющие основные действия, которые пользователь может выполнять с пакетами Linux (поиск, установка,...). 
Эти абстракции реализуются посредством **gems**, и **view**, код только прикреплен к ним (он не знает, как **gems** обрабатывает эти действия)

**commons**: общий код, используемый **gems** и **view**

### Что ожидать в дальнейшем
- Поддержка других упаковочных технологий
- Отдельные модули для каждой технологии упаковки
- Улучшение памяти и производительности
- Улучшение пользовательского опыта
