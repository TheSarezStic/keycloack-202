# 📘 Módulo 8 – HA & Producción

## 01 – Migración a PostgreSQL (Base de datos real)

---

# 🎯 Objetivo

Entender por qué en producción **NO** debes usar la base de datos embebida y cómo migrar
**Keycloak**
a **PostgreSQL**.

Aquí empieza la arquitectura HA seria.

---

# 🧠 1️⃣ El problema del modo dev

En desarrollo usamos:

```bash
start-dev
```

Esto usa:

* H2 embebida
* No persistente
* No clusterizable
* No apta para producción

Si reinicias → puedes perder datos.

No sirve para HA.

---

# 🏗 2️⃣ Arquitectura correcta

![Image](https://www.keycloak.org/keycloak-benchmark/kubernetes-guide/latest/_images/storage/minikube-runtime-view-postgres.dio.svg)

![Image](https://students.cs.ucl.ac.uk/2021/group13/static/image/system_architecture.png)

![Image](https://i.sstatic.net/ZgD47.png)

![Image](https://docs.oracle.com/cd/E19354-01/817-5706/Images/overview2.gif)

Modelo:

```text
Keycloak
   ↓
PostgreSQL (externo)
```

Separación obligatoria.

---

# 🔐 3️⃣ Por qué PostgreSQL

Keycloak soporta varias BBDD, pero PostgreSQL es:

✔ Oficialmente recomendada
✔ Muy estable
✔ Buen rendimiento
✔ Soporta clustering
✔ Ideal en cloud (RDS, Cloud SQL)

---

# ⚙️ 4️⃣ Configuración mínima en Docker Compose

Ejemplo simple:

```yaml
version: "3.8"

services:

  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: password
    volumes:
      - pgdata:/var/lib/postgresql/data

  keycloak:
    image: quay.io/keycloak/keycloak:24.0
    command: start
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KC_DB_URL_DATABASE: keycloak
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: password
      KC_PROXY: edge
      KC_HOSTNAME: auth.local
    ports:
      - "8080:8080"
    depends_on:
      - postgres

volumes:
  pgdata:
```

Esto ya es modo producción básico.

---

# 🔎 5️⃣ Qué ocurre en el primer arranque

Keycloak:

* Crea esquema
* Crea tablas
* Aplica migraciones internas
* Inicializa realm master

Es automático.

---

# 🔄 6️⃣ Migrar desde H2 a PostgreSQL

Pasos:

1️⃣ Exportar realm desde entorno dev:

```bash
bin/kc.sh export --file realm-export.json
```

2️⃣ Arrancar Keycloak con PostgreSQL

3️⃣ Importar:

```bash
bin/kc.sh import --file realm-export.json
```

Nunca copiar directamente la base H2.

---

# 🔐 7️⃣ Seguridad en la base de datos

En producción:

✔ Base en red privada
✔ Usuario con permisos mínimos
✔ Password fuerte
✔ Conexión cifrada (SSL)
✔ Backups automáticos

Nunca:

❌ Exponer puerto 5432 públicamente

---

# 🧠 8️⃣ Parámetros importantes

Opciones útiles:

```bash
KC_DB_POOL_INITIAL_SIZE
KC_DB_POOL_MAX_SIZE
KC_DB_POOL_MIN_SIZE
```

Para ajustar conexiones en entornos con carga.

---

# 🚨 9️⃣ Errores comunes

❌ Seguir usando start-dev en producción
❌ No persistir volumen de Postgres
❌ No hacer backup
❌ No probar restauración
❌ Mezclar base entre entornos

---

# 🧩 10️⃣ Modelo mental correcto

```text
Keycloak = Stateless (casi)
Base de datos = Fuente de verdad
```

Para escalar horizontalmente:

→ Base compartida obligatoria.

---

# 🎯 Qué debes retener

* H2 solo para laboratorio
* PostgreSQL obligatorio en producción
* Separación Keycloak / DB
* Backup imprescindible
* Base privada siempre