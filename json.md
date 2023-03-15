# Домашнее задание к занятию «Языки разметки JSON и YAML»

## Задание 1

Мы выгрузили JSON, который получили через API-запрос к нашему сервису:

```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис.

### Ваш скрипт:

```
{ "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"    #Не хватало кавычек
            }
        ]
    }
```
### Результат:
```
dict_items([('info', 'Sample JSON output from our service\t'), ('elements', [{'name': 'first', 'type': 'server', 'ip': 7175}, {'name': 'second', 'type': 'proxy', 'ip': '71.78.22.43'}])])
```
---

## Задание 2

В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML-файлов, описывающих наши сервисы. 

Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. 

Формат записи YAML по одному сервису: `- имя сервиса: его IP`. 

Если в момент исполнения скрипта меняется IP у сервиса — он должен так же поменяться в YAML и JSON-файле.

### Ваш скрипт:

```python
#! /usr/bin/env python3
import socket
import os
import ast
import json
import yaml

nodes = ["drive.google.com", "mail.google.com", "google.com"]

# Просмотрим содержимое логов
logs = ['hosts.json', 'hosts.yml']
for file in logs:
    # Cоздаем файл, если его нет
    input_file = open(file, 'a+')
    input_file.close()
    input_file = open(file, 'r')

    # Читаем JSON
    if file == 'hosts.json':
        json_text = input_file.read()
        if len(json_text) != 0:
            log_from_json = json.loads(json_text)
            print(f'Найден предыдущий лог JSON: {log_from_json.items()}')
            input_file.close()
        else:
            # Если предыдущего лога нет, заполним словарь нулями
            log_from_json = {}
            n = 0
            while n <= (len(nodes) - 1):
                log_from_json[nodes[n]] = socket.gethostbyname(nodes[n])
                log_from_json[nodes[n]] = "0.0.0.0"
                n += 1
            print(f'Предыдущий лог JSON пуст. Создан новый словарь: {log_from_json.items()}')

    # Читаем YAML
    elif file == 'hosts.yml':
        yaml_text = input_file.read()
        if len(yaml_text) != 0:
            log_from_yaml = yaml.safe_load(yaml_text)
            print(f'Найден предыдущий лог YAML: {log_from_yaml.items()}')
            input_file.close()
        else:
            # Если предыдущего лога нет, заполним словарь нулями
            log_from_yaml = {}
            n = 0
            while n <= (len(nodes) - 1):
                log_from_yaml[nodes[n]] = socket.gethostbyname(nodes[n])
                log_from_yaml[nodes[n]] = "0.0.0.0"
                n += 1
            print(f'Предыдущий лог YAML пуст. Создан новый словарь: {log_from_yaml.items()}')

# Просмотрим новые данные, запишем в лог
n = 0
new_log = {}
while n <= (len(nodes) - 1):
    new_log[nodes[n]] = socket.gethostbyname(nodes[n])
    if new_log[nodes[n]] == log_from_json[nodes[n]]:
        print(f'Узел {nodes[n]}: прошлый IP {log_from_json[nodes[n]]}, новый IP {new_log[nodes[n]]}. Изменений не было.')
    else:
        print(f'Узел {nodes[n]} сменил IP с {log_from_json[nodes[n]]} на {new_log[nodes[n]]}')
    n += 1
with open('hosts.json', 'w') as hosts_json:
    json.dump(new_log, hosts_json, indent=2)
with open('hosts.yml', 'w') as hosts_yaml:
    yaml.dump(new_log, hosts_yaml, sort_keys=False, indent=2)

```

### Вывод скрипта при запуске во время тестирования:

```bash
$ python3 2.py
Найден предыдущий лог JSON: dict_items([('drive.google.com', '108.177.14.194'), ('mail.google.com', '64.233.165.83'), ('google.com', '74.125.205.113')])
Найден предыдущий лог YAML: dict_items([('drive.google.com', '108.177.14.194'), ('mail.google.com', '64.233.165.83'), ('google.com', '74.125.205.113')])
Узел drive.google.com: прошлый IP 108.177.14.194, новый IP 108.177.14.194. Изменений не было.
Узел mail.google.com сменил IP с 64.233.165.83 на 209.85.233.18
Узел google.com сменил IP с 74.125.205.113 на 173.194.222.100
```

### JSON-файл(ы), который(е) записал ваш скрипт:

```json
{
  "drive.google.com": "108.177.14.194",
  "mail.google.com": "209.85.233.18",
  "google.com": "173.194.222.100"
} 
```

### YAML-файл(ы), который(е) записал ваш скрипт:

```yaml
drive.google.com: 108.177.14.194
mail.google.com: 209.85.233.18
google.com: 173.194.222.100
```