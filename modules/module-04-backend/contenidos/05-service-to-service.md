# 📘 Módulo 4 – Backend

## 05 – Service-to-Service (Arquitectura completa)

---

# 🎯 Objetivo

Entender cómo diseñar comunicación segura entre microservicios usando:

* JWT firmados
* Client Credentials
* Validación distribuida
* Buen diseño de permisos

Todo apoyado en
**Keycloak**

---

# 🧠 1️⃣ Problema real

En una arquitectura moderna tienes:

* API Gateway
* user-service
* billing-service
* notification-service
* report-service

Todos necesitan hablar entre sí.

No puedes usar:

* API Keys hardcodeadas
* Shared secrets globales
* Tokens sin expiración

Necesitas delegación segura.

---

# 🏗 2️⃣ Arquitectura profesional

![Image](https://miro.medium.com/1%2AeaEjaer6x8QeWBmmRFqA_A.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AQbC9PZl47k2UVDLOLMyo0Q.png)

![Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1704091263164/444676ec-3569-4758-b01d-9bce898acbb3.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/0%2AsDOvdDXQqEW5_-Dd.png)

Flujo típico:

```text id="arch1"
Service A
   ↓ (client credentials)
Keycloak
   ↓ (access token)
Service B
   ↓
JWT validation
```

Stateless.
Sin sesiones.
Sin cookies.

---

# 🔐 3️⃣ Principio clave: Un cliente por servicio

Mala práctica:

```text id="bad1"
microservices-client (usado por todos)
```

Buena práctica:

```text id="good1"
billing-service
user-service
notification-service
report-service
```

Cada uno:

* Tiene su client_id
* Tiene su client_secret
* Tiene roles mínimos necesarios

---

# 🧩 4️⃣ Diseño de roles correcto

En `report-service`:

Client roles:

* report.read
* report.generate

En `billing-service`:

* invoice.read
* invoice.write

No mezclar permisos entre servicios.

---

# 🔎 5️⃣ Validación estricta en backend

Además de firma, validar:

```js id="audcheck"
audience: "report-service"
```

Así solo tokens emitidos para ese servicio son válidos.

---

# 🧠 6️⃣ Principio de mínimo privilegio

Ejemplo:

billing-service solo necesita:

```text id="least1"
invoice.read
```

No darle:

```text id="least2"
admin
```

Ni roles globales innecesarios.

---

# 🔥 7️⃣ Patrón con API Gateway

Escenario:

```text id="gateway1"
Client → API Gateway → user-service
```

Opciones:

1️⃣ Gateway valida token y reenvía
2️⃣ Cada microservicio valida JWT
3️⃣ Ambos (defensa en profundidad)

Recomendado en entornos críticos:
Validación en cada servicio.

---

# 🚨 8️⃣ Errores comunes en microservicios

❌ Compartir mismo client_secret
❌ No validar audience
❌ No validar issuer
❌ Usar tokens demasiado largos
❌ No rotar secretos

---

# 🔄 9️⃣ Rotación de claves

Keycloak puede:

* Rotar claves RSA
* Cambiar `kid`
* Publicar nuevas claves en JWKS

Si usas `jwks-rsa`, tu backend soporta rotación automática.

---

# 🧠 10️⃣ Flujo completo empresarial

```text id="enterprise1"
User login (OIDC)
   ↓
Frontend recibe token
   ↓
API Gateway
   ↓
Microservicios
   ↓
Service-to-service tokens
   ↓
Validación distribuida
```

Todo con JWT firmados.

---

# 🎯 Qué debes retener

* Un cliente por microservicio
* Client Credentials para máquina
* Validar issuer + audience + firma
* Principio de mínimo privilegio
* Arquitectura stateless escalable

