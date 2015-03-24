# Capistrano
Capistrano är ett verktyg för automatisering av deploys samt andra tasks som man annars anropar manuellt. Capistrano är en utökning av rake, och därmed finns både Capistrano, Rake och Ruby syntax tillgänglig att använda sig utav.

Capistrano kan användas för alla typer av projekt, och är inte bundet till Ruby och/eller Rails specifika projekt.

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
end
```

Installera nya paket
```
bundle install
```

Skapa Capistrano configurationsfiler:
```
cd /path/to/your/project
capify .
```
>capify skapar filerna /Capfile, /config/deploy.rb samt /config/deploy/production.rb

### Inställningar
Filerna i **deploy** mappen (production.rb et c.) representerar var sin environment, och bör ha en motsvarande fil i **environments**.

**deploy.rb** innehåller standardinställningar för kommandot **cap deploy**, men den kan även lämnas tom och skrivas över från de environment-specifika filerna i mapppen **deploy**.

### Capistrano settings
Nedan följer ett antal standardinställningar som kan användas i **deploy.rb** eller i **deploy/**