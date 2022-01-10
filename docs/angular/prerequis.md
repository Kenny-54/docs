# Prérequis 

Installation d’avant projet Angular 

# Visual Studio Code 

## IDE 

<https://code.visualstudio.com/>


## Extensions

* Angular Language Service (par Angular) 
* Angular Schematics (par Cyrille Tuzi) 
* ESLint (par Dirk Baeumer)
* EditorConfig for VS Code (par EditorConfig)
* French Language Pack for Visual Studio Code (par Microsoft)

  \

# Git

[Git](http://git-scm.com/) 


Vérifier dans un terminal avec : 


```bash
git --version 
```


Configurer votre identité : 


```bash
git config --global user.name "Xxx Xxx"
git config --global user.email xxx@example.com
```


# Node 

[Node.js](https://nodejs.org/en/) 


Vérifier dans un terminal avec :


```bash
node -v 
```


# NPM

```bash
npm install npm@6 --global
```


# Angular CLI 

```bash
npm install @angular/cli -g
```


Vérifier dans un terminal avec : 


```bash
ng version
```


# Proxy 

Si votre poste passe par un proxy, il faut le configurer partout, notamment : 


```bash
npm config set proxy http://proxy.company.com:8080
npm config set https-proxy http://proxy.company.com:8080
```


> Option http.proxy dans les Préférences de Visual Studio Code.


\
