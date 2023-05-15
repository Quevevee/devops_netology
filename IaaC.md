# Домашнее задание к занятию 2. "Применение принципов IaaC в работе с виртуальными машинами"
## Задача 1. 

 - Опишите основные преимущества применений на практике IaaC-паттернов.

Основные преимущества IaaC - ускорение производства и вывода продукта на рынок, улучшение стабильности сред разработки, более быстрая и эффективная разработка за счет автоматизации отката на стабильные версии, в случае обнаружения ошибок и легкий поиск ошибок.

 - Какой из принципов IaaC является основополагающим?

Основопологающий принцип IaaC - идемпотентность. Идемпотентность - свойство операции, при повторном выполнении которой мы получаем идентичный предыдущему и всем последующим выполнениям.

## Задача 2.

- Чем Ansible выгодно отличается от других систем управления конфигурациями?

Ansible можно использовать без установки специального ПО на целевом хосте, он работает по SSH, так же он может оповестить на сервер о неудачной доставке конфигурации.

- Какой, на ваш взгляд, метод работы систем конфигурации более надежный - push или pull?

На мой взгляд более надежный push, т.к. управляющий сервер отправляет конфигурацию, а его проще мониторить на ошибки.

## Задача 3.

Установите на личный компьютер:
- VirtualBox,
- Vagrant,
- Terraform,
- Ansible.

Приложите выывод команд установленных версий каждой из программ, оформленный в Markdown.

```bash
$ vboxmanage --version                            

7.0.6_Debianr155176
                                                                                                                                              
$ vagrant -v          
Vagrant 2.3.4
                                                                                                                                              
$ terraform -version 
Terraform v1.4.5
on linux_amd64
                                                                                                                                              
$ ansible --version  
ansible [core 2.14.3]
  config file = None
  configured module search path = ['/home/user/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/user/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
```

## Задача 4.

Воспроизведите практическую часть лекции самостоятельно.

- Создайте виртуальную машину
- Зайдите внутрь ВМ, убедитесь, что Docker установлен с помощью команды
```docker ps,```
``` bash
$ vagrant status
Current machine states:

server1.netology          running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.

$ vagrant ssh   
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.4.0-144-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue 18 Apr 2023 03:46:07 PM UTC

  System load:  0.76               Processes:             148
  Usage of /:   11.5% of 30.34GB   Users logged in:       0
  Memory usage: 21%                IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%                 IPv4 address for eth1: 192.168.56.11


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento

vagrant@server1:~$ docker ps
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied
```