# Serverkonfiguration

Innehåller information om hur vi gåt tillväga när vi installerar och
konfigurerar webservern som ska köra UBNext.

I vissa instruktioner hämtas filer från config-repositoriet. Däri finns en katalog
som heter config/servers/www-production-1 och den förkortas nedan som ```${CONFIG}```.

## Diverse grundläggande komponenter

- Installera nödvändiga paket
```shell
sudo apt-get update
sudo apt-get install fail2ban vim ntp git -y
```

### Server-användare

- skapa användaren drupal-deploy
```shell
sudo adduser drupal-deploy
```

- create tmp directory for drupal-deploy user
```shell
sudo mkdir /home/drupal-deploy/tmp
sudo chown www-data:www-data /home/drupal-deploy/tmp
sudo chmod  775 /home/drupal-deploy/tmp
```

- Skapa .ssh-katalog för drupal-deploy
```shell
sudo mkdir /home/drupal-deploy/.ssh
```

- Sätt korrekt ägarskap och rättigheter på .ssh-katalogen.
```shell
sudo chown -R drupal-deploy:drupal-deploy /home/drupal-deploy/.ssh
sudo chmod go-rwx /home/drupal-deploy/.ssh
```

- Kopiera in utvecklarnas publika nycklar (finns att hämta i config-repositoriet).

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
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://ftp.ddg.lth.se/mariadb/repo/10.1/ubuntu xenial main'
sudo apt update
sudo apt install mariadb-server -y
```
- Kontrollera innehållet i /etc/mysql/my.cnf
- TODO: vad är en sund databas-konfiguration med avseende på connection-pool, minne etc.
- Ta bort anonym databasanvändare om den finns.
- Ta bort test-databasen om den finns.

## PHP

- installera php-fpm och php-mysql
```shell
sudo apt-get install php-fpm -y
sudo apt-get install php-mysql -y
```

- Kolla att php-fpm körs
```shell
sudo systemctl status php7.0-fpm
```

- Editera php.ini
Ändra i filen __/etc/php/7.0/fpm/php.ini__ raden med ```cgi.fix_pathinfo=1``` så att den blir ```cgi.fix_pathinfo=0```.
- Starta om tjänsten
```shell
sudo systemctl restart php7.0-fpm
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
