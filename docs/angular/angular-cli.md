# Angular CLI 

@angular/cli 

# Pourquoi le CLI 

* Installation automatique
* Configuration automatique
* Boilerplate
* Environnement dev, de prod et de test
* Génération automatique de fichiers

[Angular](https://angular.io/cli) 

# Création d’un projet 

```bash
npm install @angular/cli -g

ng new <projectname> --inline-template

cd <projectname>

ng add @angular-eslint/schematics

npx typescript-strictly-typed
```

## tsconfig.json 

```typescript
"angularCompilerOptions": {
    "strictInjectionParameters": true,
    "strictInputAccessModifiers": true,
    **"strictTemplates"**: true
}
```

## npm 

Installation des dépendances : 


```bash
npm install
```


Git/SVN

* Exclure `node_modules` 
* Commiter `package.json` et `package-lock.json`

Mise à jours facile grâce au semver : “1.0.0

[npm Docs](https://docs.npmjs.com/) 

## semver 

1.0.1 = **patch** : corrections de bugs, **rétro-compatible** 

1.1.0 = **minor** : nouvelles fonctionnalités, mais **rétro-compatible**

2.0.0 = **major** : changements non rétro-compatible, **mêmes minimes** 

[npm semantic version calculator](https://semver.npmjs.com/) 


