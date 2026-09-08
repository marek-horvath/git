# Git tahak ku skoleniu

Tento subor je rychly prehlad prikazov zo skolenia. Prikazy spustaj v **Git Bash** a pri svojom repository pocitaj s branchom `main`; ak mas `master`, v prikazoch nahrad `main` za `master`.

## Materialy

- Laby: https://docs.google.com/document/d/1wYdyXlDaZp5dxQsjYPekAp0oWlS-0M5kZOmnHdtCmhg/edit?tab=t.0
- GitHub repository k labom: https://github.com/marek-horvath/git
- Prezentacia: https://docs.google.com/presentation/d/19eKB1li-AfdSF1fi6XH87tzzdozuX62jY8sfUSzZBB0/edit?usp=sharing
- SSH navod: https://kurzy.kpi.fei.tuke.sk/zap/tutorials/version-control.html

## Najdolezitejsi rytmus

```bash
git status
git diff
git add <subor>
git diff --staged
git commit -m "feat: kratka sprava"
git push origin main
```

Toto je zakladny cyklus: najprv pozri stav, potom rozdiely, priprav zmeny, skontroluj staged diff, uloz commit a posli ho na GitHub.

## Terminal

| Prikaz | Vysvetlenie |
|---|---|
| `pwd` | Ukaze, v ktorom priecinku sa prave nachadzas. |
| `ls` | Vypise subory a priecinky v aktualnom priecinku. |
| `ls -a` | Vypise aj skryte veci, napriklad `.git`. |
| `cd nazov-priecinka` | Prejde do konkretneho priecinka. |
| `cd ..` | Prejde o jeden priecinok vyssie. |
| `mkdir nazov` | Vytvori novy priecinok. |
| `cat subor.txt` | Vypise obsah suboru priamo do terminala. |
| `echo "text" > subor.txt` | Vytvori alebo prepise subor jednym riadkom textu. |
| `echo "text" >> subor.txt` | Prida text na koniec existujuceho suboru. |

## Zakladne pojmy

| Pojem | Vysvetlenie |
|---|---|
| `working tree` | Aktualny stav suborov v tvojom priecinku. |
| `staging area` | Miesto, kam das zmeny cez `git add`, aby isli do dalsieho commitu. |
| `repository` | Git historia projektu ulozena v priecinku `.git`. |
| `.git` | Skryty priecinok, v ktorom Git drzi celu historiu a nastavenia repository. |
| `.gitignore` | Subor s pravidlami, ktore subory Git nema sledovat. |
| `HEAD` | Oznacenie commitu, na ktorom prave stojis. |
| `origin` | Bezny nazov remote repository, odkial si robil clone alebo kam pushujes. |
| `origin/main` | Stav branchu `main` na remote repository. |
| `merge conflict` | Situacia, ked Git nevie automaticky spojit dve zmeny na rovnakom mieste. |
| `detached HEAD` | Stav, ked stojis priamo na commite a nie na pomenovanom branchi. |

## Instalacia a konfiguracia

| Prikaz | Vysvetlenie |
|---|---|
| `git --version` | Overi, ci mas Git nainstalovany a aku verziu pouzivas. |
| `git config --global user.name "Meno Priezvisko"` | Nastavi meno autora commitov na tomto pocitaci. |
| `git config --global user.email "meno@example.com"` | Nastavi email autora commitov na tomto pocitaci. |
| `git config --list` | Vypise aktualne Git nastavenia. |
| `git config --global --list` | Vypise globalne Git nastavenia pre tvojho pouzivatela. |
| `ssh-keygen` | Vytvori SSH kluc, ktory potom pridas na GitHub. |
| `cat ~/.ssh/id_ed25519.pub` | Vypise public SSH kluc, ktory sa kopiruje na GitHub. |

## Vytvorenie a clone repository

| Prikaz | Vysvetlenie |
|---|---|
| `git init` | Vytvori nove lokalne Git repository v aktualnom priecinku. |
| `git branch -M main` | Premenuje aktualny branch na `main`. |
| `git clone git@github.com:meno/repo.git` | Stiahne remote repository z GitHubu cez SSH. |
| `git remote -v` | Ukaze, na ake remote URL je repository napojene. |
| `git remote add origin git@github.com:meno/repo.git` | Prida remote repository pod nazvom `origin`. |

## Stav, diff a staging

| Prikaz | Vysvetlenie |
|---|---|
| `git status` | Ukaze, na ktorom branchi si a ake subory su zmenene, staged alebo untracked. |
| `git diff` | Ukaze zmeny vo working tree, ktore este nie su v staging area. |
| `git diff --staged` | Ukaze zmeny, ktore su pripravene na commit. |
| `git diff HEAD` | Ukaze vsetky aktualne zmeny oproti poslednemu commitu. |
| `git add subor.txt` | Prida jeden konkretny subor do staging area. |
| `git add .` | Prida vsetky aktualne zmeny v priecinku do staging area. |
| `git restore subor.txt` | Zahodi necommitnutu zmenu v danom subore. |
| `git restore --staged subor.txt` | Vyberie subor zo staging area, ale zmenu necha v subore. |

## Commit a historia

| Prikaz | Vysvetlenie |
|---|---|
| `git commit -m "feat: sprava"` | Vytvori commit zo staged zmien s kratkou spravou. |
| `git commit subor.txt -m "sprava"` | Commitne tracked subor priamo bez samostatneho `git add`. |
| `git log` | Zobrazi historiu commitov. |
| `git log --oneline` | Zobrazi historiu commitov v kratkom formate. |
| `git log --oneline --graph --all --decorate` | Ukaze historiu ako graf aj s branches a tags. |
| `git show HEAD` | Ukaze detail posledneho commitu. |
| `git show <hash>` | Ukaze detail konkretneho commitu podla hashu. |
| `git show <tag>` | Ukaze commit alebo anotovany tag, na ktory tag ukazuje. |
| `git revert HEAD` | Vytvori novy commit, ktory zrusi zmeny z posledneho commitu. |

## Praca so subormi v Gite

| Prikaz | Vysvetlenie |
|---|---|
| `git rm subor.txt` | Vymaze subor a pripravi toto vymazanie do staging area. |
| `git rm --cached subor.txt` | Prestane sledovat subor v Gite, ale necha ho fyzicky na disku. |
| `git mv povodny.txt novy.txt` | Premenuje alebo presunie subor tak, aby to Git videl ako jednu zmenu. |
| `git reset --hard` | Zahodi vsetky necommitnute zmeny vo working tree aj staging area. |

`git reset --hard` pouzivaj iba v demo repository alebo ked presne vies, co robis, lebo vie zahodit rozpracovanu pracu.

## Branches

| Prikaz | Vysvetlenie |
|---|---|
| `git branch` | Vypise lokalne branches a hviezdickou oznaci aktualny branch. |
| `git branch -a` | Vypise lokalne aj remote branches. |
| `git checkout main` | Prepne working tree na branch `main`. |
| `git checkout -b feature/nazov` | Vytvori novy branch a rovno sa na neho prepne. |
| `git switch main` | Novsi sposob prepnutia na branch `main`. |
| `git switch -c feature/nazov` | Novsi sposob vytvorenia a prepnutia na novy branch. |
| `git merge feature/nazov` | Spoji zmeny z daneho branchu do aktualneho branchu. |
| `git merge feature/nazov --no-edit` | Spravi merge a pouzije defaultnu merge spravu. |
| `git branch -d feature/nazov` | Vymaze lokalny branch, ktory uz bol merged. |
| `git branch -D feature/nazov` | Vynutene vymaze lokalny branch aj bez merge. |

Pred `merge` si vzdy skontroluj, na ktorom branchi stojis, lebo merge sa robi **do aktualneho branchu**.

## Merge conflict

| Prikaz | Vysvetlenie |
|---|---|
| `git merge feature/konflikt` | Spusti merge a pri rozdielnych zmenach na rovnakom mieste moze vytvorit conflict. |
| `git status` | Pri conflicte ukaze subory, ktore treba rucne vyriesit. |
| `cat konflikt.txt` | Rychlo ukaze conflict markers priamo v terminali. |
| `git diff` | Pri conflicte ukaze, co Git nevie automaticky spojit. |
| `git add konflikt.txt` | Oznaci subor ako vyrieseny po rucnej uprave. |
| `git commit -m "merge: resolve conflict"` | Dokonci merge po vyrieseni conflictu. |

Conflict markers `<<<<<<<`, `=======` a `>>>>>>>` musia zo suboru zmiznut skor, nez conflict dokoncis commitom.

## Remote, fetch, pull a push

| Prikaz | Vysvetlenie |
|---|---|
| `git push origin main` | Posle lokalne commity z branchu `main` na GitHub. |
| `git push` | Posle commity na nastavene upstream remote branch. |
| `git push -u origin feature/nazov` | Pushne novy branch na GitHub a nastavi upstream. |
| `git fetch origin` | Stiahne informacie z GitHubu, ale este nemeni tvoje subory. |
| `git pull origin main` | Stiahne zmeny z GitHubu a zapracuje ich do aktualneho branchu. |
| `git pull --no-rebase origin main` | Stiahne remote zmeny a spoji ich cez merge commit. |
| `git push origin --delete feature/nazov` | Vymaze remote branch na GitHube. |

`fetch` je iba nacitanie informacii z remote, `pull` uz meni tvoje lokalne repository.

## Stash

| Prikaz | Vysvetlenie |
|---|---|
| `git stash` | Docasne odlozi rozrobene zmeny, aby bol working tree cisty. |
| `git stash push -m "WIP sprava"` | Odlozi zmeny do stashu aj s kratkym popisom. |
| `git stash list` | Ukaze ulozene stash zmeny. |
| `git stash pop` | Vrati posledny stash naspat do working tree a odstrani ho zo stash listu. |

Stash je dobry, ked mas rozrobenu pracu a potrebujes sa rychlo prepnut na iny branch alebo hotfix.

## Tags

| Prikaz | Vysvetlenie |
|---|---|
| `git tag v1.0.0` | Vytvori jednoduchy tag na aktualnom commite. |
| `git tag` | Vypise vsetky lokalne tags. |
| `git tag -a v1.0.1 -m "Release v1.0.1"` | Vytvori annotated tag so spravou. |
| `git show v1.0.0 --stat` | Ukaze, na aky commit tag ukazuje a ake subory sa tam menili. |
| `git push origin v1.0.0` | Pushne konkretny tag na GitHub. |

Tag sa pouziva na oznacenie doleziteho bodu v historii, napriklad release verzie.

## Rebase

| Prikaz | Vysvetlenie |
|---|---|
| `git rebase main` | Presunie commity aktualneho branchu tak, aby vychadzali z najnovsieho `main`. |
| `git merge feature/nazov --ff-only` | Po uspesnom rebase prida branch do `main` iba fast-forward sposobom. |

Rebase rob iba na svojom lokalnom branchi, ktory este nepouzivaju ini ludia.

## Detached HEAD

| Prikaz | Vysvetlenie |
|---|---|
| `git checkout <hash>` | Prepne repository na konkretny starsi commit. |
| `git checkout HEAD~3` | Prepne repository tri commity dozadu od aktualneho miesta. |
| `git switch -c rescue/nazov` | Vytvori branch na aktualnom mieste a vie zachranit pracu z detached HEAD. |
| `git checkout main` | Vrati ta spat na normalny branch. |

V detached HEAD si vies pozriet alebo vyskusat starsi stav, ale normalnu pracu si radsej zachran cez novy branch.

## Typicke flow

### Nova zmena na main

```bash
git checkout main
git pull origin main
git status
git add .
git commit -m "feat: kratka sprava"
git push origin main
```

Toto pouzi pri malej zmene, ktoru robis priamo na hlavnom branchi.

### Nova feature branch

```bash
git checkout main
git pull origin main
git checkout -b feature/nazov
git add .
git commit -m "feat: kratka sprava"
git push -u origin feature/nazov
```

Toto pouzi, ked chces pracovat bokom a neskor branch mergnut.

### Merge feature branchu

```bash
git checkout main
git pull origin main
git merge feature/nazov --no-edit
git push origin main
git branch -d feature/nazov
```

Toto pouzi, ked je feature branch hotovy a chces ho dostat do `main`.

### Remote zmena od kolegu

```bash
git fetch origin
git log --oneline --graph --all --decorate --max-count=20
git pull origin main
```

Toto pouzi, ked chces najprv vidiet, co je nove na GitHube, a az potom to stiahnut do working tree.

### Push zlyha, lebo remote je dalej

```bash
git push origin main
git pull --no-rebase origin main
git push origin main
```

Toto je bezna situacia, ked niekto pushol skor ako ty a najprv musis zapracovat jeho zmeny.

### Hotfix pocas rozrobenej prace

```bash
git status
git stash push -m "WIP rozrobena praca"
git checkout main
git checkout -b hotfix/nazov
git add .
git commit -m "fix: kratka sprava"
git checkout main
git merge hotfix/nazov --no-edit
git stash pop
```

Toto pouzi, ked nechces commitnut rozrobenu pracu, ale potrebujes rychlo riesit inu zmenu.

## Proxy v Git Bash

Ak ste v sieti, kde GitHub nejde priamo, moze byt potrebne nastavit proxy:

```bash
export HTTP_PROXY=http://sia.telekom.de:8080/
export HTTPS_PROXY=http://sia.telekom.de:8080/
```

Toto nastavenie plati pre aktualne otvoreny Git Bash terminal.
