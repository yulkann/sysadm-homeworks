# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

1. Установите средство виртуализации [Oracle VirtualBox](https://www.virtualbox.org/). `Установлено`

1. Установите средство автоматизации [Hashicorp Vagrant](https://www.vagrantup.com/). `Установлено`

1. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. `+`

1. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant `+`

1. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию?
	`1 Гб оперативной памяти, 2 процессора. 4 мб видеопамяти`


1. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: [документация](https://www.vagrantup.com/docs/providers/virtualbox/configuration.html). Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
 	```bash
 	Vagrant.configure("2") do |config|
  	      config.vm.box = "bento/ubuntu-20.04"
  	      config.vm.provider "virtualbox" do |v|
 	         v.memory = 2048
	         v.cpus = 4
		 v.name = "Netology-Devops"
	        end
	end
	```

1. Команда `vagrant ssh` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
		```
		
		Last login: Tue Oct 26 09:00:17 2021 from 10.0.2.2
		 vagrant@vagrant:~$
		 vagrant@vagrant:~$ whoami
			vagrant
			vagrant@vagrant:~$ echo Hello world
			Hello world
			vagrant@vagrant:~$ echo $USER
			vagrant
			vagrant@vagrant:~$ $SHELL --version
			GNU bash, version 5.0.17(1)-release (x86_64-pc-linux-gnu)
			Copyright (C) 2019 Free Software Foundation, Inc.
			License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

			This is free software; you are free to change and redistribute it.
			There is NO WARRANTY, to the extent permitted by law.
			vagrant@vagrant:~$ new_variable=new_text
			vagrant@vagrant:~$ ll
			total 40
			drwxr-xr-x 4 vagrant vagrant 4096 Oct 26 09:15 ./
			drwxr-xr-x 3 root    root    4096 Jul 28 17:50 ../
			-rw-r--r-- 1 vagrant vagrant  220 Jul 28 17:50 .bash_logout
			-rw-r--r-- 1 vagrant vagrant 3771 Jul 28 17:50 .bashrc
			drwx------ 2 vagrant vagrant 4096 Jul 28 17:51 .cache/
			-rw------- 1 vagrant vagrant  114 Oct 26 09:15 .lesshst
			-rw-r--r-- 1 vagrant vagrant  807 Jul 28 17:50 .profile
			drwx------ 2 vagrant root    4096 Oct 25 18:04 .ssh/
			-rw-r--r-- 1 vagrant vagrant    0 Jul 28 17:51 .sudo_as_admin_successful
			-rw-r--r-- 1 vagrant vagrant    6 Jul 28 17:51 .vbox_version
			-rw-r--r-- 1 root    root     180 Jul 28 17:55 .wget-hsts
			vagrant@vagrant:~$ history
			    1  whoami
			    2  echo Hello world
			    3  echo $USER
			    4  $SHELL --version
			    5  new_variable=new_text
			    6  ll
			    7  history
			vagrant@vagrant:~$ pwd
			/home/vagrant
			vagrant@vagrant:~$ alias
			alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
			alias egrep='egrep --color=auto'
			alias fgrep='fgrep --color=auto'
			alias grep='grep --color=auto'
			alias l='ls -CF'
			alias la='ls -A'
			alias ll='ls -alF'
			alias ls='ls --color=auto'
			vagrant@vagrant:~$ top
			top - 10:13:03 up 19 min,  1 user,  load average: 0.00, 0.00, 0.00
			Tasks:  99 total,   1 running,  98 sleeping,   0 stopped,   0 zombie
			%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
			MiB Mem :    981.0 total,    708.8 free,     89.9 used,    182.3 buff/cache
			MiB Swap:    980.0 total,    980.0 free,      0.0 used.    744.9 avail Mem

			    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
			    524 root      rt   0  280200  17992   8200 S   0.7   1.8   0:00.20 multipathd
			   1002 root      20   0       0      0      0 I   0.3   0.0   0:00.04 kworker/0:0-events
			      1 root      20   0  101808  11136   8304 S   0.0   1.1   0:00.89 systemd
			      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
			      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
			      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp
			      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-kblockd
			      8 root      20   0       0      0      0 I   0.0   0.0   0:00.15 kworker/u4:0-events_power_efficient
			      9 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq
			     10 root      20   0       0      0      0 S   0.0   0.0   0:00.01 ksoftirqd/0
			     11 root      20   0       0      0      0 I   0.0   0.0   0:00.04 rcu_sched
			     12 root      rt   0       0      0      0 S   0.0   0.0   0:00.00 migration/0
			     13 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0
			     14 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0
			     15 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1
			     16 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/1
			     17 root      rt   0       0      0      0 S   0.0   0.0   0:00.20 migration/1
			     18 root      20   0       0      0      0 S   0.0   0.0   0:00.03 ksoftirqd/1
			     20 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/1:0H-kblockd
			     21 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kdevtmpfs
			     22 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 netns
			     23 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tasks_kthre
			     24 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kauditd
			     26 root      20   0       0      0      0 S   0.0   0.0   0:00.00 khungtaskd
			     27 root      20   0       0      0      0 S   0.0   0.0   0:00.00 oom_reaper
			     28 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 writeback
			     29 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kcompactd0
			     30 root      25   5       0      0      0 S   0.0   0.0   0:00.00 ksmd
			     31 root      39  19       0      0      0 S   0.0   0.0   0:00.00 khugepaged
			     77 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kintegrityd
			     78 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kblockd
			     79 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 blkcg_punt_bio
			     80 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 tpm_dev_wq
			     81 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 ata_sff
			     82 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 md
			     83 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 edac-poller
			     84 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 devfreq_wq
			     85 root      rt   0       0      0      0 S   0.0   0.0   0:00.00 watchdogd
			     86 root      20   0       0      0      0 I   0.0   0.0   0:00.18 kworker/u4:1-events_power_efficient
			     88 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kswapd0
			     89 root      20   0       0      0      0 S   0.0   0.0   0:00.00 ecryptfs-kthrea
			     91 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kthrotld
			     92 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 acpi_thermal_pm
			     93 root      20   0       0      0      0 S   0.0   0.0   0:00.02 scsi_eh_0
			     94 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 scsi_tmf_0
			vagrant@vagrant:~$
			
1. Ознакомиться с разделами `man bash`, почитать о настройках самого bash:
	* какой переменной можно задать длину журнала `history`, и на какой строчке manual это описывается?

	```bash
	HISTFILESIZE
	Manual page bash(1) line 586
	```
	
	* что делает директива `ignoreboth` в bash?
    
	```bash
	A value of ignoreboth is shorthand for ignorespace and ignoredups.-
	HISTCONTROL
  	  Список значений, разделенных двоеточиями, управляющий тем, как команды сохраняются в списке истории. 
	  Если список значений включает в себя ignorespace, строки, начинающиеся с пробела, не сохраняются в списке истории. 
	  Значение ignoredupsприводит к тому, что строки, соответствующие предыдущей записи истории, не будут сохранены. 
	  Значение ignorebothявляется сокращением для ignorespace и ignoredups.
	  ```
 
1. В каких сценариях использования применимы скобки `{}` и на какой строчке `man bash` это описано?

	```bash
	{ list; }
		list  is  simply executed in the current shell environment.  list must be terminated with a newline or semicolon.  
		This is known as a group command. The return status is the exit status of list.  
		Note that unlike the metacharacters ( and ), { and } are reserved words and must occur where a reserved word is permitted to be recognized.
		Since they do not  cause  a  word  break, they must be separated from list by whitespace or another shell metacharacter.
		Manual page bash(1) line 206
	```

1. Основываясь на предыдущем вопросе, как создать однократным вызовом `touch` 100000 файлов? А получилось ли создать 300000? Если нет, то почему?
	
	```bash
	vagrant@vagrant:~$ touch new-file-{1..10000}.txt
	vagrant@vagrant:~$ ls | wc -l
	10000
	vagrant@vagrant:~$ touch new-file-{1..300000}.txt
	-bash: /usr/bin/touch: Argument list too long
	vagrant@vagrant:~$ ls | wc -l
	10000
	```
	
1. В man bash поищите по `/\[\[`. Что делает конструкция `[[ -d /tmp ]]` 
	
	```bash
	[[ expression ]]
		Return a status of 0 or 1 depending on the evaluation of the conditional expression expression.  
		Expressions are composed of the primaries described  below  under  CONDITIONAL  EXPRESSIONS.
		Word  splitting  and  pathname expansion are not performed on the words between the [[ and ]]; 
		tilde expansion, parameter and variable expansion, arithmetic expansion, command substitution, process substitution, and quote removal are performed
		Conditional operators such as -f must be unquoted to be recognized as primaries.`
	  ```
	  
	  `[[ -d /tmp ]]` - истина если /tmp существует и является каталогом. `
	      
1. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

	```bash
	bash is /tmp/new_path_directory/bash
	bash is /usr/local/bin/bash
	bash is /bin/bash
	```
	(прочие строки могут отличаться содержимым и порядком)
    В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.
		
	```
	vagrant@vagrant:~$ mkdir /tmp/new_path_directory
	vagrant@vagrant:~$ cp /bin/bash /tmp/new_path_directory/
	vagrant@vagrant:~$ PATH=/tmp/new_path_directory/:$PATH
	vagrant@vagrant:~$ type -a bash
	bash is /tmp/new_path_directory/bash
	bash is /usr/bin/bash
	bash is /bin/bash
	```
	
	
1. Чем отличается планирование команд с помощью `batch` и `at`?

	команда at используется для назначения одноразового задания на заданное время, а команда batch — для назначения одноразовых задач, которые должны выполняться, когда загрузка системы становится меньше 0,8. 

1. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука. `+`

 	```
	PS D:\DevOps\VM> vagrant halt
	==> default: Discarding saved state of VM...
	PS D:\DevOps\VM> vagrant status
	Current machine states:

	default                   poweroff (virtualbox)

	The VM is powered off. To restart the VM, simply run `vagrant up`
	```


---
