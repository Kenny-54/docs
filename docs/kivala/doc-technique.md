# Documentation technique

# Fonctionnement de l’API

## Connexion de l’utilisateur


1. Connexion de l’utilisateur : nom d’utilisateur + mot de passe
2. Appel de la méthode POST /auth/login de l’API avec le payload suivant : 

   
   1. nom d’utilisateur (saisi par l’utilisateur)
   2. mot de passe (saisi par l’utilisateur)
   3. nom de l’application (paramètrage du frontend - fichier environment.ts)
3. Réponse de l’API

| Cas | Données retournées | Code HTTP |
|----|----|----|
| Connexion réussie | Infos utilisateurs + tokens \n (json : ApiData\<LoginResponse\>) | 200 |
| Identifiants incorrects | Message d’erreur | 401 |
| Payload vide ou incomplet | Message d’erreur | 400 |
| Utilisateur non trouvé dans l’AD | Message d’erreur | 403 |
| Application non trouvée (ou paramètres  \n la concernant manquants dans la  \n configuration de l’API) | Message d’erreur | 401 |

   \
4. La librairie Kivala traite le retour de l’API de la façon suivante :
   * Dans le cas d’une erreur 40x, le message d’erreur retourné est affiché à l’utilisateur
   * Dans le cas où la connexion est réussie
     * les données retournées sont stockées dans localstorage/sessionstorage du navigateur et l’utilisateur est redirigé sur la page racine de l’application (“/”)
     * la librairie enregistre en BDD : l’identifiant de l’utilisateur, un id de session généré, le refresh token et sa date d’expiration et le nom de l’application
5. Par la suite, l’access token sera transmis en entête de toutes les requêtes d’API (entête “Authorization”, valeur “Bearer \<acces token\>”

   \

## Rafraîchissement des tokens


1. Appel de la méthode POST /token/refresh de l’API avec dans le body le refresh token
2. Réponse de l’API

| Cas | Données retournées | Code HTTP |
|----|----|----|
| Réussite | Les tokens (json : ApiData\<TokenApiModel\> | 200 |
| Acces token manquant \n (entête “Authorization”) | - | 400 |
| Données suivantes manquantes : refresh token (payload), nom de l’appli / id de sessions (dans l’access token) | - | 400 |
| Application ou clé de signature JWT associé non trouvé | - | 400 |
| Access token non valide | Message d’erreur | 400 |
| Utilisateur non retrouvé depuis l’access token | Message d’erreur | 400 |
| Utilisateur non trouvé en BDD ou refresh token différent de celui attendu | Message d’erreur | 401 |
| Refresh token expiré | Message d’erreur | 401 |
3. La librairie Kivala traite le retour de l’API de la façon suivante :
   * Dans le cas d’une erreur 40x, le message d’erreur retourné est affiché à l’utilisateur
   * Dans le cas où la connexion est réussie
     * les nouveaux tokens retournées sont stockées dans localstorage/sessionstorage du navigateur à la place des anciens
     * la librairie met à jour en BDD le refresh token

   \


## Déconnexion de l’utilisateur


1. Appel de la méthode GET /auth/logout de l’API
2. Réponse de l’API : 

| Cas | Données retournées | Code HTTP |
|----|----|----|
| Réussite | - | 200 |
| Acces token manquant \n (entête “Authorization”) | - | 400 |
| Données suivantes manquantes : nom de l’appli / id de sessions (dans l’access token) | - | 400 |
| Application ou clé de signature JWT associé non trouvé | - | 400 |
| Access token non valide | - | 400 |
| Utilisateur non retrouvé depuis l’access token | - | 400 |
| Session non trouvé en BDD  | - | 401 |
3. Dans tous les cas (réussite ou erreur), la librairie vide le localstorage/sessionstorage du navigateur et l’utilisateur est redirigé à la racine de l’application (qui doit aboutir à l’écran de connexion)


# Fonctionnement et fonctionnalités de la librairie Angular Kivala

## Fonctionnalités générales

La librairie gère différents aspect du mécanisme d’authentification et d’autorisation : 

* gestion des tokens (acces token au format JWT et refresh token)
* redirection vers la page de connexion ou la racine de l’application selon les cas
* protection des routes Angular
* interception des requêtes HTTP pour y ajouter une entête d’authentification (pour les appels aux API)

## Fonctionnement de la librairie

### auth.guard.ts

Protège les routes (routing Angular) en se basant sur la présence de l’access token

Si ce dernier est expiré, il est renouvelé


:::info
Ce processus n’intervient que lorsque la route change, donc pas lors d’une requête Ajax (appel d’API), mais ce cas est traité via l’interceptor (voir ci-dessous)

:::

### auth.interceptor.ts

Intercepte chaque requête HTTP (appels d’API) afin d’y ajouter une entête “Authorization” contenant l’access token précédé par “Bearer “.


Lorsqu’une requête HTTP aboutit à une erreur 401, c’est que l’access token a expiré. Ce dernier est donc renouvelé et une fois les nouveaux tokens reçus, la requête HTTP initiale est executée une nouvelle fois afin que tout soit transparent pour l’utilisateur.

C’est la fonction gérant l’erreur 401 qui se charge d’exécuter une nouvelle fois la requête initiale une fois qu’elle a reçue les nouveaux tokens.


Si pendant le renouvellement des tokens d’autres requêtes HTTP sont exécutées, cela ne déclenche pas d’appels supplémentaires à l’API Kivala pour rafraîchir les tokens. 

Ces requêtes HTTP sont également exécutées après la récupération des nouveau tokens, via un `BehaviorSubject`. 

Ce dernier est affecté de la valeur`null` au moment où le processus de renouvellement des tokens est lancés. Les requêtes seront ainsi exécutées de nouveau une fois que ce`BehaviorSubject`aura été affecté d’une valeur autre que `null` (ce qui est fait quand les nouveaux tokens sont reçus).

### auth.service.ts

Expose la méthode permettant de mettre à jour les tokens via l’API Kivala

### login.service.ts

Expose les méthodes de connexion et déconnexion de l’utilisateur : 

* login() : méthode à appeler lors de la soumission du formulaire de connexion
* logout() : méthode à appeler pour déconnecter l’utilisateur

token.service.ts

Expose les méthodes gérant les données de l’utilisateur et les tokens - transmis par l’API Kivala quand la connexion est réussie - dans le localstorage du navigateur



:::info
La méthode getUser() retourne un JSON contenant les données suivantes concernant l’utilisateur : 

* identifiant
* libellé
* email
* liste des rôles

:::


Informations de l’utilisateur


