# 📘 Módulo 8 – HA & Producción

## 02 – Configuración completa de producción

---

# 🎯 Objetivo

Configurar correctamente **Keycloak** para entorno productivo:

* Modo `start` (no dev)
* Hostname fijo
* Proxy correcto
* Base PostgreSQL
* Cache distribuida preparada
* Health endpoints activos

Aquí ya hablamos como si fuera a Internet.

---

# 🧠 1️⃣ Regla de oro

```text
Nunca usar start-dev en producción
```

Modo correcto:

```bash
bin/kc.sh start
```

---

# 🏗 2️⃣ Arquitectura final objetivo

![Image](https://www.keycloak.org/keycloak-benchmark/kubernetes-guide/latest/_images/minikube-runtime-view.dio.svg)

![Image](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fdzmpjme0hkb14spdi2l8.png)

![Image](https://developers.redhat.com/sites/default/files/fkeycloak_1.png)

![Image](https://blog.ippon.tech/hubfs/Imported_Blog_Media/feedback-keycloak-high-availability-in-cloud-environment-aws-part-1-img2-2.png)

Modelo:

```text
Internet
   ↓
Reverse Proxy / Load Balancer
   ↓
Keycloak (stateless)
   ↓
PostgreSQL
```

---

# 🔐 3️⃣ Variables críticas de producción

Variables mínimas obligatorias:

```bash
KC_DB=postgres
KC_DB_URL_HOST=postgres
KC_DB_URL_DATABASE=keycloak
KC_DB_USERNAME=keycloak
KC_DB_PASSWORD=******
```

---

## 🌐 Hostname fijo

```bash
KC_HOSTNAME=auth.midominio.com
KC_HOSTNAME_STRICT=true
```

Esto evita:

* Redirects maliciosos
* Open redirect attacks
* Host header injection

---

## 🔁 Proxy

Si hay reverse proxy:

```bash
KC_PROXY=edge
```

Indica que confíe en `X-Forwarded-*`.

---

# 🔒 4️⃣ HTTPS obligatorio

En producción:

```bash
KC_HTTP_ENABLED=false
```

Si TLS termina en proxy:

* No exponer HTTP públicamente
* Red privada interna solamente

---

# ⚙️ 5️⃣ Configuración de cache

Keycloak usa Infinispan para cache interna.

En modo simple:

```bash
KC_CACHE=ispn
```

En cluster (más adelante):

→ Cache distribuida.

Por ahora:

Un nodo = cache local.

---

# 🔄 6️⃣ Health checks

Activar endpoints:

```bash
KC_HEALTH_ENABLED=true
KC_METRICS_ENABLED=true
```

Endpoints disponibles:

```text
/health
/health/live
/health/ready
/metrics
```

Crítico para:

* Kubernetes
* Load Balancers
* Monitorización

---

# 🔥 7️⃣ Ejemplo docker-compose producción básica

```yaml
services:
  keycloak:
    image: quay.io/keycloak/keycloak:24.0
    command: start
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KC_DB_URL_DATABASE: keycloak
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: password
      KC_HOSTNAME: auth.local
      KC_HOSTNAME_STRICT: "true"
      KC_PROXY: edge
      KC_HEALTH_ENABLED: "true"
      KC_METRICS_ENABLED: "true"
    depends_on:
      - postgres
```

Ya es entorno productivo mínimo.

---

# 🚨 8️⃣ Errores graves frecuentes

❌ No fijar hostname
❌ No activar health endpoints
❌ No usar start
❌ No configurar proxy
❌ Dejar HTTP abierto

---

# 🧠 9️⃣ Checklist producción inicial

```text
[ ] Base PostgreSQL externa
[ ] Hostname fijo configurado
[ ] Proxy configurado
[ ] HTTPS activo
[ ] Health endpoints activos
[ ] No start-dev
```

---

# 🧩 10️⃣ Modelo mental correcto

```text
Dev mode = laboratorio
Start mode = producción
```

Separación clara de entornos.

---

# 🎯 Qué debes retener

* start-dev nunca en prod
* Hostname fijo obligatorio
* Proxy configurado correctamente
* Health endpoints activados
* Base externa obligatoria