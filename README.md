# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1. Какой системный вызов делает команда `cd`? 
   ```bash
   vagrant@vagrant:~$ strace /bin/bash -c 'cd /tmp' 2>&1 | grep cd
   execve("/bin/bash", ["/bin/bash", "-c", "cd /tmp"], 0x7ffd971a0b40 /* 24 vars */) = 0
   ```
2. Используя `strace` выясните, где находится база данных `file` на основании которой она делает свои догадки.
   * `openat(AT_FDCWD, "/usr/share/misc/magic.mgc", O_RDONLY) = 3`
3. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
   * `lsof -nP | grep '(deleted)' #получить pid процесса` 
   * `cat /proc/$pid/fd/$fd > /dev/null/delete_file`
4. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
   * Зомби-процессы не занимают ресурсы ОС, но блокируют записи в таблице процессов.
5. На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? :
   ```bash
   vagrant@vagrant:~$ sudo opensnoop-bpfcc
   PID    COMM               FD ERR PATH
   780    vminfo              4   0 /var/run/utmp
   565    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
   565    dbus-daemon        18   0 /usr/share/dbus-1/system-services
   565    dbus-daemon        -1   2 /lib/dbus-1/system-services
   565    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
    ```
6. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.
   * `execve("/usr/bin/uname", ["uname", "-a"], 0x7fff6c729008 /* 24 vars */) = 0`
   * `Part of the utsname information is also accessible  via  /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
`
7. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
   Есть ли смысл использовать в bash `&&`, если применить `set -e`?
   * В первом случае команды выполняются последовательно, не зависимо от кода выхода
   * В последнем случае вторая команда исполняется только тогда, когда выход из первой команды равен нулю(успешное исполнение команды)
   * Нет смысла, команды равноценны.
8. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?
   * -e Выйти немедленно если команда не завершается успешно.
   * -u Расценивать не объявленные переменные как ошибку
   * -x Выводить команды и их аргументы в момент их выполнения
   * -o pipefail Выводит команду которая выполнилась с ошибкой в пайплайне (с не нулевым кодом выхода)
   * Данная конструкция может помочь в дебаге скриптов и предотвращает дальнейшее неправильное исполнение скрипта в случае ошибки на одном из этапов.
9. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).
   ```bash
   vagrant@vagrant:~$ ps -axo stat | sort -n | uniq -c | sort -n
      1 R+
      1 Sl
      1 SLsl
      1 S<s
      1 STAT
      2 SN
      4 S+
      4 Ssl
      8 I
     15 Ss
     24 S
     39 I<
   ```
   * S=52 Итого самым распространенным являются спящие процессы, которые могут быть вызваны из сна.
