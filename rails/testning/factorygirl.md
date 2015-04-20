# FactoryGirl

Jag har nu haft möjlighet att i dFlow implementera FactoryGirl, som är ett bra sätt att skapa testdata utan att behöva skriva Fixtures långt inpå ålderns höst.

För installation och övrig dokumentation, se [denna länk](http://www.rubydoc.info/gems/factory_girl/file/GETTING_STARTED.md).

Konceptet är enkelt, man definierar upp sina Factories , i 'spec/factories' inom **Factory.define**

[Exempel i dFlow](https://github.com/ub-digit/dFlow/tree/master/spec/factories)

För att skapa ett objekt som finns definierat som en Factory används sedan enkla kommandon, ex:

    build(:job) 
Skapar ett Job objekt med standardvärden utan att persistera

    build(:job, name: 'Ett fint jobb') 
Skapar ett Job objekt med standardvärden samt specifikt namn, utan att persistera

    create(:job) 
Skapar ett Job objekt med standardvärden och persisterar detta i databasen
[Exempel i dFlow](https://github.com/ub-digit/dFlow/blob/master/spec/models/job_spec.rb#L73)


Det går även enkelt att skapa en mängd objekt, nedan ett exempel där 20 Job skapas under en Treenode:

    treenode = create(:treenode)
    create_list(:job, 20, treenode: treenode)

[Exempel i dFlow](https://github.com/ub-digit/dFlow/blob/master/spec/controllers/api/treenodes_controller_spec.rb#L120-121)

Det finns även möjlighet att generera ex namn, användarnamn, epost via Sequence
[Exempel i dFlow](https://github.com/ub-digit/dFlow/blob/master/spec/factories/users.rb#L1-14)

För att skapa olika typer av objekt kan man nästla Factories inom varandra, och använda Traits för att definiera skillnaderna
[Exempel i dFlow](https://github.com/ub-digit/dFlow/blob/master/spec/factories/users.rb#L16-L37)

För ytterligare funktioner, se den välskrivna dokumentationen.