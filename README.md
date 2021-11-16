# Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

1. #### Какого типа команда `cd`? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
   * Команда `cd` является встроенной (shell builtin) проверить это можно через команду `type -t cd`. Команда `cd` является одной из базовых команд и включена в оболочку и может быть вызвана внутри текущего процесса 
2. #### Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`?
   * `grep -c <some_string> <some_file>` 
3. #### Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
   * `/sbin/init` `ps aux |grep 1`
4. #### Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?
   * `ls 2> /dev/pts/2`
5. #### Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
   * cat < peypey.txt > param.txt
6. #### Получится ли находясь в графическом режиме, вывести данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
   * Da `/dev/tty1`
7. #### Выполните команду `bash 5>&1`. К чему она приведет? Что будет, если вы выполните `echo netology > /proc/$$/fd/5`? Почему так происходит?
   * Откроется терминал bash c дополнительным дескриптором 5 который будет использоваться как stdout. Команда `echo netology > /proc/$$/fd/5` отправит строку `netology` по новому дескриптору 5 и выведет эту строку по умолчанию на экран терминала. 
8. #### Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от `|` на stdin команды справа.
Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.
9. Что выведет команда `cat /proc/$$/environ`? Как еще можно получить аналогичный по содержанию вывод?
   * Выводит на экран информацию о переменных окружения пользователя
   * С помощью команды `printenv`
10. Используя `man`, опишите что доступно по адресам `/proc/<PID>/cmdline`, `/proc/<PID>/exe`.
    *        /proc/[pid]/cmdline
              This read-only file holds the complete command line for the process, unless the process is a zombie.  In the latter case, there is  nothing
              in  this  file:  that is, a read on this file will return 0 characters.  The command-line arguments appear in this file as a set of strings
              separated by null bytes ('\0'), with a further null byte after the last string.

              If, after an execve(2), the process modifies its argv strings, those changes will show up here.  This is not the same  thing  as  modifying
              the argv array.

              Furthermore, a process may change the memory location that this file refers via prctl(2) operations such as PR_SET_MM_ARG_START.

              Think of this file as the command line that the process wants you to see.
    *           /proc/[pid]/exe
                Under  Linux  2.2  and later, this file is a symbolic link containing the actual pathname of the executed command.  This symbolic link can be dereferenced normally; attempting to open it will open the executable.  You
                can even type /proc/[pid]/exe to run another copy of the same executable that is being run by process [pid].  If the pathname has been unlinked, the symbolic link will contain the string '(deleted)'  appended  to  the
                original pathname.  In a multithreaded process, the contents of this symbolic link are not available if the main thread has already terminated (typically by calling pthread_exit(3)).

                Permission to dereference or read (readlink(2)) this symbolic link is governed by a ptrace access mode PTRACE_MODE_READ_FSCREDS check; see ptrace(2).

                Under Linux 2.0 and earlier, /proc/[pid]/exe is a pointer to the binary which was executed, and appears as a symbolic link.  A readlink(2) call on this file under Linux 2.0 returns a string in the format:

                    [device]:inode

                For example, [0301]:1502 would be inode 1502 on device major 03 (IDE, MFM, etc. drives) minor 01 (first partition on the first drive).

                find(1) with the -inum option can be used to locate the file.```
11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью `/proc/cpuinfo`.
12. При открытии нового окна терминала и `vagrant ssh` создается новая сессия и выделяется pty. Это можно подтвердить командой `tty`, которая упоминалась в лекции 3.2. Однако:

     ```bash
     vagrant@netology1:~$ ssh localhost 'tty'
     not a tty
     ```

     Почитайте, почему так происходит, и как изменить поведение.
13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись `reptyr`. Например, так можно перенести в `screen` процесс, который вы запустили по ошибке в обычной SSH-сессии.
14. #### `sudo echo string > /root/new_file` не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без `sudo` под вашим пользователем. Для решения данной проблемы можно использовать конструкцию `echo string | sudo tee /root/new_file`. Узнайте что делает команда `tee` и почему в отличие от `sudo echo` команда с `sudo tee` будет работать.
    * tee - read from standard input and write to standard output and files