# Shoulda

Stötte på gemet 'shoulda' som jag kan rekommendera, ger bra hjälp för att testa standard Rails operationer.

Exempel: Hittade ett trasigt test i dFlow som var tänkt att validera att fältet 'title' var angivet:

    it "should require title" do
      job = Job.new(catalog_id: 12345, source: "libris", treenode_id: 1)
      expect(job.save).to be_falsey
    end

Testet gick igenom, men då vi lagt till ytterligare required fields i efterhand, så kunde det gå igenom av många olika anledningar, inte säkert för att ingen titel angavs.
Det korrekta sättet att testa ovanstående vore såklart att undersöka @errors objektet för att se ifall rätt felmeddelande dyker upp. Det är just detta som 'shoulda' hjälper till med.

Numera har jag med hjälp av 'shoulda' bytt ut ovanstående test mot:

    it {should validate_presence_of(:title)}

Ifall testet fallerar dyker följande fel upp:

    1) Job create job should require title to be set
         Failure/Error: it {should validate_presence_of(:title)}
           Expected errors to include "can't be blank" when title is set to nil,
           got no errors
     # ./spec/models/job_spec.rb:25:in `block (3 levels) in <top (required)>'

Detta innebär även att 'should validate_presence_of' går att använda även för custom validators, så länge det skapas ett fel med meddelande 'can't be blank'.

Se https://github.com/ub-digit/dFlow/blob/master/spec/models/job_spec.rb#L20-34 för det kompletta resultatet.


Länk: https://github.com/thoughtbot/shoulda
Docs: http://www.rubydoc.info/github/thoughtbot/shoulda-matchers