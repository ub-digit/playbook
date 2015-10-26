# Rspec

## Metodik

Vi tänker oss att testa flöden med Selenium. För att bevara sammanhanget är det lämpligt att ha ett flöde i en `_spec`-fil, och inte blanda flera flöden i en fil.

Se exemplen mot Koha nedan.

## Installation och konfiguration

Installera gem för rspec och selenium-webdriver.

```
$ gem install rspec
$ gem install selenium-webdriver
```

Initiera rspec i ditt projekt.

```
$ rspec --init
```

Detta skapar en `.rspec`-fil som vi redigerar en aning till:
```
--color
--format documentation
--require spec_helper
```

Du har även en `spec`-katalog med en `spec_helper.rb` i. Precis som i rspec för Rails är det här vi lägger alla våra tester.

Sannolikt behöver vi lite konfigurationsinställningar som är specifika för de flöden vi ska testa, så vi skapar en katalog `spec/support/` där detta ligger.

Konfiguration lägger vi t ex i `spec/support/config.yml` och `spec/support/config_secret.yml`:

```
---
username: "loginuser"
password: "loginpass"
intraurl: "http://kohaserver-intra.example.com"
patronbarcode: "1234567890"
patronname: "Patron Name"

```

För att ladda konfiguration och tillhandahålla eventuella funktioner skapar vi en `spec/support/setup.rb`:

```
require 'yaml'

module SetupHelpers
  def init_config
    @config = YAML.load(File.open("spec/support/config_secret.yml"))
    @driver = Selenium::WebDriver.for :firefox
  end

  def login
    @driver.navigate.to @config["intraurl"]

    userform = @driver.find_element(id: "userid")
    userform.click
    userform.send_keys @config["username"]

    passform = @driver.find_element(id: "password")
    passform.click
    passform.send_keys @config["password"]

    submitform = @driver.find_element(id: "submit")
    submitform.click
  end
end
```

För att dessa ska laddas automatiskt lägger vi till följande rader i början av `spec/spec_helper.rb`:
```
require_relative 'support/setup'
require 'selenium-webdriver'

include SetupHelpers

```

Sist i konfigurationsblocket i `spec/spec_helper.rb` lägger vi också till:
```
  config.before :all do
    init_config
  end

  config.after :all do
    @driver.quit
  end

```

## Flöden

Här kommer ett par exempel på flöden mot en Koha-instans.

Inloggning:
```
describe "Login" do
  it "1. Should show login page" do
    @driver.navigate.to @config["intraurl"]

    userform = @driver.find_element(id: "userid")
    passform = @driver.find_element(id: "password")
    expect(userform).to_not be_nil
    expect(passform).to_not be_nil
  end

  it "2. Should be able to login" do
    userform = @driver.find_element(id: "userid")
    userform.click
    userform.send_keys @config["username"]

    passform = @driver.find_element(id: "password")
    passform.click
    passform.send_keys @config["password"]

    submitform = @driver.find_element(id: "submit")
    submitform.click

    logo = @driver.find_element(id: "logo")
    expect(logo).to_not be_nil
  end
end
```

Söka efter en användare:
```
describe "Search Patron" do
  before :all do
    @patron_cardnumber = @config["patronbarcode"]
    login
  end

  it "1. Should give patron page for card number" do
    patron_search = @driver.find_element(xpath: "//li[@aria-controls='patron_search']")
    patron_search.click
    
    patron_search_input = @driver.find_element(id: "searchmember")
    patron_search_input.click
    patron_search_input.send_keys @patron_cardnumber
    patron_search_input.submit
    patron_name_h3 = @driver.find_element(xpath: "//div[@id='yui-main']/div[@class='yui-b']/div[@class='yui-g']/h3")
    expect(patron_name_h3).to_not be_nil
    expect(patron_name_h3.text).to match(@config["patronname"])
  end
end
```

## Testning och resultat
När vi nu kör våra tester kan ett lyckat resultat se ut såhär:

```
$ rspec 

Login
  1. Should show login page
  2. Should be able to login

Search Patron
  1. Should give patron page for card number

Finished in 20.61 seconds (files took 0.14475 seconds to load)
3 examples, 0 failures
```
