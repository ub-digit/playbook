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

### Server Directories



## Mariadb

### Säkerhet i Mariadb

## Nginx

### Generell konfiguration

## TLS-terminator i Nginx eller Apache

## Certifikat i Nginx eller Apache

## Vhosts Nginx eller Apache

## Varnish
