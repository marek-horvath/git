# Git den 2 - suvisly demo flow od slidu 44

Toto je scenar na druhy den. Zacina v stave, v ktorom ucastnici skoncili prvy den:

- maju nastavene SSH kluce,
- maju vlastne GitHub repository,
- maju lokalny clone,
- v clone maju skopirovane tvoje HTML ulohy,
- uz ste presli `status`, `add`, `commit`, `diff`, `log`, `show`, `revert`, `clone`, `push`, `.gitignore`, `working tree`, `staging area`, `repository`.

Scenar je jeden suvisly flow. Nepustaj ho cely naraz. Chod po mensich blokoch a po dolezitych prikazoch sa zastav pri vystupe.

V prikladoch pouzivam hlavny branch `main`. Ak ma niekto repository s branchom `master`, v prikazoch jednoducho nahradi `main` za `master`.

## Zaciname v existujucom clone

```bash
pwd # ukazeme, kde sa nachadzame
ls -a # vidime obsah projektu aj skryty priecinok .git

git status # kontrolujeme, ci nemame rozrobene zmeny
git remote -v # overime, ze clone je napojeny na GitHub cez origin
git branch -a # pozrieme local aj remote branches

git checkout main # prepneme sa na hlavny branch
git pull origin main # stiahneme najnovsi stav z GitHubu
git log --oneline --graph --all --decorate --max-count=10 # rychly pohlad na historiu
```

Ak niekto este nema commitnute HTML ulohy z prveho dna, nech najprv spravi toto a potom pokracuje dalej:

```bash
git status # pozrieme, co je zmenene
git add . # pripravime skopirovane ulohy
git commit -m "feat: add training tasks" # ulozime ulohy do historie
git push origin main # posleme ich na GitHub
git status # working tree ma byt cisty
```

## Vytvorime prvy feature branch a budeme sa prepinat

```bash
git checkout -b feature/day2-intro # vytvorime novy branch na zmenu, ktoru nechceme robit rovno na main
git branch # hviezdicka ukazuje, na ktorom branchi stojime

echo "# Den 2 demo" > day2-flow.md # vytvorime novy subor iba na feature branchi
echo "" >> day2-flow.md # pridame prazdny riadok
echo "Toto je subor na skusanie branchov." >> day2-flow.md # pridame text
git status # Git ukaze novy untracked file
git add day2-flow.md # pripravime subor do staging area
git commit -m "feat: add day 2 flow file" # ulozime prvy commit na feature branchi

echo "" >> day2-flow.md # pridame prazdny riadok
echo "Tento riadok vznikol na feature branchi." >> day2-flow.md # upravime subor este raz
git diff # ukazeme rozdiel vo working tree
git add day2-flow.md # pripravime zmenu
git diff --staged # ukazeme staged diff
git commit -m "feat: extend day 2 flow" # druhy commit na feature branchi

git log --oneline --graph --all --decorate --max-count=12 # ukazeme, ze feature branch ma vlastne commity

git checkout main # prepneme sa spat na hlavny branch
ls # ukazeme, ze subor z feature branchu tu este nemusi byt
git log --oneline --graph --all --decorate --max-count=12 # feature branch existuje bokom

git checkout feature/day2-intro # ideme znova na feature branch
ls # subor day2-flow.md sa objavi
cat day2-flow.md # ukazeme obsah suboru

git checkout main # vraciame sa na hlavny branch, aby sme spravili merge
echo "Zmena priamo na hlavnom branchi." > main-note.txt # spravime na main inu zmenu
git add main-note.txt # staging
git commit -m "feat: add main branch note" # commit na main

git log --oneline --graph --all --decorate --max-count=15 # historia ma teraz dve linie
git merge feature/day2-intro --no-edit # mergujeme feature branch do hlavneho branchu
git log --oneline --graph --all --decorate --max-count=15 # ukazeme vysledok merge
git status # working tree ma byt cisty

git push origin main # po merge posleme hlavny branch na GitHub
git branch -d feature/day2-intro # lokalny feature branch uz nepotrebujeme
git branch # kontrola lokalnych branchov
```

## Urobime zamerne merge conflict

```bash
echo "tema: povodna verzia" > konflikt.txt # spolocny zaklad pre conflict demo
git add konflikt.txt # staging
git commit -m "feat: add conflict base" # commitneme zakladny subor

git checkout -b feature/konflikt # novy branch, kde zmenime rovnaky riadok
echo "tema: verzia z feature branchu" > konflikt.txt # zmena na feature branchi
git diff # ukazeme, co sa zmenilo
git add konflikt.txt # staging
git commit -m "feat: change topic in feature branch" # commit na feature branchi

git checkout main # spat na hlavny branch
echo "tema: verzia z main branchu" > konflikt.txt # zmenime ten isty riadok inak
git diff # ukazeme inu zmenu na main
git add konflikt.txt # staging
git commit -m "feat: change topic in main branch" # commit na main

git log --oneline --graph --all --decorate --max-count=18 # ukazeme rozdelenu historiu pred conflictom
git merge feature/konflikt # teraz zamerne vznikne merge conflict

git status # Git ukaze unmerged paths
cat konflikt.txt # ukazeme conflict markers v subore
git diff # ukazeme diff pocas conflictu

echo "tema: finalna spolocna verzia" > konflikt.txt # rucne vyriesime conflict jednym vyslednym riadkom
cat konflikt.txt # kontrola, ze tam uz nie su conflict markers
git add konflikt.txt # tym hovorime Gitu, ze conflict je vyrieseny
git status # Git caka na dokoncenie merge commitu
git commit -m "merge: resolve topic conflict" # dokoncime merge

git log --oneline --graph --all --decorate --max-count=20 # ukazeme merge commit s dvoma rodicmi
git branch -d feature/konflikt # vymazeme merged branch
git push origin main # posleme vyrieseny merge na GitHub
```

## Ukazeme stash a maly hotfix

```bash
echo "" >> day2-flow.md # pridame prazdny riadok
echo "Rozpracovana zmena, ktoru este nechcem commitovat." >> day2-flow.md # zacneme pracu, ale este ju nechceme ulozit
git status # working tree je dirty
git diff # ukazeme rozpracovanu zmenu

git stash push -m "WIP day2-flow text" # docasne odlozime rozpracovanu zmenu
git status # working tree je znova cisty
git stash list # vidime ulozeny stash

git checkout -b hotfix/footer-text # teraz mozeme pokojne riesit inu urgentnu vec
echo "Footer hotfix text" > footer-hotfix.txt # hotfix subor
git add footer-hotfix.txt # staging
git commit -m "fix: add footer hotfix text" # commit hotfixu

git checkout main # spat na hlavny branch
git merge hotfix/footer-text --no-edit # mergujeme hotfix
git branch -d hotfix/footer-text # upraceme hotfix branch

git stash list # stale mame odlozenu rozpracovanu zmenu
git stash pop # vratime rozpracovanu zmenu spat
git status # zmena je naspat vo working tree
git diff # ukazeme, co sa vratilo
git add day2-flow.md # pripravime dokoncenu zmenu
git commit -m "feat: finish day 2 flow text" # commitneme ju
git push origin main # posleme hotfix aj dokoncenu zmenu na GitHub
```

## Simulujeme kolegu cez druhy clone toho isteho GitHub repository

V tomto bloku si najprv pozri URL cez `git remote -v`. V prikaze `git clone` pouzi svoju SSH URL z GitHubu. Priecinok `kolega-copy` je druhy clone, ktory bude hrat kolegu.

```bash
pwd # zapamataj si nazov povodneho priecinka
git remote -v # odtialto si zober SSH URL svojho repository

cd .. # ideme o uroven vyssie
git clone git@github.com:tvoje-meno/tvoje-repo.git kolega-copy # URL nahrad svojou SSH URL

cd kolega-copy # prepneme sa do druheho clone
git status # cisty clone
git log --oneline --graph --all --decorate --max-count=10 # historia je rovnaka ako na GitHube

echo "Toto pridal kolega." > kolega.txt # kolega prida novy subor
git add kolega.txt # staging
git commit -m "feat: add colleague file" # commit v druhom clone
git push origin main # kolega pushne zmenu na GitHub

cd .. # vratime sa nad obidva clone priecinky
cd povodny-clone # sem daj nazov povodneho priecinka, ktory si videl pri pwd
git status # lokalne zatial nevidime subor kolega.txt
git fetch origin # stiahneme informacie z GitHubu, ale este nemenime working tree
git log --oneline --graph --all --decorate --max-count=20 # vidime, ze origin/main je dalej
git pull origin main # teraz zmenu zapracujeme lokalne
ls # kolega.txt uz je dostupny
cat kolega.txt # ukazeme obsah
```

## Ukazeme odmietnuty push a potom pull

```bash
cd .. # ideme nad obidva clone priecinky
cd kolega-copy # ideme do druheho clone
echo "Remote zmena pred mojim pushom." > remote-first.txt # kolega spravi dalsiu zmenu
git add remote-first.txt # staging
git commit -m "feat: add remote first file" # commit u kolegu
git push origin main # kolega pushne skor

cd .. # ideme nad obidva clone priecinky
cd povodny-clone # vratime sa do povodneho clone
echo "Lokalna zmena bez predchadzajuceho pullu." > local-after-remote.txt # lokalne spravime vlastnu zmenu
git add local-after-remote.txt # staging
git commit -m "feat: add local file after remote changed" # lokalny commit

git push origin main # toto ma zlyhat, pretoze remote branch ma novy commit
git pull --no-rebase origin main # najprv stiahneme a mergneme remote zmeny
git log --oneline --graph --all --decorate --max-count=25 # ukazeme, ako sa historia spojila
git push origin main # teraz push prejde
```

## Remote branch: kolega pushne branch a my si ho stiahneme

```bash
cd .. # ideme nad obidva clone priecinky
cd kolega-copy # ideme do druheho clone
git pull origin main # kolega si najprv aktualizuje hlavny branch
git checkout -b feature/remote-report # kolega vytvori novy branch

echo "Report vytvoreny na remote branchi." > report.txt # subor na remote branchi
git add report.txt # staging
git commit -m "feat: add remote report" # commit na remote branchi
git push -u origin feature/remote-report # push noveho branchu na GitHub

cd .. # ideme nad obidva clone priecinky
cd povodny-clone # spat do povodneho clone
git fetch origin # stiahneme informacie o novom remote branchi
git branch -a # zobrazime local aj remote branches
git checkout feature/remote-report # Git vytvori local branch podla remote branchu
ls # report.txt je viditelny na tomto branchi
git status # branch sleduje origin/feature/remote-report

git checkout main # spat na hlavny branch
git merge feature/remote-report --no-edit # mergneme remote branch do hlavneho branchu
git push origin main # pushneme vysledok
git push origin --delete feature/remote-report # vymazeme remote branch po merge
git branch -d feature/remote-report # vymazeme local branch
```

## Tagy na oznacenie verzie

```bash
git checkout main # tag davame na hlavny branch
git pull origin main # uistime sa, ze sme aktualni

git tag den2-v1 # jednoduchy tag na aktualnom commite
git tag # zobrazime tags
git log --oneline --decorate --max-count=8 # tag vidime pri commite
git show den2-v1 --stat # tag ukazuje na konkretny commit

git tag -a den2-v1-notes -m "Den 2 demo verzia" # annotated tag s metadata
git show den2-v1-notes --stat # ukazeme detail annotated tagu

git push origin den2-v1 # pushneme konkretny tag na GitHub
git push origin den2-v1-notes # pushneme aj annotated tag
```

## Rebase ako upratanie lokalnej feature historie

```bash
git checkout main # zaciname z hlavneho branchu
git pull origin main # hlavny branch musi byt aktualny

git checkout -b feature/rebase-cleanup # vytvarame branch, ktory zatial nebudeme pushovat
echo "rebase krok 1" > rebase-demo.txt # prva zmena na feature branchi
git add rebase-demo.txt # staging
git commit -m "feat: add rebase demo file" # prvy commit na feature branchi

echo "rebase krok 2" >> rebase-demo.txt # druha zmena
git add rebase-demo.txt # staging
git commit -m "feat: extend rebase demo file" # druhy commit na feature branchi

git log --oneline --graph --all --decorate --max-count=25 # pred rebase ukazeme aktualnu historiu

git checkout main # vratime sa na hlavny branch
echo "Novy zaklad na main branchi." > base-after-feature.txt # na main pribudne novy commit
git add base-after-feature.txt # staging
git commit -m "feat: add new base on main" # main je teraz dalej

git log --oneline --graph --all --decorate --max-count=25 # feature branch vychadza zo starsieho bodu
git checkout feature/rebase-cleanup # spat na feature branch
git rebase main # presunieme feature commity na novy zaklad
git log --oneline --graph --all --decorate --max-count=25 # historia je linearnejsia

git checkout main # spat na main
git merge feature/rebase-cleanup --ff-only # po rebase staci fast-forward merge
git branch -d feature/rebase-cleanup # branch je hotovy
git push origin main # posleme vysledok na GitHub
```

Pri rebase povedz nahlas: toto robime na branchi, ktory este nie je zdielany. Nerobime rebase prace, ktoru uz pouzivaju ini ludia.

## Detached HEAD a zachrana commitu cez novy branch

```bash
git checkout main # zaciname z hlavneho branchu
git log --oneline --decorate --max-count=10 # vyberieme si starsi commit
git checkout HEAD~3 # ideme na starsi commit, tym vznikne detached HEAD
git status # Git ukazuje, ze nie sme na branchi

echo "Toto vzniklo v detached HEAD." > detached-demo.txt # spravime zmenu v detached HEAD
git add detached-demo.txt # staging
git commit -m "demo: commit in detached HEAD" # commit existuje, ale nie je na pomenovanom branchi
git log --oneline --decorate --max-count=5 # ukazeme novy commit

git switch -c rescue/detached-demo # zachranime commit tym, ze mu dame branch
git status # teraz uz stojime na normalnom branchi

git checkout main # vratime sa na hlavny branch
git merge rescue/detached-demo --no-edit # mergneme zachraneny commit
git branch -d rescue/detached-demo # upraceme rescue branch
git push origin main # posleme vysledok na GitHub
```

## Zaverecne zopakovanie rozdielov a stavu

```bash
echo "" >> README.md # pridame prazdny riadok
echo "Staged zmena na zaver." >> README.md # pripravime jednu zmenu
git add README.md # dame ju do staging area
echo "" >> README.md # pridame prazdny riadok
echo "Nestaged zmena na zaver." >> README.md # pridame druhu zmenu mimo staging area

git status # ukaze staged aj unstaged zmenu
git diff # ukaze iba unstaged cast
git diff --staged # ukaze staged cast
git diff HEAD # ukaze vsetko oproti poslednemu commitu

git restore --staged README.md # vyberieme README zo staging area
git restore README.md # zahodime demo zmenu vo working tree
git status # cisty working tree

git log --oneline --graph --all --decorate --max-count=30 # finalna historia celeho flow
git branch -a # local aj remote branches
git tag # vytvorene tags
```

## Kratky rytmus, ktory im mozes opakovat cely den

```bash
git status # kde som a co je zmenene
git diff # co som zmenil vo working tree
git add <subor> # co chcem dat do dalsieho commitu
git diff --staged # co presne pojde do commitu
git commit -m "type: message" # ulozeny bod v historii
git log --oneline --graph --all --decorate # kontrola historie
git push # zdielanie na remote
```
