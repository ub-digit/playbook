# Rails

Skapa ett rails projekt
```
rails new <projektnamn>
```
Exempel Gemfile
```
ruby "2.1.5"

gem 'rails', '4.2.0'

# Choose your database adapter
#gem 'sqlite3'
gem 'pg'

# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem 'jbuilder', '~> 2.0'
# bundle exec rake doc:rails generates the API under doc/api.
gem 'sdoc', '~> 0.4.0',          group: :doc

group :test, :development do
  # Use rspec for testing
  gem 'rspec-rails', '~> 3.1'
  
  # Gem for defining factories for testing purposes, rather than using fixtures
  gem "factory_girl_rails", "~> 4.0"
  
  # Implicit dependency of FactoryGirl
  gem 'database_cleaner'
end

group :test do
  # Gem for mocking web requests
  gem 'webmock'
  
  # Gem for creating simple ActiveRecord tests
  gem 'shoulda'
end

# Gem for XML translations
gem 'nokogiri'

# Use ActiveModel has_secure_password
gem 'bcrypt', '~> 3.1.7'

# CodeClimate gem
gem "codeclimate-test-reporter", group: :test, require: nil

# CORS support gem for APIs
gem 'rack-cors', :require => 'rack/cors'

# Pagination gem
gem 'will_paginate', '~> 3.0.5'

# PDF Gem
gem 'prawn' # For PDF creation

gem 'responders', '~> 2.0'

# API documentation gem
gem 'apipie-rails'

# Capistrano for deployment to different environments
group :development do
  gem 'capistrano',  '~> 3.4.0'
  gem 'capistrano-rails', '~> 1.1'
  gem 'capistrano-passenger'
end
```