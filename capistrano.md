# Capistrano
Capistrano är ett verktyg för automatisering av deploys samt andra tasks som man annars anropar manuellt. Capistrano är en utökning av rake, och därmed finns både Capistrano, Rake och Ruby syntax tillgänglig att använda sig utav.

Capistrano kan användas för alla typer av projekt, och är inte bundet till Ruby och/eller Rails specifika projekt.

Capistrano deployar via ssh till angiven remote host, därmed bör lämpliga nycklar existera:
1. Till remote-host från anropande maskin
2. Till Git-server från remote-host

## Dokumentation
http://capistranorb.com - Officiell dokumentation för Capistrano v3+
## Capistrano för rails
För Rails finns ett specifikt gem 'capistrano-rails':

https://github.com/capistrano/rails/

###Installation
Lägg till följande i Gemfile:
```
group :development do
  gem 'capistrano',  '~> 3.4.0'
  gem 'capistrano-rails', '~> 1.1'
  gem 'capistrano-passenger' #For passenger specific projects
  gem 'capistrano-bundler', '~> 1.1.2' #To be able to run bundle install on deploy
  gem 'capistrano-rvm' # För att hantera ruby version vid deploy
end
```

Installera nya paket
```
bundle install
```

Skapa Capistrano configurationsfiler:
```
cd /path/to/your/project
cap install
```
>cap install skapar filerna: 

>Capfile

>config/deploy.rb

>config/deploy/production.rb

### Capistrano Inställningar
I **Capfile** definieras beroenden som ska inkluderas i Capistrano. I vårat exempel tar vi med följande paket:

* require 'capistrano/setup'
* require 'capistrano/deploy'
* require 'capistrano/bundler'
* require 'capistrano/rails/migrations'
* require 'capistrano/passenger'

Filerna i **deploy** mappen (production.rb et c.) representerar var sin environment, och bör ha en motsvarande fil i **environments**.

**deploy.rb** innehåller standardinställningar för kommandot **cap deploy**, men den kan även lämnas tom och skrivas över från de environment-specifika filerna i mapppen **deploy**.

### Deploy inställningar
Nedan följer ett antal standardinställningar som kan användas i **deploy.rb** eller i **deploy/**

Förslagsvis läggs alla inställningar som är generella för applikationen i **config/deploy.rb**

```
# Set the application name
set :application, 'dFlow'

# Set the repository link
set :repo_url, 'git@github.com:ub-digit/dFlow.git'

# Set tmp directory on remote host - Default value: '/tmp , which often will not allow files to be executed
set :tmp_dir, '/home/rails/tmp'

# Copy originals into /{app}/shared/config from respective sample file
set :linked_files, %w{config/database.yml config/config_secret.yml config/passwd}

server 'stagingserver.com', user: 'user', roles: %w{app db web}

set :deploy_to, '/remote/path/to/app'

set :rvm_ruby_string, :local              # use the same ruby as used locally for deployment

# Forces user to assign a valid tag for deploy
def get_tag
  all_tags = `git tag`.split("\n")

  ask :answer, "Tag to deploy (make sure to push the tag first): #{all_tags} "
  tag = fetch(:answer)
  if !all_tags.include? tag
    abort "Tag #{tag} is not a valid value"
  end
  tag
end

set :branch, get_tag # Sets branch according to given tag
```

### Skapa environment för deploy
>**OBS: Kom ihåg att alla filer som innehåller känslig information (servernamn / användarnamn ex.) inte ska checkas in i repositoriet. Skapa isf sample-filer (config/deploy/production.rb.sample) som checkas in utan denna information.**

I exemplet nedan skapar vi en environment för en testserver, som vi döper till **lab**. Detta då **test** används inom rails för att köra testsviten.

#### Uppdatera production.rb
Kommentera ut raden **config.assets.js_compressor = :uglifier
** ifall den finns i **config/environments/production.rb**

Sätt **config.serve_static_files = true** i samma fil.

#### Skapa environment fil
Kopiera **config/environments/production.rb** till **config/environments/lab.rb**

`cp config/environments/production.rb config/environments/lab.rb`

#### Skapa environment i secrets-fil
Kopiera **production** elementet i **config/secrets.yml** och döp om det kopierade elementet till **lab:**

#### Skapa environment i database-fil
Kopiera **production** elementet i **config/database.yml** och döp om det kopierade elementet till **lab:**. Uppdatera med korrekta uppgifter.

#### Skapa Capistrano environment
Nästa steg är att skapa en Capistrano environment som överensstämmer med den Rails-environment vi just har skapat.

Kopiera därmed **config/deploy/production.rb** som **config/deploy/lab.rb**

`cp config/deploy/{production, lab}.rb`

Uppdatera konfig filen med gällande servernamn, användarnamn och deploy sökväg för lab-environment.

#### Lägg till SSH nycklar till servrarna
Lägg till SSH nycklar för att slippa logga in under deploy.

#### Skapa tmp mapp på servern ifall sådan ej existerar.
Då Capistrano deployar genom att skapa en .sh fil, ladda upp denna till servern och sedan exekvera denna, behövs en tmp-mapp som den aktuella användaren har rätt att köra skriptet från.

Denna läggs lämpligen i användarens hem-katalog. 

Se sedan till att variabeln **tmp_dir** går mot denna mapp i **config/deploy.rb**

**deploy.rb:**

`set :tmp_dir, '/home/rails/tmp'`

#### Skapa mapp på servern att deploya till
Skapa samma mapp som angetts som variabeln **deploy_to**

#### Konfigurationsfiler
De konfig-filer som inte ligger i repositoriet med korrekta värden, måste kopieras till servern samt anges som **linked_files** i **deploy.rb**.

I exemplet nedan ska vi ange database.yml som delad fil.

##### Kopiera över filen
Kopiera filen till servern enligt mappstrukturen **&lt;deploy_to&gt;/shared/config/database.yml**

#### Ange filen som linked_file
Lägg till nedanstående i **deploy.rb**

`set :linked_files, %w{config/database.yml}`

### Deploy kommandon

För att kolla så att inga rättigheter eller liknande saknas:
```
cap <environment> deploy:check
```

För att köra deploy:
```
cap <environment> deploy
```