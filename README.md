# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

1. ### Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
   * CPU 2 потока
   * RAM 2 Гб
   * Диск системы 64 Гб формата .VMDK, методика выделения пространства Thin provisioning
   * Сетевой адаптер подключенный через NAT 
2. ### Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
   * VM->Настроить->Система->Материнская плата->Основная память
   * VM->Настроить->Система->Процессор->Процессор(ы)
3. ### Ознакомиться с разделами `man bash`, почитать о настройках самого bash:
    * какой переменной можно задать длину журнала `history`, и на какой строчке manual это описывается? `HISTSIZE` `630`
    * что делает директива `ignoreboth` в bash? позволяет для опции HISTCONTROL отключить не отбражать в истории команды начинающиеся с пробела(пустые строки) и команды совпадающие с последней выполненной командой 
4. ### В каких сценариях использования применимы скобки `{}` и на какой строчке `man bash` это описано? 
    * 206 То, что находится между фигурными скобками — выполняется в контексте текущей оболочки.
5. ### Основываясь на предыдущем вопросе, как создать однократным вызовом `touch` 100000 файлов? А получилось ли создать 300000? Если нет, то почему?
    * `touch test{000001..100000}` 
    * нет `Argument list too long` indicates when a user feeds too many arguments into a single command which hits the ARG_MAX limit. The ARG_MAX defines the maximum length of arguments to the exec function. An argument, also called a command-line argument, can be defined as the input given to a command, to help control that command line process. Arguments are entered into the terminal or console after typing the command. Multiple arguments can be used together; they will be processed in the order they typed, left to right. This limit for the length of a command is imposed by the operating system. You can check the limit for maximum arguments on your Linux system using this command: `getconf ARG_MAX`
6. ### В man bash поищите по `/\[\[`. Что делает конструкция `[[ -d /tmp ]]` ?
    * Выводит значение `True` если `/tmp` существует и это директория 
7. ### Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

    ```bash
    bash is /tmp/new_path_directory/bash
    bash is /usr/local/bin/bash
    bash is /bin/bash
    ```

    (прочие строки могут отличаться содержимым и порядком)
    В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

8. Чем отличается планирование команд с помощью `batch` и `at`?
