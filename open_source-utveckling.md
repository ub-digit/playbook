# Open Source-utveckling

## Förberedelse

* Skapa en fork i Github:

  * Surfa in i huvudrepot på github och välj **fork**
  * Välj Att forka till **ditt eget** github-konto.

* Klona dit forkade repo

  ```bash
  git clone git@github.com:<YOUR_USERNAME>/<REPO_NAME>.git
  ```

* Lägg till huvudrepo som _**remote**_ som ges namnet _**upstream**_:

  ```bash
  cd <REPO_NAME>
  git remote -v
  git remote add upstream git@github.com:ub-digit/<REPO_NAME>.git
  git remote -v
  ```

* Hämta ner alla branchar, taggar med mera som finns i bl.a. uppstream-reposet

  ```bash
  git fetch --all
  ```

## Uppdatera egen master och skapa en ny feature branch

* Uppdatera master först se \(Få master-branchen i synk\)

```bash
git checkout -b my-feature
```

* Koda, fixa, greja, testa, committa som vanligt.
* När man är klar comitta och pusha feature-branchen till forken på Github.

```bash
git add .
# git commit -m "Bugfix"
# Skapa snyggare och tydligare och begripligare commit-meddelanden
# genom att inte använda flaggan -m.
git commit
git push
```

## Skapa en pull request

* Gör det på Github

## Få master-branchen i synk

Detta hämtar ned alla ändringar från upstream-reposet \(ja, från alla remote repos i själva verket\).  
Ändringarna ska sedan mergas in i den egna lokala mastern och pushas upp till  
den egna klonen på Github.

```bash
git checkout master
git fetch --all
git merge upstream/master
git push
```

## Updatera feature-branch med ändringar från upstream-repot

* Se till att lokala repot är uppdaterat och mastern är i synk med upstream-reposets master först \(se ovan\).
* Gör sedan följande:
  ```bash
  git checkout my-feature
  git rebase master
  Om en konflikt uppstår
  fixa konflikten och...
  git status
  git add .
  git status
  git rebase --continue
  Konflikten är löst
  så vi fortsätter som vanligt
  git status
  git push --force # Ja eftersom feature-branchen i min fork på Github är förlegad och i osynk måste vi tvinga mha --force
  ```

## Rensa upp efter en godkänd pullrequest

* När vi är klara med en pull request, dvs den har blivit godkänd och mergad eller avfärdad i en kodgranskning, behöver vi städa upp. Det gör vi genom att ta bort branchen från vår fork och sedan ta bort branchen från vårt lokala repositorium:

```bash
git push origin :my-feature
git branch -d my-feature
```

## Hämta hem en Pull request för lokal testning eller vidareutveckling

* Ta reda på ID för den Pull request vi vill hämta hem, detta görs i Github.
* Hämta vald Pull request och lagra lokalt i en ny namngiven branch med följande kommando:

```bash
# Givet att ID=33
git fetch upstream pull/33/head:the-branch-name
```

* Nu kan vi köra testerna.
* Om vi kodar vidare utifrån denna branch pushar vi den sedan till vår egen fork och gör en ny pull request.

## Regler

* Aldrig push till huvudrepo
* Aldrig push till master i eget repo
* Utveckling i branch i eget repo
* Vid färdig utveckling \(redo för merge\):
  * Push till branch i eget repo
  * \(från github\) Skapa pull-request
* \(någon annan helst\) Titta på pull-request och accepterar eller kommenterar



