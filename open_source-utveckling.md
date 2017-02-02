# Open Source-utveckling

Stolpar ännu bara.

## Förberedelse

- Skapa en fork i Github:
  - Surfa in i huvudrepot på github och välj **fork**
  - Välj Att forka till **ditt eget** github-konto.

- Klona dit forkade repo
```bash
git clone git@github.com:<YOUR_USERNAME>/<REPO_NAME>.git
```

- Lägg till huvudrepo som ***remote***
```bash
cd <REPO_NAME>
git remote -v
git remote add upstream git@github.com:ub-digit/<REPO_NAME>.git
git remote -v
```

- Hämta ner alla branchar, taggar med mera som finns i bl.a. uppstream-reposet
```bash
git fetch --all
```

### Huvudrepo
### Eget repo

#### Remote

## Arbetsgång
Arbetsgång när man ska fixa en bugg eller börja jobba med en ny feature.

### Uppdatera egen master och skapa en ny feature branch
- Uppdatera master först se (Få master-branchen i synk)
```bash
git checkout -b my-feature
git push -u my-feature
```
### Koda, fixa, greja, testa, committa
Görs som vanligt. När man är klar comitta och pusha feature-branchen till forken på Github.
```bash
git add .
# git commit -m "Bugfix"
# Skapa snyggare och tydligare och begripligare commit-meddelanden
# genom att inte använda flaggan -m.
git commit
git push
```
### Skapa en pull request
- Gör det på Github

### Få master-branchen i synk
Detta hämtar ned alla ändringar från upstream-reposet (ja, från alla remote repos i själva verket).
Ändringarna ska sedan mergas in i den egna lokala mastern och pushas upp till
den egna klonen på Github.

```bash
git checkout master
git fetch --all
git merge upstream/master
git push
```

### Updatera feature-branch med ändringar från upstream-repot
- Se till att lokala repot är uppdaterat och mastern är i synk med upstream-reposets master först.
- Gör sedan följande:
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

#### Do the code review at github with a colleague
8. Keep the Fork master branch up to date (see 3)


#### Rensa upp efter en godkänd pullrequest
```bash
git push origin :my-feature
git branch -d my-feature
```



### Regler
* Aldrig push till huvudrepo
* Aldrig push till master i eget repo
* Utveckling i branch i eget repo
* Vid färdig utveckling (redo för merge):
  * Push till branch i eget repo
  * (från github) Skapa pull-request
* (någon annan helst) Titta på pull-request och accepterar eller kommenterar
