# Операционные системы, Университет ИТМО, 2018

Задачи взяты со [страницы курса](http://neerc.ifmo.ru/~os/hw.html)

## Домашнее задание №0: Hello, world:

Написать программу, выводящую на экран надпись "Hello, world". При этом функция, печатающая строку и функция main должны
находиться в разных единицах трансляции. Сборка должна осуществляться с помощью утилиты make.

[Решение](HelloWorld/)

## Домашнее задание №1: Shell:

Необходимо написать упрощенный командный интерпретатор(shell).

shell должен:

    1. В бесконечном цикле, как настоящий shell
    2. Считывать со стандартного ввода программу, которую необходимо запустить, вместе с ее аргументами. 
    Программа задается в виде полного пути до исполняемого файла.
    3. Выполнять заданную программу с заданными аргументами
    4. Дожидаться завершения программы и выводить в стандартный поток вывода код ее завершения
    5. Необходимо обрабатывать ошибки всех используемых системных вызовов
    6. Необходимо обрабатывать конец вводу со стандартного потока

[Решение](Shell/)

## Практическое задание № 1: Cat

Написать подмножество утилиты cat, выводящей на стандартный поток вывода содержимое файла или символы, вводимые со стандартного потока ввода.

[Решение](Cat/)

## Домашнее задание №2: Grep:

Назовём "каноническим" путём к файлу среди множества возможных такой путь, в котором количество компонент минимально, а при равенстве количества компонент путь минимален лексикографически.

Grep принимает первым аргументом строку s и рекурсивно ищет файлы, в которых встречается эта строка, начиная с текущей директории, выводя канонические пути относительно текущей директории ко всем уникальным файлам, в которых строка нашлась. Порядок, в котором выводятся пути, не важен.

Файлы с одинаковым inode считаются одним и тем же файлом.

Необходимо корректно обрабатывать симлинки, но игнорировать символьные устройства, FIFO-пайпы и т.д.

[Решение](Grep/)

## Домашнее задание № 3: Кусочек JIT компилятора

Программа должна:

    1. Выделить память с помощью mmap(2)
    2. Записать в выделенную память машинный код, соответсвующий какой-либо функции
    3. Изменить права на выделенную память - чтение и исполнение. See: mprotect(2)
    4. Вызвать функцию по указателю на выделенную память
    5. Освободить выделенную память
    
Сильные духом призываются к возможности модификации кода выполняемой функции в runtime.

Записываемая в память программа представляет собой функцию *return_value*, не принимающую аргументов и возвращающую число 42.

Возможна модификация машинного кода этой фукнции во время выполнения числом, полученным в качестве аргумента вызова программы JitCompiler.

Варианты вызова:

    1. ./JitCompiler - вызов машинного кода, соответствующего функции return_vaue
    2. ./JitCompiler <число> - вызов машинного кода, соответсвующего изменённой версии 
    функции return_value. Вместо 42 она будет возвращать переданное программе в качестве 
    аргумента <число>

### Замечания по сборке
С помощью shell-скрипта *compile_for_gdb.sh* можно скомпилировать файл *return_value.cpp* таким образом, чтобы информация о машинном коде функции *return_value* была доступна для gdb.

С помощью shell-скрипта *gdb.sh* можно вывести информацию о машинном и ассемблерном коде функции *return_value* на экран.

Разрешить их запуск можно с помощью команд

    $ chmod +x compile_for_gdb.sh
    $ chmod +x gdb.sh

[Решение](JitCompiler/)

## Домашнее задание № 4: Библиотеки
Необходимо создать статическую, и две динамических библиотеки и программу, которая будет их использовать.

Помимо этого нужен Makefile, с помощью которого можно будет собрать все части.

Статическая библиотека должна:

    1. Собираться статически
    2. Предоставлять какие-то функции

Первая динамическая библиотека должна:

    1. Собираться динамически
    2. Динамически линковаться с программой на этапе линковки
    3. Предоставлять какие-то функции

Вторая динамическая библиотека должна:

    1. Собираться динамически
    2. Предоставлять какие-то функции

Программа должна

    1. Статически линковаться с статической библиотекой и вызывать предоставляемые ей функции
    2. Динамически линковаться с первой динамической библиотекой и вызывать предоставляемые ей функции
    3. Во время выполнения в явном виде загружать вторую динамическию библиотеку с помощью dlopen(3) 
    и вызывать какие-то функции из нее

### Замечания по сборке
Каждая из библиотек собирается независимо, с помощью запуска утилиты make в корне директории этой библиотеки.

Программа так же собирается запуском make в корне директории *utility*. В случае отсутствия собранных библиотек, запуск make в директории *utility* попробует собрать так же и их, но для этого должен быть разрешён запуск shell-скриптов *build_static.sh* и *build_shared.sh*. Разрешить их запуск можно с помощью команд

    $ chmod +x build_static.sh
    $ chmod +x build_shared.sh

После этого библиотеки могут быть собраны с помощью запуска make в директории *utility*. 

На момент запуска программы вторая динамическая библиотека (загружаемая на этапе выполнения) должна быть уже собрана с помощью утилиты make.

[Решение](Libs/)

## Домашнее задание № 5: Синхронные сокеты
Необходимо попробовать клиент-серверное взаимодействие через синхронные сокеты.

Помимо этого нужен Makefile, с помощью которого можно будет собрать клиент и сервер.

Семейство протоколов для использования на выбор: AF_UNIX, AF_INET, AF_INET6

Сервер должен:

    1. В качестве аргументов принимать адрес, на котором будет ожидать входящих соединений.
    2. Стартовать, делать bind(2) на заданный адрес и ожидать входящих соединений
    3. При получении соединения, выполнять серверную часть придуманного вами протокола
    4. После обработки принятого соединения возвращаться в режим ожидания входящих соединений

Клиент должен:

    1. Принимать параметром адрес, к которому стоит подключиться
    2. Выполнять клиентскую часть придуманного вами протокола
    3. Завершаться

Для сильных духом предлагается выбрать какой-то существующий протокол и имплементировать его, или его разумное подмножество.

Сильность духа будет оцениваться в два балла, при условии что выбранный протокол сложнее чем ECHO(https://tools.ietf.org/html/rfc862).

### Описание протокола:

Реализован протокол Dictionary Server Protocol.

https://tools.ietf.org/html/rfc2229

Поддерживаемые на данный момент комады:

    3.1 - Initial connection
    3.2 - DEFINE
    3.3 - MATCH
    3.5 - SHOW
    3.6 - CLIENT
    3.7 - STATUS
    3.8 - HELP
    3.9 - QUIT

### Замечания по сборке
Сборка осуществляется с помощью запуска утилиты make в корне директории с решением. При этом сборкой и запуском разных компонентов программы управляют разные цели make-файла.

    1. make all - собрать и клиент, и сервер.
    2. make client - собрать клиента.
    3. make server - собрать сервер.
    4. make run-client ADDRESS=address - запустить клиента, передав ему в качестве адреса сервера, 
    к которому он должен подключаться, address. Например, make run-client ADDRESS=127.0.0.1
    5. make run-server - запустить сервер.
    6. make clean - удалить из директории запускаемые и объектные файлы.

[Решение](Client-server/)

## Домашнее задание № 6: Мультиплексирование ввода-вывода

### (Yet another implementation of hello server)

Необходимо попробовать клиент-серверное взаимодействие с использованием механизмов мультиплексирования.

Помимо этого нужен Makefile, с помощью которого можно будет собрать клиент и сервер.

Семейство протоколов для использования на выбор: AF_UNIX, AF_INET, AF_INET6

Сервер должен:

    1. В качестве аргументов принимать адрес, на котором будет ожидать входящих соединений.
    2. Стартовать, делать bind(2) на заданный адрес и ожидать входящих соединений с использованием 
    одного из механизмов мультиплексирования
    3. При получении соединения, добавлять дескриптор в механизм мультиплексирования и ожидать событий и на нем
    4. Выполнять на принятых соединениях серверную часть протокола
    5. По завершении обработки соединения удалять все события из механизмов мультиплексирования

Клиент должен:

    1. Принимать параметром адрес, к которому стоит подключиться
    2. Используя механизмы мультиплексирования подключаться к серверу
    3. Используя механизмы мультиплексирования выполнять клиентскую часть протокола
    4. Завершаться

### Описание протокола:

Сервер принимает от клиента строку-запрос, завершающуюся символами "\r\n". Для каждой строки-запроса *request* сервер отправляет обратно строку-ответ *Hello, request*, так же заканчивающуюся символами "\r\n".

Клиент читает со стандартного ввода строку-запрос (любая строка со стандартного ввода, заканчивающаяся переводом строки), добавляет к ней терминирующие символы "\r\n", и отправляет её на сервер. Киент заканчивает работу вместе с окончанием стандартного ввода (либо по нажатию комбинации клавиш *ctrl + D*, либо по вводу пустой строки).

### Замечания по сборке
Сборка осуществляется с помощью запуска утилиты make в корне директории с решением. При этом сборкой и запуском разных компонентов программы управляют разные цели make-файла.

    1. make all - собрать и клиент, и сервер.
    2. make client - собрать клиента.
    3. make server - собрать сервер.
    4. make run-client ADDRESS=address PORT=port - запустить клиента, передав ему в качестве адреса сервера, 
    к которому он должен подключаться, address, а в качестве порта port. Например, 
    make run-client ADDRESS=127.0.0.1 PORT=1337
    5. make run-server PORT=port - запустить сервер, который будет слушать на порту port. Например,
    make run-server PORT=1337
    6. make clean - удалить из директории запускаемые и объектные файлы.

[Решение](IO-multiplexing/)


## Домашнее задание № 7: Межпроцессное взаимодействие

Необходимо получить опыт работы с IPC. Нужно создать приложение клиента и сервера.

Клиент и сервер обшаются через UNIX сокет.

Клиент подключается к серверу через UNIX сокет, получает от сервера файловый дескриптор, соответсвующий объекту какого-либо вида IPC.

Клиент и сервер выполняют какое-то взаимодействие используя IPC

Сервер должен:

    1. Ожидать подключений на UNIX сокете
    2. Для новых соединений создавать новый вид IPC, объекты которого представимы в виде файловых дескрипторов
    3. Передавать через UNIX сокет клиенту файловый дескриптор IPC соответсвующий клиенту
    4. Ожидать выполнения какого-либо протокола поверх IPC с клиентом

Клиент должен:

    1. Подключиться на UNIX сокет к серверу
    2. Получать в виде файлового дескриптора клиентскую часть IPC
    3. Взаимодействовать с сервером через IPC для выполнения какого-либо протокола

Клиент присылает серверу два числа, сервер считает их сумму и возвращает ответ клиенту. Клиент выводит ответ на консоль.

Для взаимодействия между процессами используются пайпы (pipe).

### Замечания по сборке
Сборка осуществляется с помощью запуска утилиты make в корне директории с решением. При этом сборкой и запуском разных компонентов программы управляют разные цели make-файла.

    1. make all - собрать и клиент, и сервер.
    2. make client - собрать клиента.
    3. make server - собрать сервер.
    4. make run-client SOCK_NAME=name A=a B=b - запустить клиента, передав ему name в качестве имени 
    файла, представляющего Unix-сокет в файловой системе, a в качестве первого числа, b в качестве второго. 
    Например, make run-client SOCK_NAME=unix_socket A=13 B=37
    5. make run-server SOCK_NAME=name - запустить сервер,передав ему name в качестве имени 
    файла, представляющего Unix-сокет в файловой системе. Например, make run-server  SOCK_NAME=unix_socket
    6. make clean - удалить из директории запускаемые и объектные файлы.

Имена файла, представляющего Unix-сокет в файловой системе, должны совпадать для сервера и клиента.

[Решение](IPC/)

## Сборка:

Каждое из домашних заданий находится в отдельной директории и собирается запуском утилиты make в корне этой директории. При этом в каждом Makefile прописана цель clean, удаляющая из директории исполняемые и объектные файлы.
