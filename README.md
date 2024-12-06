# Самый простой гайд на zapret от bol-van для арчеводов
## Установка зависимостей
`sudo pacman -S --needed curl ipset bind git jq` строчка для арча и производных

`sudo dnf install -y curl ipset dnsutils git jq` строчка для федоры

`sudo apt install -y curl ipset dnsutils git jq` строчка для убунты

`sudo pacman -S nftables` установка nftables
### Установка самого запрета
Скачать последний релиз .tar.gz с [тык](https://github.com/bol-van/zapret/releases)

Распаковываем

Для этого понадобиться утилита tar и gzip

`sudo pacman -S tar`

`sudo pacman -S gzip`

Далее `tar -xvzf zapret-v69.5.tar.gz -C /home`
