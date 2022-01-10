# PrimeNG

# Installation

```json
npm install primeng --save
npm install primeicons --save
npm install primeflex --save
```

# Définition des styles CSS

## Dans le fichier angular.json

```json
"styles": [
	"src/styles.scss",
	"node_modules/primeng/resources/themes/mdc-light-indigo/theme.css",
	"node_modules/primeng/resources/primeng.min.css",
	"node_modules/primeicons/primeicons.css",
	"node_modules/primeflex/primeflex.css"
],
```

## Dans le fichier style.css

```css
body {
    font-family: var(--font-family);
    background-color: #eff3f8;
    color: var(--text-color);
    min-height: 100%;
    -webkit-font-smoothing: antialiased;
}
```

# Angular CDK

Pour bien faire fonctionner les “tables” de PrimeNG, qui utilisent le VirtualScrolling, il faut installer le Component Dev Kit (CDK)

```bash
npm install @angular/cdk --save
```


# Traduction

Pour mettre en place la traduction des textes de certains éléments de PrimeNG, il y a 2 méthodes possibles

* via l’API de PrimeNG
* via ngx-translate


Dans tous les cas, se référer à la documentation officielle : <https://www.primefaces.org/primeng/showcase/#/i18n>

## Via l’API de PrimeNG

Dans ce cas, on définit les traductions directement dans la classe “AppComponent” et on ne peut pas utiliser plusieurs langues. 

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';
import { PrimeNGConfig } from 'primeng/api';

@Component({
    selector: 'app-root',
    templateUrl: './app.component.html'
})
export class AppComponent implements OnInit, OnDestroy {

    constructor(private config: PrimeNGConfig) {}

    ngOnInit() {
        this.config.setTranslation({
            accept: 'Accept',
            reject: 'Cancel',
            //translations
        });
    }
}
```


## Via npx-translate 

Dans ce cas, on utilise des fichiers de traduction (un par langue). 

Cela est plus complexe à mettre en place, mais cela reste plus propre et plus dynamique (il est possible de changer de langue à n’importe quel moment dans le cycle de vie de l’application).


Documentations : 

* <https://github.com/ngx-translate/core>
* <https://dev.to/yigitfindikli/primeng-i18n-api-usage-with-ngx-translate-3bh2>


### Installation des packages nécessaires

```typescript
npm install @ngx-translate/core --save
npm install @ngx-translate/http-loader --save
```

Le package @ngx-translate/http-loader permet le chargement des fichiers de traduction en HTTP

## Mise en place dans le projet Angular

### Fichier app.module.ts

Modifier le fichier pour y ajouter

* la fonction HttpLoaderFactory
* le TranslateModule dans les imports

  \

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClient } from '@angular/common/http';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { AuthInterceptor } from '@core/auth';
import { TranslateLoader, TranslateModule } from '@ngx-translate/core';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

// AoT requires an exported function for factories
export function HttpLoaderFactory(http: HttpClient) : TranslateHttpLoader {
  return new TranslateHttpLoader(http);
}

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    [...]
    TranslateModule.forRoot({
      loader: {
          provide: TranslateLoader,
          useFactory: HttpLoaderFactory,
          deps: [HttpClient]
      }
  })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Création des fichiers de traductions

Dans le répertoire “src/assets”, créer un répertoire “i18n” et y ajouter les fichiers de traductions (un par langue)


Exemple en anglais (fichier en.json)

```json
{    
    "primeng": {
        "startsWith": "Starts with",
        "contains": "Contains",
        "notContains": "Not contains",
        "endsWith": "Ends with",
        "equals": "Equals",
        "notEquals": "Not equals",
        "noFilter": "No Filter",
        "lt": "Less than",
        "lte": "Less than or equal to",
        "gt": "Greater than",
        "gte": "Greater than or equal to",
        "is": "Is",
        "isNot": "Is not",
        "before": "Before",
        "after": "After",
        "dateIs": "Date is",
        "dateIsNot": "Date is not",
        "dateBefore": "Date is before",
        "dateAfter": "Date is after",
        "clear": "Clear",
        "apply": "Apply",
        "matchAll": "Match All",
        "matchAny": "Match Any",
        "addRule": "Add Rule",
        "removeRule": "Remove Rule",
        "accept": "Yes",
        "reject": "No",
        "choose": "Choose",
        "upload": "Upload",
        "cancel": "Cancel",
        "dayNames": ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"],
        "dayNamesShort": ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
        "dayNamesMin": ["Su", "Mo", "Tu", "We", "Th", "Fr", "Sa"],
        "monthNames": ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"],
        "monthNamesShort": ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
        "dateFormat": "mm/dd/yy",
        "firstDayOfWeek": 0,
        "today": "Today",
        "weekHeader": "Wk",
        "weak": "Weak",
        "medium": "Medium",
        "strong": "Strong",
        "passwordPrompt": "Enter a password",
        "emptyMessage": "No results found",
        "emptyFilterMessage": "No results found"
    }
}
```


Exemple en français (fichier fr.json)

```json
{
    "primeng": {
        "startsWith": "Commence par",
        "contains": "Contient",
        "notContains": "Ne contient pas",
        "endsWith": "Termine par",
        "equals": "Egal",
        "notEquals": "Différent",
        "noFilter": "Aucun filtre",
        "lt": "Moins que",
        "lte": "Moins que ou égal",
        "gt": "Plus grand que",
        "gte": "Plus grand que ou égal",
        "is": "Est",
        "isNot": "N'est pas",
        "before": "Avant",
        "after": "Après",
        "dateIs": "Date est",
        "dateIsNot": "Date n'est pas",
        "dateBefore": "Date est avant",
        "dateAfter": "Date est après",
        "clear": "Effacer",
        "apply": "Appliquer",
        "matchAll": "Faire correspondre tout",
        "matchAny": "Correspondre à n'importe quel",
        "addRule": "Ajouter une règle",
        "removeRule": "Supprimer une règle",
        "accept": "Oui",
        "reject": "Non",
        "choose": "Choisir",
        "upload": "Télécharger",
        "cancel": "Annuler",
        "dayNames": [
            "Dimanche",
            "Lundi",
            "Mardi",
            "Mercredi",
            "Jeudi",
            "Vendredi",
            "Samedi"
        ],
        "dayNamesShort": [
            "Dim",
            "Lun",
            "Mar",
            "Mer",
            "Jeu",
            "Ven",
            "Sam"
        ],
        "dayNamesMin": [
            "Di",
            "Lu",
            "Ma",
            "Me",
            "Je",
            "Ve",
            "Sa"
        ],
        "monthNames": [
            "Janvier",
            "Février",
            "Mars",
            "Avril",
            "Mai",
            "Juin",
            "Juillet",
            "Août",
            "Septembre",
            "Octobre",
            "Novembre",
            "Décembre"
        ],
        "monthNamesShort": [
            "Jan",
            "Fév",
            "Mar",
            "Avr",
            "Mai",
            "Jui",
            "Juil",
            "Aoû",
            "Sep",
            "Oct",
            "Nov",
            "Déc"
        ],
        "dateFormat": "dd/mm/yy",
        "firstDayOfWeek": 1,
        "today": "Aujourd'hui",
        "weekHeader": "Sem",
        "weak": "Faible",
        "medium": "Moyen",
        "strong": "Fort",
        "passwordPrompt": "Saisir un mot de passe",
        "emptyMessage": "Aucun résultat trouvé",
        "emptyFilterMessage": "Aucun résultat trouvé - Filtre"
    }
}
```


### Fichier app.component.ts

Modifier le fichier pour :

* déclarer la variable “subscription” (de type “Subscription”)
* modifier le constructeur 

  \

```typescript
import { Component, OnDestroy } from '@angular/core';
import { TranslateService } from '@ngx-translate/core';
import { PrimeNGConfig, Translation } from 'primeng/api';
import { Subscription } from 'rxjs';

@Component({
  selector: 'app-root',
  template: `    
    <router-outlet></router-outlet>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnDestroy {
  title = 'Front';

  subscription: Subscription;

  constructor(private config: PrimeNGConfig, private translateService: TranslateService) {

    // Définition des langues disponibles
    translateService.addLangs(['en', 'fr']);

    // Définition de la langue par défaut
    translateService.setDefaultLang('fr');

    this.subscription = translateService.stream('primeng').subscribe(data => {
      config.setTranslation(data as Translation);
    });

  }

  ngOnDestroy(): void {
    if (this.subscription) {
      this.subscription.unsubscribe();
    }
  }
}
```


Pour utiliser et changer de langue dans l’application, il suffit d’appeler la méthode “use” du “translateService” 

```typescript
translateService.use('en');
translateService.use('fr');
[...]
```


