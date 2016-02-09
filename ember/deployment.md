# Deployment

Ember appar deployas med hjälp av ett internt skapat skript **config/tools/ubdeploy.sh**.

För att genomföra en deploy, gå igenom följande steg:

### Skapa environments.js fil i config-repositoriet
Då skriptet använder sig av incheckad config-data i config-repositoriet, måste denna uppdateras innan deploy kan genomföras.

**environment.js** ska finnas i config/apps/catindex/config-data/default/config/environment.js.

### Checka in alla ändringar i repositoriet
Skriptet checkar ut repositoriet du står i när du genomför operationen, vilket innebär att alla ändringar som ska deployas måste vara incheckade i **master**.

### Kör skriptet från app-rooten
Kör `ubdeploy.sh lab` från repositoriets root för att genomföra deployen för miljön `lab`. 