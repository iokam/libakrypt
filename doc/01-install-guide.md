Инструкция по сборке и установке
--------------------------------

Получение исходных текстов библиотеки
=====================================

Cтабильная версия библиотеки распространяется в виде архива с исходными текстами,
в котором x обозначает старший номер релиза, а y младший номер релиза.


    libakrypt-0.x.y.tar.bz2


Для разархивирования пользователи `unix`-систем могут выполнить команду


    tar -xjvf libakrypt-0.x.y.tar.bz2


Эту команду, как и все последующие, необходимо выполнять в консоли от имени обычного пользователя.
Пользователи `Windows` могут воспользоваться для разархивирования исходных 
текстов программой [7-Zip](https://www.7-zip.org/).

После разархивирования должен появиться корневой каталог `libakrypt-0.x`, содержащий следующие подкаталоги

* `aktool` - консольная утилита, использующая функции библиотеки,
* `cmake`  - управляющие файлы для системы сборки,
* `doc` - документация,
* `examples` - примеры, иллюстрирующие применение функций библиотеки, а также тесты,
* `source` - исходные тексты библиотеки,


Последняя, возможно не самая стабильная, версия исходных текстов библиотеки
может быть получена из репозитория на сайте [github.com](http://github.com/axelkenzo/libakrypt-0.x).
Для клонирования репозитория необходимо выполнить из командной строки следующую команду.


    git clone https://github.com/axelkenzo/libakrypt-0.x


Необходимая документация и описание процедуры установки распределенной
системы управления версиями `git` могут быть найдены на сайте [git-scm.org](http://git-scm.com).
Например, в Debian или Ubuntu, установка `git` может быть выполнена следующей командой,
запущенной с правами суперпользователя.


    apt install git


Система сборки
==============

Системой сборки для библиотеки `libakrypt` является [cmake](https://cmake.org").
Система сборки не зависит от используемой операционной системы.
Необходимый набор программ и утилит может быть скачан с официального сайта программы.

Установка `cmake`, как правило, доступна для стандартных средств операционной системы.
Например, в Debian или Ubuntu, установка `cmake` может быть выполнена следующей командой


    apt install cmake


Данная команда также должна выполняться с правами суперпользователя.


Зависимости библиотеки
======================

Текущая версия библиотеки зависит только от стандартной библиотеки языка Си `libc`.

**TODO:** Одной из важных, но не первостепенных задач, стоящих перед авторами библиотеки,
является сборка минимальной и переносимой конфигурации библиотеки `libc`,
для полного исключения зависимостей при сборке приложений.

Сборка в Unix
=============


Основной средой разработки библиотеки `libakrypt` является Linux,
поэтому процесс сборки под Unix-like операционными системами является максимально простой процедурой.

### Сборка библиотеки по-умолчанию ###

После получения исходных кодов и их разархивации,
для сборки библиотеки выполните в консоли следующую последовательность команд.


    mkdir build
    cd build
    cmake -DCMAKE_C_FLAGS="-march=native" ../libakrypt-0.x
    make


В результате сборки, по-умолчанию, будут собраны как динамическая, так и статическая версии 
библиотеки --- `libakrypt.so` и `libakrypt.a`,
вспомогательная библиотека `libakrypt-base` (соотвественно динамическая и статическая версии),
а также ряд тестовых примеров, использующих вызовы функций библиотеки.

Для отключения сборки, например, статической версии 
необходимо использовать флаг `AK_STATIC_LIB`, передаваемый `cmake`.


    cmake -DAK_STATIC_LIB=OFF ../libakrypt-0.x


Аналогичным образом отключается сборка динамической версии библиотеки.


    cmake -DAK_SHARED_LIB=OFF ../libakrypt-0.x


### Сборка различными компиляторами ###

Приведенная нами выше последовательность команд использует для сборки библиотеки
найденный `cmake` компилятор по-умолчанию -- в Linux этоб как правило,
компилятор `gcc`, во FreeBSD и MacOS это `clang`.
Если Вы хотите использовать другой компилятор, то Вам необходимо использовать
при вызове `cmake` опцию `CMAKE_C_COMPILER`, в явном виде определяющую имя компилятора.

Так, следующий вызов позволит произвести сборку библиотеки с помощью компилятора `clang`.


    cmake -DCMAKE_C_COMPILER=clang ../libakrypt-0.x


Аналогично, следующий вызов позволит произвести сборку 
библиотеки с помощью компилятора `tcc` ([Tiny C Compiler](https://bellard.org/tcc/))


    cmake -DCMAKE_C_COMPILER=tcc ../libakrypt-0.x


Отметим, что через опцию `CMAKE_C_COMPILER` можно указывать только те компиляторы,
которые поддерживаются `cmake`.
Перечень поддерживаемых компиляторов можно найти в документации по `cmake`
(см. раздел cmake-compile-features, supported compilers).


Сборка в Windows
================

Под управлением операционной системы семейства `Windows`
может быть собрана как статическая, так и динамическая версия библиотеки.
Далее мы описываем способ сборки библиотеки не использующий графические средства разработки.


### Сборка с использованием компилятора MSVC ###
На настоящий момент протестирована успешная сборка библиотеки с помощью компилятора MSVC версий 10 и старше.
Скачать бесплатно распространяемую среду сборки от `Microsoft` можно
скачать [здесь](https://visualstudio.microsoft.com/ru/vs/community).

Для сборки библиотеки необходимо запустить командную строку `Visual Studio`,
в которой переменными среды окружения установлен доступ к компилятору и системе сборки.
После чего необходимо
создать каталог для сборки, например, выполнив команду


    mkdir build-msvc


Далее, нужно перейти в созданный каталог и запустить `cmake` для конфигурации сборки.


    cmake -G "NMake Makefiles" -DCMAKE_C_FLAGS="-march=native" ../libakrypt-0.x


Флаг `-G` определяет имя механизма, используемого далее для сборки исходных текстов.
Сборка библиотеки и тестовых примеров выполняется следующей командой


    nmake

Если `-G` не будет указан, то `cmake` создаст проект для грфической среды `Visual Studio`.

По умолчанию для операционной системы `Windows` также будут собраны
статическая и динамическая версии библиотеки. 
Механизм отключения сборки либо статической, либо динамической версии библиотеки
аналогичен приведенному выше.


### Сборка с использованием компилятора GCC ###

Для сборки компилятором `gcc` Вам необходимо установить набор программ из проекта `MinGW`
(Minimalist GNU for Windows).
Версия компилятора для 32-х битных систем может быть найдена на сайте [mingw.org](http://mingw.org/wiki).
Различные варианты 64-х битных версий компилятора GCC могут быть
найдены на сайте [mingw-w64.org](http://mingw-w64.org/doku.php).


Для сборки библиотеки (в 32-х битной ОС) в командной строке можно
выполнить следующую последовательность команд.


    mkdir build
    cd build
    cmake -G "MinGW Makefiles" -DCMAKE_C_FLAGS="-march=native" ../libakrypt-0.x
    mingw32-make.exe

Флаги для сборки статической версии библиотеки аналогичны указанным выше.

Для тестирования библиотеки авторами используется
проект [MSyS2](https://www.msys2.org/), содержащий последие версии
компилятора `gcc`, а также консоль, эмулирующую
работу `unix`-системы и поддерживающую стандартные консольные команды, такие как ls, cat и т.п.


## Кросс-компиляция ##

Кросс-компиляция это процесс, в котором вы компилируете на одной машине программы,
которые будут выполняться на другой машине. Машина на которой происходит компиляция
называется хостом (host machine), а машина, для которой предназначается программа,
называется таргетом или целевой машиной (target machine).

Приведем пример кросс-компиляции для случая, когда в качестве хоста выступает машина с архитектурой
`x86` под управлением ОС `Linux`, а целевой машиной служат машины с архитектурой `mips` - как 32-х разрядная, 
так и 64-х разрядная. Для начала необходимо установить соответствующий компилятор и программные 
средства для тестирования (мы используем Debian).


    apt install gcc-mips-linux-gnu gcc-mips64-linux-gnuabi64
    apt install qemu-system-mips

В первой строке мы устанавливаем кросс-компилятор `gcc` для `mips`-архитектуры процессора,
а во второй - виртуальную машину [qemu](https://www.qemu.org/).

Теперь для сборки библиотеки и запуска тестовых примеров под архитектурой mips32r2 достаточно выполнить


    cmake -DCMAKE_C_COMPILER="mips-linux-gnu-gcc" 
                            -DCMAKE_C_FLAGS="-march=mips32r2" ../libakrypt-0.x/
    make
    qemu-mips-static -L /usr/mips-linux-gnu/ ./example-hello


В первой строке мы настраиваем сборку библиотеки с использованием компилятора `mips-linux-gnu-gcc`,
реализующего процесс генерации кода, исполняемого на mips32r2 архитектуре. В третьей строке
мы используем виртуальную машину `qemu` для запуска скомпилированного кода.

Аналогично, для сборки библиотеки и запуска тестовых примеров под архитектурой mips64r2 достаточно выполнить


    cmake -DCMAKE_C_COMPILER=mips64-linux-gnuabi64-gcc 
                            -DCMAKE_C_FLAGS="-march=mips64r2" ../libakrypt-0.x/
    make
    qemu-mips64-static -L /usr/mips64-linux-gnuabi64/ ./test-bckey04


## Сборка тестовых примеров ##

При сборке библиотеки компилятор также собирает ряд тестовых примеров,
используя для этого статическую версию библиотеки.
После сборки библиотеки, для запуска системы тестов под `Unix` достаточно запустить


    make test

В `Windows` необходимо будет запустить


    nmake test

или

    mingw32-make test

в зависимости от того, какой компилятор используется для сборки библиотеки и тестовых примеров.
После прохождения тестов будет выведена информация об общем числе успешно пройденных тестов,
а также времени их работы.

## Инсталляция библиотеки ##

В текущей версии библиотеки поддерживается процесс
инсталляции библиотеки только под `unix`-системами.

По умолчанию предполагается, что библиотека будет установлена в каталог `/usr/local`.
Для изменения этого каталога
можно передать в `cmake` путь установки в явном виде. Например, следующий вызов позволяет
установить библиотеку в католог `/usr`.


    cmake -DCMAKE_INSTALL_PREFIX=/usr ../libakrypt-0.x


Для инсталляции библиотеки достаточно выполнить команду


    make install


**Внимание.** Команда инсталляции библиотеки должна выполняться с правами суперпользователя.


## Сборка документации ##


Для сборки полной документациии необходимо дополнительно установить
программу [Doxygen](http://www.doxygen.org/index.html).
Также желательно установить LaTex и программу [pandoc](https://pandoc.org)


    apt install doxygen pandoc texlive-full


После этого станет возможным собрать документацию в форматах `html` и `pdf` с помощью
вызова команды


    make doc


Созданная документация будет находиться в подкаталоге `doc` каталога, в котором выполняется сборка,
а также будет упакована в архив `libakrypt-doc-0.x.y.tar.bz2`,
расположенный в каталоге сборки библиотеки.

Дополнительно,
если у Вас установлена программа `qhelpgenerator` (входит в стандартную 
установку библиотеки [Qt](https://www.qt.io/)),
то указанный вызов также создаст документацию в формате `qch`
(в каталоге сборки должен появиться файл `libakrypt-doc-0.x.y.qch`).


## Перечень опций для сборки библиотеки ##

В заключение, приведем перечень опций, которые могут передаваться в `cmake` для настройки и уточнения значений
параметров сборки библиотеки.
Данные параметры определены только для библиотеки `libakrypt` и дополняют существующие флаги `cmake`.

Применение опции  производится с помощью следующего вызова.


    cmake -D<имя опции>=<значение опции>  ../libakrypt-0.x


Вывести значения установленных опций можно с помощью вызова 


    cmake -L ../libakrypt-0.x

### AK_CA_PATH ###

Опция `AK_CA_PATH` устанавливает каталог для инсталляции и последующего
поиска доверенных корневых сертификатов. 

*Принимаемые значения*: произвольная строка символов.

*Значение по умолчанию*: в Unix-подобных системах значением является каталог 
`${CMAKE_INSTALL_PREFIX}/share/ca-certificates/libakrypt`,
для Windows-подобных систем значение не определено.
    
### AK_CONFIG_PATH ###

Опция `AK_CONFIG_PATH` устанавливает каталог инсталляции и последующего
поиска конфигурационного файла `libakrypt.conf`, содержащего точные
значения технических и криптографических характеристик библиотеки.

*Принимаемые значения*: произвольная строка символов.

*Значение по умолчанию*: в Unix-подобных системах значением является каталог `/etc`,
для Windows-подобных систем значение не определено.


### AK_SHARED_LIB ###

Опция `AK_SHARED_LIB` определяется в `CMakeLists.txt` следующим образом


    option( AK_SHARED_LIB "Build the shared library" ON )

Опция устанавливает, нужно ли собирать динамическую версию библиотеки.
Отметим, что одновременно может быть собрана только одна версия библиотеки либо динамическая, либо статическая.

*Принимаемые значения*: `ON`, `OFF`.

*Значение по-умолчанию*: `ON`.

### AK_STATIC_LIB ###

Опция `AK_STATIC_LIB` определяется в `CMakeLists.txt` следующим образом


    option( AK_STATIC_LIB "Build the static library" ON )

Опция устанавливает, нужно ли собирать статическую версию библиотеки.
Отметим, что одновременно может быть собрана только одна версия библиотеки либо динамическая, либо статическая.

*Принимаемые значения*: `ON`, `OFF`.

*Значение по-умолчанию*: `ON`.

### AK_TESTS ###

Опция `AK_TESTS` определяется в `CMakeLists.txt` следующим образом


    option( AK_TESTS "Build test for libakrypt" OFF )

Опция добавляет сборку нескольких тестовых примеров,
иллюстрирующих использование функций библиотеки.
После сборки тестовых примеров становится доступной команда, запускающая процесс тестирования.


    make test

*Принимаемые значения*: `ON`, `OFF`.

*Значение по-умолчанию*: `OFF`.

### AK_TESTS_GMP ###

Опция `AK_TESTS_GMP` определяется в `CMakeLists.txt` следующим образом


    option( AK_TESTS_GMP  
                    "Build comparison tests for gmp and libakrypt" OFF )

Опция добавляет к сборке библиотеки сборку нескольких тестовых примеров,
проводящих вычисления с использованием библиотеки [libgmp](http://gmplib.org).
Данные тестовые примеры сравнивают корректность реализации
арифметических операций с большими целыми числами
для библиотеки `libakrypt` и библиотеки `libgmp`.
Включение этой опции автоматически приводит к включению опции `AK_TESTS`.

После сборки тестовых примеров становится доступной команда, запускающая процесс тестирования.


    make test

*Принимаемые значения*: `ON`, `OFF`.

*Значение по-умолчанию*: `OFF`.