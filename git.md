# Git
Klona ett repository till din dator
```
git clone <repository-url> <lokal-mapp>
```
Skapa en branch lokalt för att utveckla 'new_stuff'
```
git checkout -b new_stuff
```
Lägg till samtliga ändringar för commit
```
git add .
```
Lägg till samtliga ändringar inklusive borttagningar av filer et cetera
```
git add --all
```
Commit:a ändringar till den brancgh du står i
```
git commit -m "My commit message"
```
Byt utecheckad branch till 'master'
```
git checkout master
```
Hämta ev uppdateringar för nuvarande branch
```
git fetch
git merge
```
Byt tillbaka till utvecklingsbranch och migrera ändringar som skett från master
```
git checkout new_stuff
git merge master
```
Byt branch till master och migrera dina ändringar från utvecklingsbranchen
```
git checkout master
git merge new_stuff
```
Skicka ändringar till respositoriet
```
git push
```
