# Opgaveliste – trin for trin

Dette er en Vue 3-side, der bruger Axios til at hente opgaver med **GET** fra JSONPlaceholder og viser dem som responsive Bootstrap-kort. Grøn betyder færdig, grå betyder ikke færdig. Der skal **ikke** laves en API eller database.

## 1. Kør siden på din computer

Du skal have Node.js (version 22.18+ eller 24.12+), npm og senere Git installeret. Åbn PowerShell i mappen med løsningen. Projektet ligger i undermappen `JAVAToDoList`, så kør først:

```powershell
cd .\JAVAToDoList
node --version
npm.cmd --version
npm.cmd install
npm.cmd run dev
```

Hvis terminalen **allerede** er i projektmappen (den med `package.json`), skal du springe `cd` over. Brug `npm.cmd` i PowerShell, fordi `npm` kan blive blokeret af Windows' scriptpolitik. Åbn den lokale adresse, som Vite udskriver (typisk `http://localhost:55523/`). Du bør se overskriften **Opgaveliste** og kort med opgaver. På mobil står kortene i én kolonne. Stop serveren med **Ctrl+C**.

Tjek derefter, at siden kan bygges til produktion:

```powershell
npm.cmd run build
```

En vellykket bygning opretter `dist`. `node_modules` og `dist` er lokale/genererede mapper og skal ikke lægges på GitHub.

## 2. GitHub – repositoryet er forbundet

Projektmappen er allerede sat op med Git og `origin` peger på **https://github.com/VikkerGH/OpgaveListe**. Du skal **ikke** oprette et nyt repository eller køre `git init`/`git remote add` igen. Når filer skal sendes til GitHub, kræver det, at du logger ind på din GitHub-konto, hvis du bliver bedt om det.

Hvis Visual Studio viser projektet i **Git Changes** (menuen **View → Git Changes**), kan du skrive en kort besked, fx `Opdater opgaveliste`, vælge **Commit All** og derefter **Push**. Hvis Git Changes ikke viser projektet, kan du bruge terminalen i stedet: Løsningen ligger én mappe over Git-projektet, så skift først til undermappen med `package.json`:

```powershell
cd .\javatodolist
git status
git add .
git commit -m "Opdater opgaveliste"
git push
```

Kør `cd` **kun**, hvis terminalen står i mappen med `JAVAToDoList.slnx`. Står du allerede i mappen med `package.json`, starter du ved `git status`. Kommandoerne betyder:

| Kommando | Hvad den gør |
| --- | --- |
| `cd .\javatodolist` | Går ind i projektmappen. |
| `git status` | Viser ændringer; her kan du tjekke, hvad du er ved at sende. |
| `git add .` | Vælger ændrede projektfiler til næste gemning. |
| `git commit -m "..."` | Gemmer et lokalt øjebliksbillede med en beskrivelse. |
| `git push` | Sender dine gemte ændringer til GitHub. |

Hvis du får fejlen om manglende upstream ved den **første** push, brug `git push -u origin main` i stedet. Godkend GitHub-login i browseren, hvis det vises. Opdatér derefter https://github.com/VikkerGH/OpgaveListe og se, om `src`, `package.json` og `README.md` er der. `node_modules`, `dist` og `obj` er genererede mapper og bliver ikke sendt med.

## 3. Læg siden på Azure – du skal bruge din konto

1. Log ind på [Azure-portalen](https://portal.azure.com/). Du skal have et Azure-abonnement, som du må oprette ressourcer i.
2. Vælg **Create a resource** → søg efter **Static Web App** → **Create**. Vælg dit abonnement og en resource group (opret en ny, hvis du ikke har en passende), og giv appen et navn. Vælg **Free**, hvis den er tilgængelig.
3. Under **Deployment details** vælger du **GitHub**. Giv Azure adgang til din GitHub-konto, og vælg det repository, du oprettede ovenfor, samt branch **main**.
4. Under **Build details** vælges **Vue** som build preset (eller **Custom**, hvis Vue ikke findes). Brug disse værdier:

   | Felt | Værdi |
   | --- | --- |
   | App location | `/` |
   | API location | Lad feltet være tomt |
   | Output location | `dist` |

5. Vælg **Review + create** → **Create**. Azure tilføjer automatisk en GitHub Actions-workflow til repositoryet. Gå til fanen **Actions** i GitHub, og vent på en grøn/vellykket kørsel.
6. Åbn din Static Web App i Azure-portalen og klik på dens URL. Du bør se den samme opgaveliste som lokalt. Efter senere kodeændringer: `git add .`, `git commit -m "Beskriv ændringen"` og `git push`; GitHub Actions udgiver siden igen.

## Hvis noget ikke virker

- **`npm.ps1 cannot be loaded`**: brug `npm.cmd`, som i kommandoerne ovenfor. Du behøver ikke ændre PowerShells sikkerhedsindstillinger.
- **`git` findes ikke**: installér Git for Windows og åbn terminalen igen.
- **`git push` afvises**: kontrollér repositoryets URL med `git remote -v`, og fuldfør GitHub-login. Del aldrig adgangskoder eller tokens i kode/repository.
- **Azure-siden er tom eller workflowet er rødt**: se fejlen under GitHubs **Actions**-fane. Kontrollér, at app location er `/`, API location er tom, og output location er `dist`.
- **Kortene kan ikke indlæses**: tjek internetforbindelsen og om JSONPlaceholder svarer. Brug knappen **Prøv igen** på siden. Opgaverne hentes i browseren; hverken GitHub eller Azure gemmer dem.
