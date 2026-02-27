# 📘 Módulo 3 – Angular

## 05 – Refresh Automático (Implementación Profesional)

---

# 🎯 Objetivo

Implementar refresh automático de tokens en Angular usando:

* `keycloak-js`
* Buen patrón de arquitectura
* Sin memory leaks
* Sin intervalos sucios
* Con manejo correcto de errores

Con tokens emitidos por
**Keycloak**

---

# 🧠 1️⃣ Problema del enfoque básico

Muchos hacen esto:

```ts
setInterval(() => {
  keycloak.updateToken(30);
}, 10000);
```

Problemas:

* Interval permanente
* No se cancela
* No maneja errores
* Puede provocar múltiples refresh simultáneos

Esto es amateur.

---

# 🏗 2️⃣ Patrón profesional: Refresh bajo demanda

En vez de refrescar cada X segundos:

👉 Refresca antes de cada llamada HTTP.

---

# 🔐 3️⃣ Crear un HTTP Interceptor

`auth.interceptor.ts`

```ts id="x3k9dl"
import { Injectable } from '@angular/core';
import { HttpEvent, HttpHandler, HttpInterceptor, HttpRequest } from '@angular/common/http';
import { from, Observable } from 'rxjs';
import { switchMap } from 'rxjs/operators';
import keycloak from './keycloak';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {

    return from(keycloak.updateToken(30)).pipe(
      switchMap(() => {

        const token = keycloak.token;

        const cloned = req.clone({
          setHeaders: {
            Authorization: `Bearer ${token}`
          }
        });

        return next.handle(cloned);
      })
    );
  }
}
```

---

# 🧠 4️⃣ Qué hace esto

Antes de cada request:

1. Comprueba si el token expira en <30s
2. Si es así → lo refresca
3. Añade Authorization header
4. Envía request

Sin intervalos.
Sin polling.
Solo cuando hace falta.

---

# 🛠 5️⃣ Registrar el interceptor

En `app.module.ts`:

```ts id="q8z4ne"
providers: [
  {
    provide: HTTP_INTERCEPTORS,
    useClass: AuthInterceptor,
    multi: true
  }
]
```

---

# 🔥 6️⃣ Manejo de error elegante

Si refresh falla:

```ts id="t7n3vq"
return from(keycloak.updateToken(30)).pipe(
  switchMap(() => next.handle(cloned)),
  catchError(() => {
    keycloak.login();
    return EMPTY;
  })
);
```

Así:

* Si refresh token expiró
* Se redirige automáticamente a login

---

# 🧠 7️⃣ Modelo mental correcto

```text
No refresques por tiempo.
Refresca por necesidad.
```

---

# ⚠️ 8️⃣ Edge cases reales

## 🔹 Usuario inactivo 2h

Refresh token expira → login automático

## 🔹 Múltiples pestañas

Cada pestaña tiene su memoria.
Se recomienda usar:

* BroadcastChannel API
* O iframe silent check-sso

---

# 🔐 9️⃣ Seguridad importante

Nunca:

* Guardes refresh token en localStorage
* Guardes token en sessionStorage
* Expongas token en logs

keycloak-js lo mantiene en memoria.

---

# 🧩 10️⃣ Flujo final profesional

```text
Angular
   ↓
HTTP Interceptor
   ↓
updateToken()
   ↓
Añade Bearer
   ↓
API valida JWT
```

---

# 🎯 Qué debes retener

* No usar setInterval simple
* Usar HTTP interceptor
* Refrescar antes de request
* Manejar errores de refresh
* Mantener tokens en memoria
