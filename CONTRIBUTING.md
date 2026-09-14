## Engangsopsætning (én gang pr. person)

```bash
git clone https://github.com/DanniTheAsian/hr-teamproject.git
cd hr-teamproject
python -m venv .venv                 # opretter et virtuelt miljø til projektet
source .venv/Scripts/activate        # aktiverer det (Git Bash på Windows)
pip install -r requirements.txt      # installerer de pakker, projektet bruger
```

Næste gang du åbner projektet, skal du kun køre `source .venv/Scripts/activate`.

## Modellen

- **`main`** er den fælles sandhed. Den skal altid virke. Ingen redigerer den direkte.
- **En branch** er din egen lokale arbejdsopgave, taget fra `main`. Intet du laver
  på en branch påvirker andre, før den bliver merget.
- **En pull request (PR)** er måden en branch bliver gennemset og merget ind i `main`.

```
   main   ●────────────────────────────●─────────  ← fælles sandhed, virker altid
           \                          /
 din branch ●──●──●──────────────────   (merges via PR)
              dit private arbejde her
```

## Navngivning af brancher
 
Brug formen `<type>/<navn>` med små bogstaver og bindestreger. Undgå mellemrum og æ, ø, å.
 
| Type        | Bruges til                  | Eksempel                   |
|-------------|-----------------------------|----------------------------|
| `feature/`  | Ny funktionalitet           | `feature/...` |
| `fix/`      | Rettelse af en fejl         | `fix/...`           |
| `docs/`     | Kun dokumentation           | `docs/...`        |               |


## Lav en ny branch og få merget

1. **Start fra det nyeste**
```bash
   git checkout main && git pull
```

2. **Lav en ny branch**.
```bash
   git checkout -b <type>/<navn>
```

3. **Arbejd i din egen branch.** Lav dine ændringer og commit ofte.

4. **Commit**
```bash
   git add .
   git commit -m "Tydelig besked om hvad ændringen gør"
```

5. **Push**
```bash
   git push -u origin <type>/<navn>
```

6. **Pull request.** Åbn en PR på GitHub fra din branch til `main`, og tilføj en anden contributor som reviewer.

7. **Merge.** Når PR'en er godkendt, merger revieweren branchen ind i `main`.

8. **Ryd op**
```bash
   git checkout main && git pull
   git branch -d <type>/<navn>
```

## Hold din branch opdateret

Mens din branch er åben, bevæger `main` sig ¨. Din branch opdager det ikke selv. 

- **Start friskt:** `git checkout main && git pull` før du opretter en branch.
- **Opdatér en længerevarende branch** ved at hente `main` ind i den hver dag eller to:

```bash
git checkout main && git pull      # hent alles mergede arbejde
git checkout <type>/<navn>         # tilbage til din branch
git merge main                     # tag main's ændringer med ind
```

## Merge-konflikter — hvad de er, og hvordan de løses

### Hvad en konflikt egentlig er

En konflikt opstår **kun når to brancher ændrer de samme linjer i den samme
fil.** D

En konflikt er **ikke** en fejl eller et ødelagt repo. Git siger bare: "to
personer har ændret det samme sted." 
### Sådan ser det ud

Når du kører `git merge main` (eller GitHub ikke kan merge din PR automatisk),
markerer Git stedet i filen sådan her:

```
<<<<<<< HEAD
const RETRIES = 3;            ← din version (den branch du står på)
=======
const RETRIES = 5;            ← deres version (fra main)
>>>>>>> main
```

De tre markører betyder: `<<<<<<< HEAD` starter *din* ændring, `=======`
adskiller, `>>>>>>> main` afslutter *deres* ændring.

### Sådan løser du den — trin for trin

```
 1. se hvad der konflikter      git status         ← viser filer som "both modified"
 2. åbn hver af de filer og find markørerne <<<<<<< ======= >>>>>>>
 3. bestem den endelige tekst behold din, behold deres, eller kombinér — og SLET derefter alle tre markørlinjer
 4. markér som løst             git add <filen>
 5. afslut merge                git commit          (eller: git merge --continue)
```
### Gør det i en editor (nemmere end i hånden)

- **VS Code** fremhæver hver konflikt og viser knapper: **Accept Current** (din),
  **Accept Incoming** (deres), **Accept Both**, eller redigér selv. Klik det
  rigtige pr. konflikt, gem, og kør så `git add` + `git commit`.
- **PyCharm** åbner en konfliktdialog (ellers find den under **Git → Resolve Conflicts**). Vælg **Accept Yours** (din), **Accept Theirs** (deres) eller **Merge...** for at få tre paneler: din version til venstre, deres til højre og resultatet i midten. Klik **Apply**, når du er færdig. PyCharm markerer selv filen som løst, så du skal kun committe bagefter
- **GitHub i browseren:** Har en PR konflikter, er der en **Resolve conflicts**-knap, som åbner filen med markørerne i en simpel editor. Den er fin til små konflikter.

### Hvis det går galt 

Er du midt i en merge og forvirret, kan du altid fortryde og starte forfra:

```bash
git merge --abort      # annullerer merge og går tilbage til før du startede
```

Intet er committet før du selv siger til.