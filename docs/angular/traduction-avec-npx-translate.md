# Traduction avec npx-translate

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
* le TranslateStore dans les providers

  \

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClient } from '@angular/common/http';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { AuthInterceptor } from '@core/auth';
import { TranslateLoader, TranslateModule, TranslateStore } from '@ngx-translate/core';
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
  providers: [
    TranslateStore
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


### Dans chaque module

Adapter le fichier du module (xxx.module.ts) de la même façon que dans “app.module.ts”, si ce n’est que dans la section “imports”, on utilise “forChild(…)” au lieu de “forRoot(…)”

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { TranslateLoader, TranslateModule } from '@ngx-translate/core';
import { HttpClient } from '@angular/common/http';
import { TranslateHttpLoader } from '@ngx-translate/http-loader';

// AoT requires an exported function for factories
export function HttpLoaderFactory(http: HttpClient): TranslateHttpLoader {
  return new TranslateHttpLoader(http);
}

@NgModule({
  declarations: [
    LogComponent
  ],
  imports: [
    CommonModule,
    [...]
    TranslateModule.forChild({
      loader: {
        provide: TranslateLoader,
        useFactory: HttpLoaderFactory,
        deps: [HttpClient]
      }
    })
  ]
})
export class LogModule { }
```

### Création des fichiers de traductions

Dans le répertoire “src/assets”, créer un répertoire “i18n” et y ajouter les fichiers de traductions (un par langue)


Exemple en anglais (fichier en.json)

```typescript
{
  "demo": {
    "greetings": "Hello!",
    "en": "English",
    "fr": "French"
  }
}
```


Exemple en français (fichier fr.json)

```typescript
{
  "demo": {
    "greetings": "Bonjour!",
    "en": "Anglaise",
    "fr": "Français"
  }
}
```


Dans un template HTML, pour écrire du texte qui sera traduit selon la langue choisie (voir section ci-dessous), il suffit de faire

```typescript
<h2 class="p-ml-3">{{ 'demo.greetings' | translate }}</h2>
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


