# 📘 Módulo 8 – HA & Producción

## 07 – API Gateway (Conceptual) y Patrones de Integración

---

# 🎯 Objetivo

Entender cómo integrar
**Keycloak**
dentro de una arquitectura moderna usando:

* API Gateway
* Microservicios
* Validación centralizada de JWT
* Zero Trust

Aquí ya hablamos de arquitectura empresarial real.

---

# 🧠 1️⃣ El problema en microservicios

Sin patrón claro:

```text id="2b8r3v"
Frontend → Servicio A
Frontend → Servicio B
Frontend → Servicio C
```

Cada servicio valida JWT por su cuenta.

Puede funcionar, pero:

* Repetición de lógica
* Riesgo de inconsistencia
* Mala observabilidad central

---

# 🏗 2️⃣ Arquitectura con API Gateway

![Image](https://aws-samples.github.io/keycloak-on-aws/en/images/implementation-guide/tutorial/api-gateway/01-en-architecture-diagram.svg)

![Image](https://dz2cdn1.dzone.com/storage/temp/16172727-figure-1-1.png)

![Image](https://fusionauth.io/img/articles/zero-trust-diagram.png)

![Image](https://learn.microsoft.com/en-us/security/zero-trust/media/diagram-conditional-access-policies.png)

Modelo:

```text id="r5y0cd"
Cliente
   ↓
API Gateway
   ↓
Microservicios
```

El Gateway:

✔ Valida JWT
✔ Aplica rate limiting
✔ Enruta tráfico
✔ Centraliza seguridad

---

# 🔐 3️⃣ Flujo típico con Gateway

```text id="0n1h2u"
Usuario login
   ↓
Keycloak emite JWT
   ↓
Cliente llama API Gateway
   ↓
Gateway valida firma JWT (JWKS)
   ↓
Reenvía request a microservicio
```

El backend puede:

* Volver a validar
* Confiar en gateway (según diseño)

---

# 🔎 4️⃣ Patrones de validación

## Opción A – Validación en cada servicio

✔ Más seguro
✔ Verdadero Zero Trust
✔ Cada servicio valida JWT con JWKS

Recomendado en sistemas críticos.

---

## Opción B – Validación solo en Gateway

✔ Más simple
✔ Menos carga en servicios
⚠ Menos estricto

---

# 🧩 5️⃣ Roles y scopes en arquitectura Gateway

El JWT puede contener:

```json id="1z7jmn"
{
  "realm_access": {
    "roles": ["admin"]
  },
  "resource_access": {
    "api-service": {
      "roles": ["read"]
    }
  }
}
```

El Gateway puede:

* Bloquear rutas por rol
* Aplicar políticas
* Traducir scopes

---

# 🔥 6️⃣ Gateway + Keycloak en producción

Componentes típicos:

* NGINX
* Kong
* Traefik
* AWS API Gateway
* Istio

Todos pueden validar JWT emitidos por Keycloak.

---

# 🧠 7️⃣ Zero Trust real

Modelo moderno:

```text id="mental1"
Ningún servicio confía en la red.
Solo confía en identidad verificada.
```

Cada request:

* Debe llevar JWT válido
* Debe validarse firma
* Debe validarse issuer
* Debe validarse audience

---

# 🔒 8️⃣ Audience y separación de APIs

Muy importante:

Configurar correctamente:

```text id="r5u2k8"
aud claim
```

Para evitar:

* Token de API A usado contra API B
* Privilege escalation

---

# 🚨 9️⃣ Error común grave

❌ No validar audience
❌ No validar issuer
❌ Usar el mismo client para todo
❌ Usar redirect URIs abiertas
❌ No separar entornos

---

# 🏢 10️⃣ Arquitectura enterprise madura

Modelo completo:

```text id="archfinal"
Usuario
   ↓
Keycloak (IdP)
   ↓
API Gateway
   ↓
Microservicios
   ↓
Base de datos
```

Con:

✔ HA en Keycloak
✔ HA en DB
✔ Health checks
✔ Monitorización
✔ Backup

---

# 🧠 11️⃣ Modelo mental final del curso

```text id="mental2"
Identidad centralizada
+
Tokens firmados
+
Validación consistente
+
Infra HA
=
Arquitectura segura
```

---

# 🎯 Qué debes retener

* API Gateway simplifica seguridad
* JWT debe validarse siempre
* Audience e issuer son críticos
* Zero Trust es el modelo moderno
* Keycloak es el núcleo de identidad