### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-03-yaml/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
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
  Нужно найти и исправить все ошибки, которые допускает наш сервис
  
Ответ:  
```
    { "info" : "Sample JSON output from our service\\t",
        "elements" :[
            {
			  "name": "first",
              "type": "server",
              "ip": 7175 
            },
            { 
			  "name": "second",
              "type": "proxy",
              "ip": "71.78.22.43"
            }
        ]
    }
```

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
import socket
import time
import json
import yaml

hosts = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}
while 1 == 1 :
  for host in hosts :
    ip = socket.gethostbyname(host)
    if ip != hosts[host] :
      print(' [ERROR] ' + str(host) +' IP mistmatch: '+hosts[host]+' '+ip)
      hosts[host]=ip
    else :
        print(str(host) + ' ' + ip)
    with open('hosts1.json', 'w') as json_file:
        json_file.write(json.dumps(hosts, indent=2))
    with open('hosts1.yaml', 'w') as yaml_file:
        yaml_file.write(yaml.dump(hosts, indent=2, explicit_start=True, explicit_end=True))
    time.sleep(2)
```

### Вывод скрипта при запуске при тестировании:
```
user@ubuntu-me:~/test$ python3 test4.py
 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 64.233.165.194
 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 142.251.1.17
 [ERROR] google.com IP mistmatch: 0.0.0.0 216.58.210.174
drive.google.com 64.233.165.194
mail.google.com 142.251.1.17
google.com 216.58.210.174
drive.google.com 64.233.165.194
mail.google.com 142.251.1.17
google.com 216.58.210.174
drive.google.com 64.233.165.194
mail.google.com 142.251.1.17
google.com 216.58.210.174
drive.google.com 64.233.165.194
mail.google.com 142.251.1.17
google.com 216.58.210.174
drive.google.com 64.233.165.194
mail.google.com 142.251.1.17
google.com 216.58.210.174
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{
  "drive.google.com": "64.233.165.194",
  "mail.google.com": "142.251.1.17",
  "google.com": "216.58.210.174"
}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
---
drive.google.com: 64.233.165.194
google.com: 216.58.210.174
mail.google.com: 142.251.1.17
...
```
