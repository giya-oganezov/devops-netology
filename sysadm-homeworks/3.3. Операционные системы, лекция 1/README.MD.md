# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1.  Какой системный вызов делает команда `cd`? В прошлом ДЗ мы выяснили, что `cd` не является самостоятельной программой, это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Тем не менее, вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае вы увидите полный список системных вызовов, которые делает сам `bash`при старте. Вам нужно найти тот единственный, который относится именно к `cd`. Обратите внимание, что `strace` выдаёт результат своей работы в поток stderr, а не в stdout.

	Ответ: 

		chdir("/tmp")

2.  Попробуйте использовать команду `file` на объекты разных типов на файловой системе. Например:
    
    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    
    Используя `strace` выясните, где находится база данных `file` на основании которой она делает свои догадки.
	
	Ответ:
	       
		   "/etc/magic"
    
3.  Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).

	Ответ:
	
		 команда сat /dev/null > test.sh
	
		
1.  Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?

		Зомби не занимают памяти но блокируют записи в таблице процессов, размер которой ограничен для каждого пользователя и системы в целом.

3.  В iovisor BCC есть утилита `opensnoop`:
    
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools`для Ubuntu 20.04. Дополнительные [сведения по установке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).
	
		vagrant@myhost:~$ sudo opensnoop-bpfcc -d 1
		PID    COMM               FD ERR PATH
		636    PLUGIN[proc]       22   0 /sys/class/power_supply
		636    PLUGIN[cgroups]    19   0 /sys/fs/cgroup/cpu,cpuacct/system.slice/irqbalance.service/cpuacct.stat
		636    PLUGIN[cgroups]    18   0 /sys/fs/cgroup/memory/system.slice/irqbalance.service/memory.stat
		636    PLUGIN[cgroups]    22   0 /sys/fs/cgroup/memory/system.slice/irqbalance.service/memory.usage_in_bytes
		636    PLUGIN[cgroups]    22   0 /sys/fs/cgroup/memory/system.slice/irqbalance.service/memory.failcnt
		636    PLUGIN[cgroups]    21   0 /sys/fs/cgroup/blkio/system.slice/irqbalance.service/blkio.throttle.io_service_bytes
		636    PLUGIN[cgroups]    21   0 /sys/fs/cgroup/blkio/system.slice/irqbalance.service/blkio.throttle.io_serviced
		636    PLUGIN[cgroups]    19   0 /sys/fs/cgroup/cpu,cpuacct/system.slice/vboxadd-service.service/cpuacct.stat
		636    PLUGIN[cgroups]    18   0 /sys/fs/cgroup/memory/system.slice/vboxadd-service.service/memory.stat
		636    PLUGIN[cgroups]    22   0 /sys/fs/cgroup/memory/system.slice/vboxadd-service.service/memory.usage_in_bytes
		636    PLUGIN[cgroups]    22   0 /sys/fs/cgroup/memory/system.slice/vboxadd-service.service/memory.failcnt
		636    PLUGIN[cgroups]    21   0 /sys/fs/cgroup/blkio/system.slice/vboxadd-service.service/blkio.throttle.io_service_bytes
		636    PLUGIN[cgroups]    21   0 /sys/fs/cgroup/blkio/system.slice/vboxadd-service.service/blkio.throttle.io_serviced
		636    PLUGIN[cgroups]    19   0 /sys/fs/cgroup/cpu,cpuacct/system.slice/systemd-networkd.service/cpuacct.stat
    
6.  Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.
7.  Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    
		Ответ:
	   	Команды, разделенные ; выполняются последовательно; оболочка ожидает завершения каждой команды по очереди. Статус возврата это статус выхода последней выполненной команды.
		- в случае использования конструкции command1 && command2 выпвыполение command2 произойдет если, command1 завершиться успехом.
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?
      
	  	Ответ: 
		В отличии от `&&`, при использовании `set -e` скрипт немедленно завершит работу, если любая команда выйдет с ошибкой
	
		
1.  Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?

1.  Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps`ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).