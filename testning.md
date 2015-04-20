# Testning

För testning av Rails APIer används RSpec (http://rspec.info/).

Gemfile
```
group :development, :test do
  gem 'rspec-rails', '~> 3.0'
end
```

Installation
```
bundle install
rails generate rspec:install
```
Efter installation kommer rails generators att skapa testfiler under /spec/ automatiskt när de används. 

För att generera testfiler för ex ett Model-objekt separat, använd följande syntax:
```
rails generate rspec:model MyModel
```
För att köra hela testsviten
```
bundle exec rspec
```
Testa en specifik fil
```
bundle exec rspec spec/models/test_model_spec.rb
```
