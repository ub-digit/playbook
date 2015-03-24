# Capistrano
Capistrano är ett verktyg för automatisering av deploys samt andra tasks som man annars anropar manuellt. Capistrano är en utökning av rake, och därmed finns både Capistrano, Rake och Ruby syntax tillgänglig att använda sig utav.

Capistrano kan användas för alla typer av projekt, och är inte bundet till Ruby och/eller Rails specifika projekt.

## Dokumentation
http://capistranorb.com - Officiell dokumentation för Capistrano v3+
## Capistrano för rails
För Rails finns ett specifikt gem 'capistrano-rails'.

###Installation
Lägg till följande i Gemfile:
```
group :development do
  gem 'capistrano',  '~> 3.4.0'
  gem 'capistrano-rails', '~> 1.1'
  gem 'capistrano-passenger' #For passenger specific projects
end
```

