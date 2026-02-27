# 📘 Módulo 1 – Fundamentos

## 06 – Instalación en modo desarrollo

---

# 🎯 Objetivo

Levantar **Keycloak** en modo desarrollo para:

* Entrar en la consola admin
* Crear un realm
* Probar login
* Entender endpoints OIDC

Sin PostgreSQL externa.
Sin clustering.
Solo laboratorio.

---

# 🧠 1️⃣ Qué es el modo `start-dev`

Cuando ejecutas:

```bash
start-dev
```

Keycloak:

* Usa base H2 embebida
* Activa configuración flexible
* Desactiva optimizaciones de producción
* Permite admin bootstrap simple

⚠️ Todo se pierde al reiniciar.

---

# 🐳 2️⃣ Opción recomendada: Docker

Crear `docker-compose.yml`

```yaml
version: "3.8"

services:
  keycloak:
    image: quay.io/keycloak/keycloak:24.0
    container_name: keycloak-dev
    command: start-dev
    environment:
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8080:8080"
```

---

# ▶️ 3️⃣ Levantar el entorno

```bash
docker compose up -d
```

Ver logs:

```bash
docker logs -f keycloak-dev
```

Esperar mensaje:

```
Keycloak started
```

---

# 🌐 4️⃣ Acceso a consola

Abrir navegador:

```
http://localhost:8080
```

Entrar en:

```
Administration Console
```

Usuario:

```
admin
```

Password:

```
admin
```

---

# 🔎 5️⃣ Endpoints importantes

Una vez levantado:

### Realm master

```text
http://localhost:8080/realms/master
```

### OpenID Configuration

```text
http://localhost:8080/realms/master/.well-known/openid-configuration
```

Aquí ves:

* Authorization endpoint
* Token endpoint
* JWKS endpoint
* Userinfo endpoint

---

# 🔐 6️⃣ Estructura interna en dev

```text
Browser → Keycloak container → H2 (in-memory)
```

No hay base externa.
No hay clustering.
No hay proxy.

---

# 🧪 7️⃣ Validación rápida

Probar health:

```
http://localhost:8080/health
```

Probar realm:

```
http://localhost:8080/realms/master
```

---

# ⚠️ 8️⃣ Limitaciones del modo dev

* ❌ No usar en producción
* ❌ No escalable
* ❌ Sin clustering real
* ❌ Sin optimización
* ❌ Persistencia frágil

---

# 🧠 9️⃣ Qué debes entender

* Keycloak puede arrancar sin PostgreSQL en dev
* En producción siempre necesita DB externa
* start-dev simplifica arranque
* Docker facilita laboratorio reproducible

---

# 🚀 Resultado esperado

Al terminar este módulo debes poder:

* Levantar Keycloak
* Entrar en consola
* Identificar realm master
* Localizar endpoints OIDC

