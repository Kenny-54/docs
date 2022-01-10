# TypeScript 

# ES

## Récapitulatif 

Variable : 


```typescript
let / const 
```


Template strings : 


```typescript
`${userFirstName} ${userLastName}
sur plusieurs lignes`
```


Itération : 


```typescript
for (const value of list) {}
```


Arrow functions : 


```typescript
list.forEach((value) => value * 2);
```


Nouvelles collections : 


```typescript
Map 
```


Optional chaining :


```typescript
user?.identity?.name
```


Nullish coalescing : 


```typescript
data ?? `default`
```


# TypeScript 

## Pourquoi TypeScript ? 

Application = JavaScript qui gère : 

* La navigation (routing)
* La génération des pages (templating)

**A la moindre erreur => application H.S.**


Exemples de problèmes types : 


```typescript
1 == '1' // true
1 === '1' // false

'Il y a ' + 5 + 3 + ' commentaires'; // 53

if (`Henri Bergson`.indexOf(`H`)) {} // false

{} != {}
[] != []
2 != true
```


## Typage 

* **Typage dynamique** = possibilités de changer de type en cours de route => **plus facile** mais peu fiable et peu performant => JavaScript, PHP
* **Typage statique** = impossible de changer de type en cours de route => performant et **fiable** => **TypeScript**, Java, C#…


## Typage explicite 

```typescript
const userMan: boolean = true;
const userAge: number = 81;
const userName: string = `Henri Bergson`;
const userBooks: string[] = [`Livre 1`, `Livre 2`];
```


## Typage explicite d’une fonction 

```typescript
function test(
    required: string,
    optional: string = `default value`
): boolean {

return true;

}
```


## Transpilation 

* Transpilation en continue (watch mode) 
* Debug grâce au sourcemaps 


## TypeScript : récapitulatif 

Pas de frein à utiliser TypeScript car : 

* Simplement du **JavaScript** **amélioré**, réversible à tout moment 
* **Transparent** lors du développement 


Avantages : 

* **Typage** **statique** = gestion des erreurs 
* **Typing** : complétion pour n’importe quelle librairie 
* Fonctionnalités supplémentaires, notamment en POO (interfaces,…) 
* Performances 


# TypeScript strict

## Typage implicite 

Typage explicite systématique **non nécessaire** 

* TypeScript se débrouille dans \~80% des cas 
* Il faut seulement l’aider quand il ne peut pas deviner (any)


## Configuration

* Typage strict (tsconfig.json)


```typescript
"strict": true
```


* Automatique à partir d’Angular 12


## Typage strict


2 principaux changements : 

* **noImplicitAny** : typage obligatoire si (et seulement si) non inférable
* **strictNullChecks** : null non autorisé par défaut 


## Unions 

```typescript
let message: string | null = null;
```


## Librairies tierces 

Librairies TypeScript : vérifier le support du monde strict 

* Strict
* ou noImplicitAny + strictNullChecks 

Librairies JavaScript : vérifier l’existence de @types 


# TypeScript (vraiment) strict 

## Any is evil 

```typescript
function test(message: any): void {
    // Will compile but may fail at runtime, as message may not be a string
    message.substr(2);
}
```


## Lint Strict 

Mode strict pas suffisant [TypeScript strictly typed: strict mode is not enough](https://medium.com/@cyrilletuzi/typescript-strictly-typed-strict-mode-is-not-enough-40df698e2deb)

eslintrc.json : 


```typescript
"@typescript-eslint/no-explicit-any": true
```


## unknown 

```typescript
function test(message: unknown): void {
    // Will NOT compile
    message.substr(2);
    // Will compile and work at runtime
    if (typeof message === 'string') {
        message.substr(2);
    }
}
```


\
