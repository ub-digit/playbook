# Serverkonfiguration

Innehåller information om hur vi gåt tillväga när vi installerar och
konfigurerar webservern som ska köra UBNext.

I vissa instruktioner hämtas filer från config-repositoriet. Däri finns en katalog
som heter config/servers/www-production-1 och den förkortas nedan som ```${CONFIG}```.

## Diverse grundläggande komponenter

- Installera nödvändiga paket
```shell
sudo apt-get update
sudo apt-get install fail2ban vim ntp git
```

### Server-användare

- skapa användaren drupal-deploy
```shell
sudo adduser drupal-deploy
```
- gör henne till en sudo-användare
- ge henne en .gitconfig-fil
name: put gitconfig file in place for drupal-deploy user
```shell
sudo scp ${CONFIG}/home/drupal-deploy/.gitconfig
/home/drupal-deploy/.gitconfig
sudo chown drupal-deploy:drupal-deploy /home/drupal-deploy/.gitconfig
sudo chmod 664 /home/drupal-deploy/.gitconfig
```


- create tmp directory for drupal-deploy user
```shell
sudo mkdir /home/drupal-deploy/tmp
sudo chown drupal-deploy:drupal-deploy /home/drupal-deploy/tmp
sudo chmod  775 /home/drupal-deploy/tmp
```

- Skapa .ssh-katalog för drupal-deploy
- Kopiera in utvecklarnas publika nycklar

- Skapa .ssh-katalog för installer-användaren
- Kopiera utvecklarnas publika nycklar så att de kan logga in med installer-användaren

- Skapa användare för utvecklare
- Gör dem till sudo-användare
- Lägg in publika nycklar för utvecklarna


### Katalogstrukturer
```shell
sudo mkdir -p /srv/www/drupal7
```

## Mariadb

- Instalera mariadb

```shell
sudo apt-get install mariadb-server
```
- Kontrollera innehållet i /etc/mysql/my.cnf
- TODO: vad är en sund databas-konfiguration med avseende på connection-pool, minne etc.
- Ta bort anonym databasanvändare
- Ta bort test-databasen

## PHP

- installera php-fpm och php-mysql
```shell
sudo apt-get install php-fpm
sudo apt-get install php-mysql
```


## Nginx

- Lägg till nginx gpg-nyckel
  apt_key: id=7BD9BF62 url=http://nginx.org/keys/nginx_signing.key

- Lägg till apt-repositorium för nginx
```shell
sudo add-apt-repository ppa:ricotz/testin
```

  apt_repository: repo='deb http://nginx.org/packages/ubuntu/ {{ ansible_distribution_release }} nginx' state=present
```shell

sudo apt-get update
```

- Intallera nginx
```shell
sudo apt-get install nginx
```

- name: add nginx.conf
  copy: src=nginx.conf dest=/etc/nginx/nginx.conf mode=644 owner=root group=root
  notify: restart nginx

- name: create /var/lib/nginx
  file: path=/var/lib/nginx owner=www-data group=www-data state=directory

- name: create /etc/nginx/{{ item }}
  file: path=/etc/nginx/{{ item }} owner=root group=root state=directory
  with_items:
    - sites-available
    - sites-enabled


### Generell konfiguration

## TLS-terminator i Nginx eller Apache

## Certifikat i Nginx eller Apache

## Vhosts Nginx eller Apache

## Varnish
