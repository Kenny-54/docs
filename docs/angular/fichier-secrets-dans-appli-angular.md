# Fichier secrets dans une appli Angular

Afin de stocker des données sensibles nécessaires au fonctionnement de l’application et ne devant pas être enregistrées dans GIT, voici la démarche à suivre.

# Création des fichiers

Dans le répertoire src/environments, créer les 2 fichiers suivants :

* secrets.ts
* secrets.prod.ts

Exemple du contenu d’un de ses fichiers, pour y stocker une clé d’API dans ce cas

# Configuration dans le projet Angular

Pour utiliser le contenu de ses fichiers, ajouter le code suivant dans la section “projects/<nom_du_projet>/architect/build/configurations/production/fileReplacements“ du fichier angular.json.

Cela permet d'avoir un fichier pour l'environnement de DEV et un autre pour celui de PROD.

```json
{
    [...]
    "projects": {
        "<nom_du_projet>": {
            [...]
            "architect": {
                "build": {
                    [...]
                    "configurations": {
                        "production": {
                            [...]
                            "fileReplacements": [{
                                [...]
                                {
                                    "replace": "src/environments/secrets.ts",
                                    "with": "src/environments/secrets.prod.ts"
                                }
                            ],
                            [...]
                        },
                        [...]
                    },
                    [...]
                },
                [...]
            }
        }
    },
    [...]
}
```


Et ajouter une entrée dans la section “paths” du fichier tsconfig.json

```json
/* To learn more about this file see: https://angular.io/config/tsconfig. */
{
  [...]
  "compilerOptions": {
    [...]
    "paths": {
     [...]
      "@core/secrets": [
        "src/environments/secrets"
      ],
      [...]
    }
  },
  [...]
}
```

# Contenu des fichiers

```typescript
export const secrets = {
    cle1: 'valeur1',
    cle2: 'valeur2',
};
```

# Utilisation

Exemple

```javascript
import { secrets } from '@core/secrets';
[...]
const apiKey = `${secrets.apiKey}`;
```

# Configuration GIT

Afin que les fichiers “secrets” ne soit pas stockés sur GIT, ajouter le code suivant dans le fichier “.gitignore” du projet

```
# Fichiers contenant des données sensibles de l'application
src/environments/secrets*.ts
```


