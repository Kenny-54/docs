# Projet Angular : étapes à suivre

# Bien démarrer

## Création du projet

```bash
npm install @angular/cli -g
ng new *projectname* --inline-template
# accepter la création de route puis choisir scss ou css

cd *projectname*

ng add @angular-eslint/schematics

npx typescript-strictly-typed
```

## Ajout d'ESLint

.eslintrc.json type à copier dans le projet

```json
{
  "root": true,
  "ignorePatterns": [
    "projects/**/*",
    "cypress/**/*"
  ],
  "overrides": [
    {
      "files": [
        "*.ts"
      ],
      "parserOptions": {
        "project": [
          "tsconfig.json"
        ],
        "createDefaultProgram": true
      },
      "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@typescript-eslint/recommended-requiring-type-checking",
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {
        // Delete this one in a real project, just enabled for training purposes
        //"@typescript-eslint/no-unused-vars": "off",
        // Good practice to differentiate component types
        "@angular-eslint/component-class-suffix": [
          "error",
          {
            "suffixes": [
              "Component",
              "Page"
            ]
          }
        ],
        // Prefixes
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ],
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        // Strict types
        "@typescript-eslint/no-explicit-any": "error",
        "@typescript-eslint/explicit-module-boundary-types": "error",
        // Stricter Angular ESLint rules
        "@angular-eslint/contextual-decorator": "error",
        "@angular-eslint/no-attribute-decorator": "error",
        "@angular-eslint/no-forward-ref": "error",
        "@angular-eslint/no-input-prefix": "error",
        "@angular-eslint/no-lifecycle-call": "error",
        "@angular-eslint/no-pipe-impure": "error",
        "@angular-eslint/no-queries-metadata-property": "error",
        "@angular-eslint/use-component-view-encapsulation": "error",
        "@angular-eslint/use-injectable-provided-in": "error",
        // Annoying as Angular CLI generates empty constructors and `ngOnInit()`
        "@typescript-eslint/no-empty-function": "off",
        "@angular-eslint/no-empty-lifecycle-method": "off",
        // Allow Angular forms validators like `Validator.required`
        "@typescript-eslint/unbound-method": [
          "error",
          {
            "ignoreStatic": true
          }
        ],
        // Disallow some imports to enforce architecture good practices
        "no-restricted-imports": [
          "error",
          {
            "paths": [
              // Disallow imports from entry points inside a module (`.` = `./index`)
              ".",
              "..",
              "../..",
              "../../.."
            ],
            "patterns": [
              // Disallow imports forbidden folders
              "dist/*",
              "cypress/*",
              "playwright/*",
              // Disallow imports from another application and direct imports from libraries
              "projects/*",
              // Disallow absolute imports to force either a relative import or a @shortcut import
              "src",
              // Disallow imports from raw entry points to force usage of @shortcut imports
              "index",
              // Disallow direct imports from a core module to force usage of @shortcut imports
              "core-*",
              // Disallow imports from a lazy-loaded module, otherwise lazy-loading is broken
              "+*"
            ]
          }
        ],
        // Formatting rules (can be removed when using a dedicated tool like Prettier)
        "quotes": "off",
        "@typescript-eslint/quotes": [
          "warn",
          "single",
          {
            "allowTemplateLiterals": true
          }
        ]
      }
    },
    {
      "files": [
        "*.html"
      ],
      "extends": [
        "plugin:@angular-eslint/template/recommended"
      ],
      "rules": {
        // Strict types
        "@angular-eslint/template/no-any": "error",
        // Stricter Anguler ESLint rules
        "@angular-eslint/template/conditional-complexity": "error",
        "@angular-eslint/template/cyclomatic-complexity": "error",
        "@angular-eslint/template/no-call-expression": "error",
        "@angular-eslint/template/no-duplicate-attributes": "error",
        "@angular-eslint/template/eqeqeq": "error",
        // Accessibility
        "@angular-eslint/template/accessibility-alt-text": "error",
        "@angular-eslint/template/accessibility-elements-content": "error",
        "@angular-eslint/template/accessibility-table-scope": "error",
        "@angular-eslint/template/accessibility-valid-aria": "error",
        "@angular-eslint/template/click-events-have-key-events": "error",
        "@angular-eslint/template/mouse-events-have-key-events": "error",
        "@angular-eslint/template/no-autofocus": "error",
        "@angular-eslint/template/no-distracting-elements": "error",
        "@angular-eslint/template/no-positive-tabindex": "error",
        "@angular-eslint/template/accessibility-label-for": "error",
        "@angular-eslint/template/accessibility-label-has-associated-control": "error"
      }
    }
  ]
}
```

## Activation du mode strict total

```bash
npx typescript-strictly-typed
```

# Création des répertoires de base

(dans **src/app)**

* core-declarations
* core-services
* features

# Utilisation d’API : classe “APIData”

Afin d’utiliser proprement les éventuelles API, on va utiliser une structure d’échange des données entre les API et l’application Angular.

Pour cela, créer un répertoire “api” (dans src/app/core-services) et y ajouter les 2 fichiers suivants 


* api-data.model.ts

  ```json
  export interface APIData<T> {
    data: T;
    error?: {
      message: string;
      code?: number;
      errors?: string[];
    };
  }
  ```

  \
* index.ts

  ```json
  export { APIData } from './api-data.model';
  ```

# Définition de “paths”

Dans la section “compilerOptions” du fichier tsconfig.json

```json
"paths": {
  "@core/environment": [
	"src/environments/environment"
  ],
  "@core/api": [
	"src/app/core-services/api"
  ]
}
```

Cela permet de simplifier les imports

# Modules de pages

## Etapes à suivre

Ne pas sauter l'étape de conception

Génération en lazy-loading ( -- route)

Au minimum 3 sous-répertoires :

* services
* components
* pages

## Ordre de création


1. le service
2. le composant de présentation
3. la page
4. la route

# **Modules génériques**

A la génération d'un composant, d'une directive ou d'un pipe génériques, penser à utiliser l'option **export**

Un service global doit être injecté :

* Là où j'en ai besoin
* Dans AppComponent

# Architecture

Qui peut importer quoi :

![Qui peut importer quoi](/img/angular/qui-peut-importer-quoi.png)

Comment se chargent les modules :

![Comment se chargenet les modules](/img/angular/comment-se-chargent-les-modules.png)