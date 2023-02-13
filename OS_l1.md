## 1. Какой системный вызов делает команда cd? В прошлом ДЗ мы выяснили, что cd не является самостоятельной программой, это shell builtin, поэтому запустить strace непосредственно на cd не получится. Тем не менее, вы можете запустить strace на /bin/bash -c 'cd /tmp'. В этом случае вы увидите полный список системных вызовов, которые делает сам bash при старте. Вам нужно найти тот единственный, который относится именно к cd.
  
Ответ: 
 
`strace -o output.log /bin/bash -c 'cd /tmp' && egrep *tmp output.log`

**chdir("/tmp")                           = 0**



##2. Используя strace, выясните, где находится база данных file, на основании которой она делает свои догадки.

Ответ:  
 
man magic указывает на такую информацию:  

* The database of these “magic patterns” is usually located in a binary file in **/usr/share/misc/magic.mgc** or a directory of source text magic pattern fragment files in **/usr/share/misc/magic**.  The database  specifies what patterns are to be tested for, what message or MIME type to print if a particular pattern is found, and additional information to extract from the file. 

Попробуем выполнить file /bin/bash:
    openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libmagic.so.1", O_RDONLY|O_CLOEXEC) = 3   
    stat("/home/grigorii_azatyan/.magic.mgc", 0x7ffe5073a820) = -1 ENOENT (Нет такого файла или каталога)   
    stat("/home/grigorii_azatyan/.magic", 0x7ffe5073a820) = -1 ENOENT (Нет такого файла или каталога)   
    openat(AT_FDCWD, "/etc/magic.mgc", O_RDONLY) = -1 ENOENT (Нет такого файла или каталога)   
    stat("/etc/magic", {st_mode=S_IFREG|0644, st_size=111, ...}) = 0   
    openat(AT_FDCWD, "/etc/magic", O_RDONLY) = 3   
    openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3   

Результат:
  
 - Обращение к библиотеке **/lib/x86_64-linux-gnu/libmagic.so.1**;
 - Обращение к файлу **/usr/share/misc/magic.mgc**
  

## 3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков, предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

Представим ситуацию:
  
`ping google.com > ~/1.log   `    
`sudo rm -rf 1.log  `  
`sudo lsof | grep 1.log  `  

    ping      7770      kali    1w      REG      8,5    73432    1453088 /home/kali/output.log (deleted)  

Видим: PID 7770, файловый дескриптор 1.  
Значит, можно найти этот файловый дескриптор в /proc/7770/fd/1 и записать туда пустоту:        
`sudo su`   
`> /proc/7770/fd/1`    

**Результат:**    
`cat /proc/7770/fd/1  `  

    64 bytes from 173.194.220.101: icmp_seq=563 ttl=109 time=44.9 ms   
    64 bytes from 173.194.220.101: icmp_seq=564 ttl=109 time=47.9 ms   
    64 bytes from 173.194.220.101: icmp_seq=565 ttl=109 time=44.9 ms    
    64 bytes from 173.194.220.101: icmp_seq=566 ttl=109 time=45.3 ms   
    64 bytes from 173.194.220.101: icmp_seq=567 ttl=109 time=44.6 ms   
  
Файл обнулился на момент выполнения команды, что и требовалось в задании.


## 4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
Зомби не занимают памяти, но блокируют записи в таблице процессов, размер которой ограничен для каждого пользователя и системы в целом.

## 5. На какие файлы вы увидели вызовы группы open за первую секунду работы утилиты?

Ответ:

/usr/sbin/opensnoop-bpfcc  
      `PID    COMM               FD ERR PATH`  
      `1856   vminfo              4   0 /var/run/utmp`    
      `626    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services`  
      `626    dbus-daemon        28   0 /usr/share/dbus-1/system-services`  
      `626    dbus-daemon        -1   2 /lib/dbus-1/system-services`  
      `626    dbus-daemon        28   0 /var/lib/snapd/dbus-1/system-services/`  
      `2496   gnome-shell        27   0 /proc/self/stat`  


## 6. Какой системный вызов использует uname -a? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в /proc, где можно узнать версию ядра и релиз ОС.

Ответ:

    `uname({sysname="Linux", nodename="ubuntu-20", ...}) = 0`  
    `fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x1), ...}) = 0`  
    `uname({sysname="Linux", nodename="ubuntu-20", ...}) = 0`  
    `uname({sysname="Linux", nodename="ubuntu-20", ...}) = 0`  
    `write(1, "Linux ubuntu-20 5.8.0-53-generic"..., 115) = 115 ` 

Цитата из man:

* Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.


## 7. Чем отличается последовательность команд через ; и через && в bash? Есть ли смысл использовать в bash &&, если применить set -e?

Ответ:

- С использованем ; обе команды отработают в любом случае.    
- С использованием && вторая команда отработает только при успешном результате первой.  
- С применением set -e скрипт завершит работу при ненулевом коде возврата команды. Конструкция с && работать будет, но она необязательна, т.к. при другом условии оболочка просто завершится. 

## 8. Из каких опций состоит режим bash set -euxo pipefail и почему его хорошо было бы использовать в сценариях?

Ответ:

set -e прекращает выполнение скрипта, если команда завершилась ошибкой.  
set -u - прекращает выполнение скрипта, если встретилась несуществующая переменная.  
set -x - выводит выполняемые команды в stdout перед выполненинем.  
set -o pipefail - прекращает выполнение скрипта, даже если одна из частей пайпа завершилась ошибкой. В этом случае bash-скрипт завершит выполнение, если mycommand вернёт ошибку, не смотря на true в конце пайплайна: mycommand | true.  
Его использовать безопасно.  

## 9. Используя -o stat для ps, определите, какой наиболее часто встречающийся статус у процессов в системе. В man ps ознакомьтесь (/PROCESS STATE CODES) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).

Ответ:

`ps -ao stat,command`
`STAT COMMAND`  
`S+   dbus-run-session -- gnome-session --autostart /usr/share/gdm/greeter/autostart`  
`Sl+  /usr/libexec/gnome-session-binary --systemd --autostart /usr/share/gdm/greeter/autostart`  
`Sl   /usr/libexec/ibus-engine-simple`  
`...`  
`R+   ps -ao stat,command ` 

Наиболее частый статус:   
**S    interruptible sleep (waiting for an event to complete)**   

PROCESS STATE CODES

       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to

       describe the state of a process:



               D    uninterruptible sleep (usually IO)

               I    Idle kernel thread

               R    running or runnable (on run queue)

               S    interruptible sleep (waiting for an event to complete)

               T    stopped by job control signal

               t    stopped by debugger during the tracing

               W    paging (not valid since the 2.6.xx kernel)

               X    dead (should never be seen)

               Z    defunct ("zombie") process, terminated but not reaped by its parent



       For BSD formats and when the stat keyword is used, additional characters may be displayed:



               <    high-priority (not nice to other users)

               N    low-priority (nice to other users)

               L    has pages locked into memory (for real-time and custom IO
