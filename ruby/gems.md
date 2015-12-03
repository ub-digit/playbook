# Skapa eget Gem

Ett Gem är i grunden ett paket med filer. Det består av en ```.gemspec``` som beskriver paketet, en katalog ```lib``` där rubykoden ligger samt blandade hjälpfiler och teststrukturer.

Gem är en fristående struktur som är självförsörjande med kod, beroenden och tester. Dess filträd ska alltså **inte** ingå i något annat repositorium.

## Komma igång

Först skapar man en mallstruktur att arbeta från. Detta görs enklast med kommandot ```bundle gem```.

```
$ bundle gem my_gem_name --test
      create  my_gem_name/Gemfile
      create  my_gem_name/Rakefile
      create  my_gem_name/LICENSE.txt
      create  my_gem_name/README.md
      create  my_gem_name/.gitignore
      create  my_gem_name/my_gem_name.gemspec
      create  my_gem_name/lib/my_gem_name.rb
      create  my_gem_name/lib/my_gem_name/version.rb
      create  my_gem_name/.rspec
      create  my_gem_name/spec/spec_helper.rb
      create  my_gem_name/spec/my_gem_name_spec.rb
      create  my_gem_name/.travis.yml
Initializing git repo in /home/stefan/src/gems/my_gem_name
$ 

```

Med detta har vi nu fått följande:
* ```my_gem_name.gemspec``` som beskriver själva paketet
* ```READEME.md``` med självförklarande namn
* ```LICENSE.txt``` som är satt till MIT-licens
* ```lib/``` katalog med en liten struktur i
* ```lib/my_gem_name/version.rb``` som innehåller versionsnumret av paketet
* ```lib/my_gem_name.rb``` där koden ligger, eller som inkluderar andra filer med kod
* ```spec/``` katalog för rspec-tester
* ```spec/my_gem_name_spec.rb``` där våra tester ligger
* Ett git-repo (hittills utan någon commit)

## Licens

```bundle gem``` lägger automatiskt till en MIT-licens i ```LICENSE.txt```. Detta skrivs också in i ```my_gem_name.gemspec```. Vill man byta licens behöver ändra båda dessa.

## Tester

I ```spec/my_gem_name.rb``` ligger två tester. Ett test kontrollerar att det finns en version satt (så att man inte av misstag tar bort version.rb). Det andra testet kontrollerar att false är true, vilket självklart inte är sant. Initialt går alltså testerna inte igenom.

I sann TDD-anda börjar man nu i testfilen och producerar tester, sedan löser man dem i koden, precis som vanligt.

För att köra sina tester kan man helt enkelt köra ```rake```.

```
$ rake
/home/stefan/.rvm/rubies/ruby-2.1.5/bin/ruby -I/home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-core-3.1.5/lib:/home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-support-3.1.1/lib /home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-core-3.1.5/exe/rspec --pattern spec/\*\*\{,/\*/\*\*\}/\*_spec.rb

MyGemName
  has a version number
  does something useful (FAILED - 1)

Failures:

  1) MyGemName does something useful
     Failure/Error: expect(false).to eq(true)
       
       expected: true
            got: false
       
       (compared using ==)
     # ./spec/my_gem_name_spec.rb:9:in `block (2 levels) in <top (required)>'

Finished in 0.00137 seconds (files took 0.06331 seconds to load)
2 examples, 1 failure

Failed examples:

rspec ./spec/my_gem_name_spec.rb:8 # MyGemName does something useful
/home/stefan/.rvm/rubies/ruby-2.1.5/bin/ruby -I/home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-core-3.1.5/lib:/home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-support-3.1.1/lib /home/stefan/.rvm/gems/ruby-2.1.5/gems/rspec-core-3.1.5/exe/rspec --pattern spec/\*\*\{,/\*/\*\*\}/\*_spec.rb failed
$ 

```

## Kod

Från början är koden man får ganska sparsam. Man får en versionsfil.

```
$ cat lib/my_gem_name/version.rb 
module MyGemName
  VERSION = "0.0.1"
end
$ 
```

Man får också en basfil som laddar versionsfilen samt har en plats för din kod.

```
$ cat lib/my_gem_name.rb 
require "my_gem_name/version"

module MyGemName
  # Your code goes here...
end
$ 
```

## Paketinformation

All information om paketet återfinns i ```my_gem_name.gemspec```.

```
$ cat my_gem_name.gemspec 
# coding: utf-8
lib = File.expand_path('../lib', __FILE__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require 'my_gem_name/version'

Gem::Specification.new do |spec|
  spec.name          = "my_gem_name"
  spec.version       = MyGemName::VERSION
  spec.authors       = ["Stefan Berndtsson"]
  spec.email         = ["stefan.berndtsson@gmail.com"]
  spec.summary       = %q{TODO: Write a short summary. Required.}
  spec.description   = %q{TODO: Write a longer description. Optional.}
  spec.homepage      = ""
  spec.license       = "MIT"

  spec.files         = `git ls-files -z`.split("\x0")
  spec.executables   = spec.files.grep(%r{^bin/}) { |f| File.basename(f) }
  spec.test_files    = spec.files.grep(%r{^(test|spec|features)/})
  spec.require_paths = ["lib"]

  spec.add_development_dependency "bundler", "~> 1.7"
  spec.add_development_dependency "rake", "~> 10.0"
  spec.add_development_dependency "rspec"
end
$ 
```

Vi kan här se att den automatiskt fyllt i mitt namn och min epost-adress baserat på vad som står i min git-konfiguration. Den har också lagt till MIT som licens. Versionen hämtas direkt från ```lib/my_gem_name/version.rb``` och behöver alltså bara uppdateras där.

De filer som kommer ingå i paketet genereras av ```git ls-files``` vilket innebär "ta med alla filer som commit:ats till repo". Allt som ska med måste alltså ingå i en commit. Det räcker inte att filen ligger i filstrukturen.

Beroenden på andra paket ska läggas till här.

```spec.add_development_dependency``` används för saker som behövs för att utveckla koden.

```spec.add_runtime_dependency``` kan man istället använda om koden behöver tillgång till ett annat paket när det ska köras (exempelvis om din kod behöver Nokogiri för att hantera XML).

Från början finns bara beroenden för utveckling.

Lämpligen skriver man nu en kort notis i ```spec.summary``` och en lite längre beskrivning i ```spec.description```, samt eventuellt lägger till något i ```spec.homepage```. Länk till github-repositoriet är vanligt för homepage.

# Bygga paketet

Att bygga paketet är enkelt, men först ska man se till att testerna går igenom. Normal procedur är alltså:

1. ```bundle``` för att installera eventuella beroenden
2. ```rake``` för att köra testerna
3. ```gem build my_gem_name.gemspec``` som bygger en ```my_gem_name-0.0.1.gem```

# Skicka paketet till rubygems.org (det blir då publikt tillgängligt)

För att skicka till rubygems.org behöver man ett konto där. Det fixar man via en webbläsare.

När man har konto och paketet är byggt så kör man ```gem push my_gem_name-0.0.1.gem```, svarar på inloggningsfrågorna och sedan är det klart och publicerat.
