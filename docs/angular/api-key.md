# API key (en cours)

Démarche à suivre pour utiliser une API key dans un projet Angular


Avant de passer à la suite, consulter et suivre la doc. [suivante](fichier-secrets-dans-appli-angular)


Dans src/app/core-services, créer un dossier “auth” et y ajouter les fichiers suivants


auth.interceptor.ts

```javascript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpHandler, HttpRequest, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { secrets } from '@core/secrets';


/* `{ providedIn: 'root' }` has been deleted as an interceptor is manually provided in `AppModule` */
// eslint-disable-next-line @angular-eslint/use-injectable-provided-in
@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  constructor() {}

  intercept(request: HttpRequest<unknown>, next: HttpHandler): Observable<HttpEvent<unknown>> {

    const authReq = request.clone({
      setHeaders: { 'X-Api-Key': `${secrets.apiKey}` },
    });

    return next.handle(authReq).pipe();

  }

}
```


index.ts

```javascript
export { AuthInterceptor } from './auth.interceptor';
```


Dans le fichier app.module.ts, ajouter un provider 

```javascript
import { AuthInterceptor } from '@core/auth';
[...]
providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true },
  ],
```


