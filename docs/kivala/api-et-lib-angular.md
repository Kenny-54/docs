# API et librairie Angular

# Librairie Kivala

## Build

A la racine du projet, lancer `ng build kivala`

## Packaging

A la racine du projet, lancer `npm pack dist\kivala` sous Windows ou `npm pack dist/kivala/` sous Linux/MacOs

# Testapp

Application permettant de tester la librairie Kivala.

Pour débugger la librairie en même temps que l’appli de test, lancer dans un terminal :

```bash
ng build kivala --watch
```

et dans un autre exécuter l’application de test

# Dans une application Angular

## Installation de la librairie

Copier le fichier cd54-kivala-x.x.x.tgz puis lancer (où "x.x.x" est le n° de version)

```bash
npm i cd54-kivala-x.x.x.tgz
```

## Mise à jour de la libraire

Supprimer le fichier .tgz de l’ancienne version et copier celui de la nouvelle version puis lancer 

```bash
npm i cd54-kivala-x.x.x.tgz
```


## Mise en place dans l’application

### Dans app.module.ts

Dans l’import du module, il faut fournir l’URL de l’API Kivala ainsi que le nom de l’application

```javascript
[...]
import { AuthInterceptor, AuthModule } from '@cd54/kivala';
import { environment } from '@core/environment'

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    [...]
    AuthModule.forRoot(
      environment.kivalaApiUrl,
      environment.applicationName)    
  })
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```


### Paramètres

Dans environment.ts

```javascript
export const environment = {
  [...]
  applicationName: '<nom_de_l_appli>',
  kivalaApiUrl : 'https://localhost:7285'
};
```

### Protection des routes

Dans app-routing.module.ts

```javascript
const authGuardData = {
  loginUrl: '/login',
  apiUrl: environment.kivalaApiUrl
};

const routes: Routes = [
  [...]
  { path: 'log', loadChildren: () => import('./features/+log/log.module').then(m => m.LogModule), canActivate: [AuthGuard], data: authGuardData },
];
```


### Page de connexion

Créer une page de connexion avec un formulaire contenant un champ pour le nom d’utilisateur et le mot de passe.

La soumission du formulaire doit appeler la méthode `login` du service `LoginService` (inclus dans la librairie) en lui fournissant un objet de type `LoginRequestBody`

```javascript
LoginRequestBody = {
    username: '',
    password: '',
    applicationname: ''
  };
```

Cet objet doit contenir les données du formulaire.

Le nom de l’application n’est pas à utiliser ici, car elle est déjà définie avant lors de la mise en place dans les routes (voir ci-dessus)


:::warning
Revoir la librairie pour que la méthode login() ait le nom d’utilisateur et le mot de passe directement en paramètres et non plus un objet LoginRequestBody car c’est ce dernier qui porte le nom de l’application, mais il n’est pas transmis lors de l’appel de la méthode

:::


