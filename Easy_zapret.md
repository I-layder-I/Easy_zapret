# Самый простой гайд на zapret от bol-van для арчеводов
### Все команды выполняем под
```
sudo -s
```
Иначе придётся перед каждой писать `sudo` (для выхода набрать `exit`)

Также может убряться каталог пользователя то есть заместо `layder` в данном случае моё имя, пользователя будет `~` и чтобы вернуться в домашнюю папку пишем
```
cd /home/layder
```
`layder` заменить на ваше имя пользователья
## Установка зависимостей
`pacman -S --needed curl ipset bind git jq` строчка для арча и производных

`dnf install -y curl ipset dnsutils git jq` строчка для федоры

`apt install -y curl ipset dnsutils git jq` строчка для убунты

`pacman -S nftables` установка nftables
### Установка самого запрета
Скачать последний релиз .tar.gz с [тык](https://github.com/bol-van/zapret/releases)

Распаковываем

Для этого понадобиться утилита tar и gzip

```
pacman -S tar gzip
```

Далее переходим в папку загрузки и распаковываем
```
cd Загрузки
tar -xvzf zapret-v69.5.tar.gz -C /home
```
Где `zapret-v69.5.tar.gz` имя архива, на данный момент оно такое и `/home/layder` - куда файл распаковывается
## Сама программа
Для начала переходим в папку с распакованной программой в моём случае это `/home/layder/zapret-v69.5`
```
cd /home/layder/zapret-69.5
```
Запускаем
```
./install_bin.sh
./install_prereq.sh
```
### Далее настройка и применение блокчека 
Блокчек (`blockcheck`) - скрипт проверяющий и ищущий подходящую стратегию для обхода, подробнее > [оригиналь](https://github.com/bol-van/zapret/tree/master)

Изначально он берёт стратегию и проверяет адрес на отзыв, если он отзывается, то стратегия записывается в рабочие и выводиться в конечном листе, если же стратегия не рабочая, то она пропускается, но при `times to repeat each test` то есть количество проверок тобиш пингов больше одного, что рекомендуется из-за возможного ложного срабатывания и записи стратегии в рабочие, он пингует всегда 3 раза, тоесть если пинга 3 то проверок будет в любом случае 3, если 10, то 10, и даже если стратегия не рабочая он всё равно проверит нужное количество раз, а это занимает не мало времени с учётом скорости одного отклика адреса ~2 секунды(у меня), сами понимаете.

Так к чему же это
### КАК УСКОРИТЬ БЛОКЧЕК И НАЙТИ МАКСИМУМ РАБОЧИХ СТРАТЕГИЙ ОБХОДА
1. Открываете blockcheck.sh в текстовом редакторе, у меня на арче `kate`
2. В поиске ищете строчку вида `[ "$SCANLEVEL" = quick ] && break`
3. Заменяете её на просто `break`
4. Сохраняете и все вы ускорили блокчек.
С помощью этого действия теперь он будет при первой недоступности стратегии сразу переходить к следующей.

#### Если заблокирован хост видео - `googlevideo.com`
Адрес кеширующего сервера вы будете вставлять в блокчек чтобы найти способ обхода блокировки именно ютуба а не рутрекера (по умолчанию в скрипте блокчека стоит именно адрес рутрекера)
1. Переходим на сайт ютуба
2. Тыкаем правой кнопкой мыши в любом пустом сером месте
3. В открывшемся меню нажимаем `Инспектировать` или `Посмотреть код` или `Инструменты разработчика -> Инспектировать`
4. Выбираем пункт `Network`
> [!NOTE]
> Если его нет расширяем открывшуюся консоль, потянув её влево за край
5. В строке `Filter` пишем `googlevideo`
6. В столбце `Name` выбираем любой пункт
7. Копируем всё между `https://` и `/videoplayback?expire=1733589557&ei=1SVUZ4ebL6jM...`(может быть ещё дофигилион текста)
8. Будет вида `rr4---sn-8ph2xajvh-n8vs.googlevideo.com`, у вас скорее всего другое
![Пример](https://github.com/user-attachments/assets/20d6dec8-3db0-4847-96d8-cd4d26dcb59f)


Запускаем
```
./blockcheck.sh
```
В поле для адреса `domain(s) (default: rutracker.org) :` вписываем наше `rr4---sn-8ph2xajvh-n8vs.googlevideo.com`, далее в полях:
1. `ip protocol version(s)` : жмём `4`
2. `check http` : `n`
3. `check https tls 1.2` : `y` или ентер
4. `check https tls 1.3` : `n`
5. `check http3 QUIC` : `n`
6. `times to repeat each test` : `3`
7. `your choice` : `standart` или ентер

Далее берём соответственно нашу стратегию
>* SUMMARY
>  
>ipv4 rr4---sn-8ph2xajvh-n8vs.googlevideo.com curl_test_https_tls12 : nfqws --dpi-desync=split2
1. Где `ipv4` режим сети, может также быть `ipv6`, но нам надо именно `ipv4`
2. `rr4---sn-8ph2xajvh-n8vs.googlevideo.com` адрес, вписанный раннее
3. `curl_test_https_tls12` режим проверки, выбранный до этого, их можетбыть несколько, нам нужен этот
4. `nfqws` об этом далее
5. И наконец сама стратегия(у вас скорее всего будет другая) `--dpi-desync=split2`
#### Если заблокирован сам сайт ютуба
Лучше пропускать и идти к следующему шагу, если же он не помог, то

Запускаем
```
./blockcheck.sh
```
В поле для адреса `domain(s) (default: rutracker.org) :` вписываем `youtube.com`, далее в полях:
1. `ip protocol version(s)` : жмём `4`
2. `check http` : `n`
3. `check https tls 1.2` : `y` или ентер
4. `check https tls 1.3` : `n`
5. `check http3 QUIC` : `n`
6. `times to repeat each test` : `3`
7. `your choice` : `standart` или ентер

Ждёмс
> [!IMPORTANT]
> Если же написало youtube.com Works without dpi или подобное то ютуб работает без дпи, тоесть он всё таки не нужен, пропускаем этот шаг.

## Собственно установка
Пишем
```
./install_easy.sh
```
> [!NOTE]
> Напоминаю, всё это выполняем под `sudo -s` и в `cd /home/layder/zapret-v69.5` илиже в вашем местонахождении папки с запретом.

1.
```
easy install is supported only from default location : /opt/zapret
currently its run from /home/layder/zapret-v69.5
do you want the installer to copy it for you (default : N) (Y/N) ?
```
Тут вводим `y` и жмём ентер, если кратко то спрашивают хотим ли мы создать папку с запретом в директорию по умолчанию, то есть `/opt/zapret`
2. `select firewall type :` тут `2` тобиш nftables
3. `select firewall type :` - `n`
4. `select flow offloading :` - может и не встретиться, тут `1` - none
5. `enable tpws socks mode on port 987 ?` - `n`
6. `enable tpws transparent mode ?` - `n`
7. `enable nfqws ?` тут наконец да - `y`
8. `do you want to edit the options` - `y`

Откроется редактор кода в терминале, будет примерно так
![Пример_2](https://github.com/user-attachments/assets/ae3c89b3-b2b5-433f-b841-a2ea86fd6fd0)
Если в поле `NFQWS_OPT=""` в кавычках (для бибизян это `""`) будет что-то, то можете сразу удалять

Рассмотрим строчку
```
--filter-tcp=443 --dpi-desync=fake,fakedsplit --dpi-desync-fooling=badseq --dpi-desync-split-pos=midsld --dpi-desync-fake-tls=0x00000000 --hostlist=/opt/zapret/lists/googlevideo.txt --new
```
1) `--filter-tcp=443` - параметр, для домохозяек - менять не потребуется
2) `--dpi-desync=fake,fakedsplit --dpi-desync-fooling=badseq --dpi-desync-split-pos=midsld --dpi-desync-fake-tls=0x00000000` - сама наша стратегия, может быть и `--dpi-desync=fake,fakedsplit`. и `--dpi-desync-fake-tls=0x00000000`, и длиннее, и намного короче, в зависимости от сложности обхода
3) `--hostlist=/opt/zapret/lists/googlevideo.txt` - об этом далее
4) `--new` - пишется после каждой строчки, кроме последней, если же она одна, то параметр не пишется
Итак в кавычки
![Пример_3](https://github.com/user-attachments/assets/5d454499-75e5-4491-8525-fe8326bbd8bf)
вставляем
`--filter-tcp=443` пробел
наша стратегия `--dpi-desync=split2` пробел,

и тот самый хост лист `--hostlist=/opt/zapret/lists/googlevideo.txt`.

Тут поподробнее, это параметр, указывающий на то, к каким адресам будет применяться страта, рассмотрим:
1) `--hostlist=` - сам параметр
2) `/opt/zapret/lists/googlevideo.txt` путь до файла с адресами
##### Настройка `lists`
Можем открыть новую вкладку в терминале или сделать перед этим.

Созаём папку `lists` в глайной папке запрета
```
mkdir /opt/zapret/lists
```
Далее перемещаемся в папку с листами
```
cd /opt/zapret/lists
```
Cоздаём файл с расширением`.txt` и любым именем, лучше указывать с учётом нужного адреса(адресов), например `googlevideo.txt`
```
touch googlevideo.txt
```
Открываем файл, можно уже из файлового менеджера, и в нём пишем список нужных адресов, каждый с новой строки, сохраняем
![Пример_4](https://github.com/user-attachments/assets/a5ef3608-d1af-41df-b244-cdf6cb5ae228)
И на этом настройка листа завершена
---



