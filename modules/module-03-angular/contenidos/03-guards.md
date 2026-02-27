# 📘 Módulo 3 – Angular

## 03 – Guards

---

# 🎯 Objetivo

Proteger rutas en Angular usando:

* Tokens emitidos por **Keycloak**
* Autenticación OIDC
* Validación de roles

La autenticación ya funciona.
Ahora toca **autorizar navegación**.

---

# 🧠 1️⃣ ¿Qué es un Guard?

En Angular, un Guard es una clase que decide:

> ¿Puede el usuario acceder a esta ruta?

Se ejecuta antes de cargar el componente.

---

# 🏗 2️⃣ Escenario típico

Rutas:

* `/dashboard` → cualquier usuario autenticado
* `/admin` → solo rol admin
* `/facturas` → solo rol invoice.read

---

# 🔐 3️⃣ Guard básico (solo autenticación)

Crear `auth.guard.ts`:

```ts
import { Injectable } from '@angular/core';
import { CanActivate } from '@angular/router';
import keycloak from './keycloak';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  canActivate(): boolean {
    return keycloak.authenticated ?? false;
  }
}
```

---

# 🛣 4️⃣ Aplicarlo en routing

```ts
const routes = [
  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [AuthGuard]
  }
];
```

Ahora solo usuarios autenticados pueden entrar.

---

# 🧩 5️⃣ Guard con validación de roles

Aquí ya miramos el token.

```ts
canActivate(): boolean {

  const roles = keycloak.tokenParsed?.realm_access?.roles || [];

  return roles.includes('admin');
}
```

---

# 🔎 6️⃣ Validar Client Roles (más correcto)

Normalmente las APIs usan client roles.

```ts
canActivate(): boolean {

  const resourceAccess = keycloak.tokenParsed?.resource_access;
  const clientRoles = resourceAccess?.['angular-app']?.roles || [];

  return clientRoles.includes('invoice.read');
}
```

---

# 🧠 7️⃣ Modelo mental importante

```text id="p8v1zw"
Frontend protege UX
Backend protege seguridad real
```

El Guard:

* Mejora experiencia
* Evita mostrar pantallas indebidas

Pero la seguridad REAL siempre está en la API.

---

# 🚨 8️⃣ Error típico

Pensar que:

> “Si lo protejo con Guard ya es seguro”

Incorrecto.

Cualquiera puede:

* Manipular frontend
* Forzar llamadas API

La API debe validar el token SIEMPRE.

---

# 🔥 9️⃣ Guard avanzado con redirección

```ts
canActivate(): boolean {

  if (!keycloak.authenticated) {
    keycloak.login();
    return false;
  }

  return true;
}
```

Redirige automáticamente a login si no autenticado.

---

# 🧠 10️⃣ Buen patrón

Separar:

* AuthGuard → valida autenticación
* RoleGuard → valida roles

Permite reutilización limpia.

---

# 🎯 Qué debes retener

* Guard protege rutas Angular
* Puede validar roles del token
* No sustituye seguridad backend
* Debe usar client roles preferentemente
* Es capa UX, no capa seguridad real