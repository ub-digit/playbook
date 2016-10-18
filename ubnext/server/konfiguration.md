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
sudo chmod 775 /home/drupal-deploy/tmp
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



---
---



# Config a la ubn-vm

## Grundläggande paket med mera

```bash
sudo apt-get update
sudo apt-get install curl -y
sudo apt-get install python-apt -y
sudo apt-get install python-pycurl -y
sudo apt-get install build-essential -y
sudo apt-get install sudo -y
sudo apt-get install unzip -y
sudo apt-get install vim-nox -y
# sudo apt-get install nmap -y # Denna behövs knappast. För nätverksanalys och OS-detektering
sudo apt-get install redis-server -y
```

## Brandvägg
- Avaktivera okomplicerade brandväggen ufw
```bash
sudo systemctl disable ufw.service
```

- Installera brandvägg (iptables)
```bash
sudo apt-get install iptables
sudo iptables -F
```
- Lägg brandväggsskript på plats
```bash
cp ${CONFIG}/etc/firewall.bash /etc/firewall.bash
sudo chown root:root /etc/firewall.bash
sudo chmod 0744 /etc/firewall.bash
```

- Starta om brandväggen
```bash
sudo systemctl restart firewall.service
```

- Lägg brandväggens init-skript på rätt plats
```bash
cp ${CONFIG}/etc/init.d/firewall /etc/init.d/firewall
sudo chown root:root /etc/init.d/firewall
sudo chmod 0755 /etc/init.d/firewall
```

- Se till att brandväggen startas som tjänst vid serverstart
```bash
sudo systemctl enable firewall.service
```

## Installera git
```bash
sudo apt-get install git
```


## Installera certifikat
- kopiera var och en av serverns privata nycklar
```bash
sudo scp ${USER_NAME}@${CERT_LOCATION}/${KEY_FILE} /etc/ssl/certs/
sudo chown root:root /etc/ssl/private/${KEY_FILE}
sudo chmod 0400 /etc/ssl/private/${KEY_FILE}
```

- kopiera var och en av serverns certifikat
```bash
sudo scp ${USER_NAME}@${CERT_LOCATION}/${CERT_FILE} /etc/ssl/certs/
sudo chown root:root /etc/ssl/certs/${CERT_FILE}
sudo chmod 0644 /etc/ssl/certs/${CERT_FILE}
```


## Installera Mariadb


### Mariadb - mine

- Instalera mariadb

```shell
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo echo 'deb [arch=amd64,i386,ppc64el] http://ftp.ddg.lth.se/mariadb/repo/10.1/ubuntu xenial main' > /etc/apt/sources.list.d/mariadb.list
sudo apt update
sudo apt-get install software-properties-common
sudo apt-get install mariadb-server -y
sudo apt-get python-mysqldb
```
- Kontrollera innehållet i /etc/mysql/my.cnf


- Skapa slow-query-log (TODO: fungerar detta i MariaDB?) (som ska ägas av mysql-användaren chown:mysql:mysql chmod: 0640)
- Skapa Error-log (som ska ägas av mysql-användaren chown:mysql:mysql chmod: 0640)
- TODO: vad är en sund databas-konfiguration med avseende på connection-pool, minne etc.
- Ta bort anonym databasanvändare om den finns.
- Ta bort test-databasen om den finns.

- Aktivera MariaDB som en serverprocess
```bash
sudo systemctl enable mariadb
```


### Mariadb - geerlingguy

- geerlingguy.mysql
```bash
```

- include: configure.yml
- include: secure-installation.yml
- include: databases.yml
- include: users.yml
- include: replication.yml

## PHP med mera

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
Ändra i filen __/etc/php/7.0/fpm/php.ini__ raden med ```;cgi.fix_pathinfo=1``` så att den blir ```cgi.fix_pathinfo=0```.
- Starta om tjänsten
```shell
sudo systemctl restart php7.0-fpm
```

- geerlingguy.php
- geerlingguy.php-pecl
- geerlingguy.php-mysql
- geerlingguy.composer
- geerlingguy.drush
- php.xdebug
```bash
```



## Installera Nginx

- Hämta hem och lägg till nginx gpg-nyckel
```bash
sudo wget -qO - http://nginx.org/keys/nginx_signing.key | sudo apt-key add -
```

- Lägg till apt-repositorium för nginx
```bash
sudo echo 'deb http://nginx.org/packages/ubuntu/ xenial nginx' > /etc/apt/sources.list.d/nginx.list
sudo echo 'deb-src http://nginx.org/packages/ubuntu/ xenial nginx' >> /etc/apt/sources.list.d/nginx.list
```

- Installera nginx
```bash
sudo apt-get update
sudo apt-get install python-selinux # Oklart varför denna behövs
sudo apt-get install nginx
```

- Skapa kataloger för nginx
```bash
if ! [ -d /etc/nginx/sites-available ] ; then sudo mkdir /etc/nginx/sites-available ; fi
if ! [ -d /etc/nginx/sites-enabled ] ; then sudo mkdir /etc/nginx/sites-enabled ; fi
if ! [ -d /etc/nginx/auth_basic ] ; then sudo mkdir /etc/nginx/auth_basic ; fi
if ! [ -d /etc/nginx/conf.d ] ; then sudo mkdir /etc/nginx/conf.d ; fi
if ! [ -d /etc/nginx/conf.d/stream ] ; then sudo mkdir /etc/nginx/conf.d/stream ; fi
# sudo chown root:www-data /etc/nginx/sites-available # Varför ska www-data vara grupp?
# sudo chown root:www-data /etc/nginx/sites-enabled # Varför ska www-data vara grupp?
# sudo chown root:www-data /etc/nginx/auth_basic # Varför ska www-data vara grupp?
# sudo chown root:www-data /etc/nginx/conf.d # Varför ska www-data vara grupp?
# sudo chown root:www-data /etc/nginx/conf.d/stream # Varför ska www-data vara grupp?
sudo chmod 0755 /etc/nginx/sites-available
sudo chmod 0755 /etc/nginx/sites-enabled
sudo chmod 0755 /etc/nginx/auth_basic
sudo chmod 0755 /etc/nginx/conf.d
sudo chmod 0755 /etc/nginx/conf.d/stream
```

- Skapa kataloger för loggar
```bash
if ! [ -d /var/log/nginx ] ; then sudo mkdir /var/log/nginx ; fi
sudo chown root:www-data /var/log/nginx # Varför ska root vara ägare? Korrigera nginx user i nginx.conf!!!
sudo chmod 0755 /var/log/nginx
```

### Städa
Går ut på att ta bort sådant som inte används och som kan störa.
Titta i ansible-skripten var ganska komplicerade här, men se skripten i
playbooken för detaljer. Sunt förnuft är användbart.

#### Remove defaults
- Avaktivera default siten
- Ta bort default-konfigurationen
- Ladda om nginx

#### Remove extras
- name: Find enabled sites
- name: Disable unmanaged sites
- name: Find config files
- name: Remove unmanaged config files
- Ladda om nginx

#### Remove unwanted
- Ta bort oönskade siter
- Ta bort oönskad konfiguration
- Ta bort oönskade auth_basic_files i /etc/nginx/auth_basic
- Ladda om nginx

### Konfiguration

- Kopiera över nginx.conf (huvud-configuration) och eventuella include-konfigurationsfiler, testa konfigen
``bash
if test -e ${CONFIG}/etc/nginx/*.conf ; then sudo cp ${CONFIG}/etc/nginx/*.conf /etc/nginx/ ; sudo nginx -t ; fi
sudo systemctl restart nginx
```

- Lägg eventuellt in auth_basic-filer
``bash
if test -e ${CONFIG}/etc/nginx/auth_basic/*
then sudo cp ${CONFIG}/etc/nginx/auth_basic/* /etc/nginx/auth_basic/
sudo nginx -t
sudo chown root:www-data /etc/nginx/auth_basic/*
sudo chmod 0750 /etc/nginx/auth_basic/*
sudo nginx -t
sudo systemctl restart nginx
fi
```

- Kopiera över eventuella konfigurationsfiler till conf.d och testa ny konfig
``bash
if test -e ${CONFIG}/etc/nginx/conf.d/* ; then sudo cp ${CONFIG}/etc/nginx/conf.d/* /etc/nginx/conf.d/ ; sudo nginx -t ; fi
```

- starta om nginx
```bash
sudo systemctl restart nginx
```

- Aktivera nginx som systemtjänst
```bash
sudo systemctl enable nginx
```

- Lägg in vhosts filer i /etc/nginx/sites-available/
```bash
sudo cp ${CONFIG}/etc/sites-available/www-lab.conf /etc/nginx/sites-available
sudo cp ${CONFIG}/etc/sites-available/www-staging.conf /etc/nginx/sites-available
sudo cp ${CONFIG}/etc/sites-available/www-production.conf /etc/nginx/sites-available
```
- Skapa länkar till siterna i sites-enabled och testa ny konfig
```bash
cd /etc/nginx/sites-enabled
sudo ln -s ../sites-available/www-lab.conf
sudo ln -s ../sites-available/www-staging.conf
sudo ln -s ../sites-available/www-production.conf
cd -
sudo nginx -t
sudo systemctl reload nginx
```

## Installera Apache2

- Installera Apache
```bash
sudo apt update
sudo apt-get install apache2 apache2-utils -y

```

- Konfigurera portar
```bash
if test -e ${CONFIG}/etc/apache2/ports.conf ; then sudo cp ${CONFIG}/etc/apache2/ports.conf /etc/apache2/ports.conf ; fi
```

-Aktivera moduler
```bash
sudo a2enmod rewrite
sudo a2enmod ssl
sudo a2enmod proxy
sudo a2enmod proxy_fcgi
```

- Kopiera in vhost filer från config-reposet
```bash
sudo cp ${CONFIG}/etc/apache2/sites-available/www-lab.conf /etc/apache2/sites-available
sudo cp ${CONFIG}/etc/apache2/sites-available/www-staging.conf /etc/apache2/sites-available
sudo cp ${CONFIG}/etc/apache2/sites-available/www-production.conf /etc/apache2/sites-available
sudo chown -R root:root /etc/apache2/sites-available
sudo chmod 0644 /etc/apache2/sites-available/*.conf
```

- Aktivera vhost:arna
```bash
sudo a2ensite www-lab.conf
sudo a2ensite www-staging.conf
sudo a2ensite www-production.conf
```

- Ta bort default vhosts i sites-enabled
```bash
# sudo rm /etc/apache2/sites-enabled/*default*.conf
```

-- Starta om apache
```bash
sudo systemctl restart apache2
```

- Aktivera apache som systemtjänst
```bash
sudo systemctl enable apache2
```




## Säkerhet vid produktionsmiljö
- geerlingguy.security
```bash
```


---

## SSH
- include: tasks/sshd.yml
- include: tasks/www.yml
- include: tasks/apparmor.yml
```bash
```

## Hämta ner ubnext-kod
- tasks/drupal7-ubn-dev.yml
```bash
```

## Skapa drush-aliases
```bash
```

## Importera databas från produktionsmiljön
tasks/drupal7-ubn-dev-db.yml
```bash
```

## Sätt upp drupal cron
tasks/cron.yml
```bash
```

```bash
```
