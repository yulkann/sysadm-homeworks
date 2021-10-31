# Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

1. Какого типа команда `cd`? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
		
		
		vagrant@vagrant:~$ type -a cd
		cd is a shell builtin
		
		` cd- это встроенная в оболочку команда`

1. Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`? `man grep` поможет в ответе на этот вопрос. Ознакомьтесь с [документом](http://www.smallo.ruhr.de/award.html) о других подобных некорректных вариантах использования pipe.

		`grep -c <some_string> <some_file>`
		
1. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?

		
		1 root      20   0  101796  11104   8224 S   0.0   1.1   0:01.75 systemd
		
 
1. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?
		
		
		vagrant@vagrant:~$ ls  /root/ 2> /dev/pts/1
		
		vagrant@vagrant:~$ ls: cannot open directory '/root/': Permission denied
		

1. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
		
		
		vagrant@vagrant:~$ cat /var/log/dmesg | grep Linux | tee loggrep
		[    0.000000] kernel: Linux version 5.4.0-80-generic (buildd@lcy01-amd64-030) (gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)) #90-Ubuntu SMP Fri Jul 9 22:49:44 UTC 2021 (Ubuntu 5.4.0-80.90-generic 5.4.124)
		[    0.291942] kernel: ACPI: Added _OSI(Linux-Dell-Video)
		[    0.291943] kernel: ACPI: Added _OSI(Linux-Lenovo-NV-HDMI-Audio)
		[    0.291944] kernel: ACPI: Added _OSI(Linux-HPI-Hybrid-Graphics)
		[    0.378017] kernel: pps_core: LinuxPPS API ver. 1 registered
		[    1.017298] kernel: Linux agpgart interface v0.103
		[    6.738037] kernel: 23:49:42.175156 main     OS Product: Linux
		vagrant@vagrant:~$ cat loggrep
		[    0.000000] kernel: Linux version 5.4.0-80-generic (buildd@lcy01-amd64-030) (gcc version 9.3.0 (Ubuntu 9.3.0-17ubuntu1~20.04)) #90-Ubuntu SMP Fri Jul 9 22:49:44 UTC 2021 (Ubuntu 5.4.0-80.90-generic 5.4.124)
		[    0.291942] kernel: ACPI: Added _OSI(Linux-Dell-Video)
		[    0.291943] kernel: ACPI: Added _OSI(Linux-Lenovo-NV-HDMI-Audio)
		[    0.291944] kernel: ACPI: Added _OSI(Linux-HPI-Hybrid-Graphics)
		[    0.378017] kernel: pps_core: LinuxPPS API ver. 1 registered
		[    1.017298] kernel: Linux agpgart interface v0.103
		[    6.738037] kernel: 23:49:42.175156 main     OS Product: Linux
		


1. Получится ли вывести находясь в графическом режиме данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?

	![изображение](https://user-images.githubusercontent.com/91043924/139574235-ddab0b3e-aaf7-4195-8858-79c8aff5eb10.png)



1. Выполните команду `bash 5>&1`. К чему она приведет? Что будет, если вы выполните `echo netology > /proc/$$/fd/5`? Почему так происходит?

		создает новый файловый дискриптр 5 и перенаправляет его в  stdout
		vagrant@vagrant:~$ echo netology > /proc/$$/fd/5
		netology
		выводит в консоли дескриптор 5, который  мы перенаправили в stdout

1. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от `|` на stdin команды справа.
Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.

		vagrant@vagrant:~$ ls /root /home /root 4>&2 2>&1 1>&4 | grep Permission -c
		/home:
		vagrant
		2

1. Что выведет команда `cat /proc/$$/environ`? Как еще можно получить аналогичный по содержанию вывод?

		
		vagrant@vagrant:~$ cat /proc/$$/environ
		SHELL=/bin/bashLANGUAGE=en_US:PWD=/home/vagrantLOGNAME=vagrantXDG_SESSION_TYPE=ttyMOTD_SHOWN=pamHOME=/home/vagrantLANG=en_US.UTF-8LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:SSH_CONNECTION=10.0.2.2 56814 10.0.2.15 22LESSCLOSE=/usr/bin/lesspipe %s %sXDG_SESSION_CLASS=userTERM=xterm-256colorLESSOPEN=| /usr/bin/lesspipe %sUSER=vagrantSHLVL=1XDG_SESSION_ID=7XDG_RUNTIME_DIR=/run/user/1000SSH_CLIENT=10.0.2.2 56814 22XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktopPATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/binDBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/busSSH_TTY=/dev/pts/0_=/usr/bin/bashvagrant@vagrant:~$
		
		
		
		vagrant@vagrant:~$ env
		SHELL=/bin/bash
		LANGUAGE=en_US:
		PWD=/home/vagrant
		LOGNAME=vagrant
		XDG_SESSION_TYPE=tty
		MOTD_SHOWN=pam
		HOME=/home/vagrant
		LANG=en_US.UTF-8
		LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:
		SSH_CONNECTION=10.0.2.2 56814 10.0.2.15 22
		LESSCLOSE=/usr/bin/lesspipe %s %s
		XDG_SESSION_CLASS=user
		TERM=xterm-256color
		LESSOPEN=| /usr/bin/lesspipe %s
		USER=vagrant
		SHLVL=2
		XDG_SESSION_ID=7
		XDG_RUNTIME_DIR=/run/user/1000
		SSH_CLIENT=10.0.2.2 56814 22
		XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
		PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
		DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
		SSH_TTY=/dev/pts/0
		_=/usr/bin/env
		


1. Используя `man`, опишите что доступно по адресам `/proc/<PID>/cmdline`, `/proc/<PID>/exe`.

		       /proc/[pid]/cmdline
		      This read-only file holds the complete command line for the process, unless the process is a zombie.  In the latter case, there is nothing in this file: that is, a read on  this  file  will return 0 characters.  The command-line arguments appear in this file as a set of strings separated by null bytes ('\0'), with a further null byte after the last string.
			Manual page proc(5) line 178
			
			/proc/[pid]/exe
              		Under  Linux 2.2 and later, this file is a symbolic link containing the actual pathname of the executed command.
			Manual page proc(5) line 219
			
1. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью `/proc/cpuinfo`.
			
			
			cat /proc/cpuinfo | grep sse
			sse4a 
			

1. При открытии нового окна терминала и `vagrant ssh` создается новая сессия и выделяется pty. Это можно подтвердить командой `tty`, которая упоминалась в лекции 3.2. Однако:

    ```bash
	vagrant@netology1:~$ ssh localhost 'tty'
	not a tty
    ```

	Почитайте, почему так происходит, и как изменить поведение.
	
	`ssh Host command для запуска команды на удаленном хосте, команда не запускается внутри терминала.`
	
1. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись `reptyr`. Например, так можно перенести в `screen` процесс, который вы запустили по ошибке в обычной SSH-сессии.

		reptyr -T [PID]
		
1. `sudo echo string > /root/new_file` не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без `sudo` под вашим пользователем. Для решения данной проблемы можно использовать конструкцию `echo string | sudo tee /root/new_file`. Узнайте что делает команда `tee` и почему в отличие от `sudo echo` команда с `sudo tee` будет работать.
	
		`sudo не выполняет перенаправление вывода, это произойдет как непривилегированный пользователь.
		tee получит вывод команды echo, повысит права на sudo и запишет в файл.`
