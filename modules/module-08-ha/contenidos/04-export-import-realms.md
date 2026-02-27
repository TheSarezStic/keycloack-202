# 📘 Módulo 8 – HA & Producción

## 04 – Export / Import de Realms en Producción

---

# 🎯 Objetivo

Aprender a mover configuraciones entre entornos en
**Keycloak** sin:

* Corromper datos
* Romper cluster
* Duplicar usuarios
* Perder claves

Esto es clave para:

* Promover de dev → stage → prod
* Hacer backups lógicos
* Migrar entre clusters

---

# 🧠 1️⃣ Qué es realmente un Export

Un export NO es un dump de base de datos.

Es un:

```text
Export lógico de configuración
```

Incluye:

✔ Realms
✔ Clientes
✔ Roles
✔ Flows
✔ Grupos
✔ Policies

Opcionalmente:

✔ Usuarios

No incluye:

❌ Sesiones activas
❌ Cache
❌ Logs

---

# 🏗 2️⃣ Arquitectura de migración típica

![Image](https://developers.redhat.com/sites/default/files/blog/2019/11/keycloak3.png)

![Image](https://production-gitops.dev/guides/cp4i/mq/cloud-native/images/mq_deployment_environment.drawio.svg)

![Image](https://developers.redhat.com/sites/default/files/blog/2019/11/keycloak1.png)

![Image](https://miro.medium.com/1%2AVIJMYhB_QyE2Tr5waWHRBg.jpeg)

Modelo:

```text
DEV
  ↓ export
STAGE
  ↓ export validado
PROD
```

Nunca migrar base directamente entre entornos.

---

# 🔐 3️⃣ Export básico

Comando:

```bash
bin/kc.sh export \
  --dir /opt/keycloak/data/export \
  --realm mi-realm
```

Genera:

```text
mi-realm-realm.json
```

---

# 🔄 4️⃣ Export con usuarios

```bash
bin/kc.sh export \
  --dir /opt/keycloak/data/export \
  --realm mi-realm \
  --users realm_file
```

Opciones:

| Modo            | Qué exporta                    |
| --------------- | ------------------------------ |
| skip            | No usuarios                    |
| realm_file      | Todos en mismo JSON            |
| different_files | Usuarios en archivos separados |

---

# ⚠️ 5️⃣ Import en producción

Debe hacerse:

* Con el servidor parado
  o
* En nodo aislado

Comando:

```bash
bin/kc.sh import \
  --dir /opt/keycloak/data/export
```

Nunca importar mientras cluster está activo.

---

# 🚨 6️⃣ Error crítico común

❌ Importar en cluster activo
❌ Importar dos veces mismo realm
❌ Mezclar export de versión antigua con versión nueva

Siempre validar versión compatible.

---

# 🔒 7️⃣ Claves criptográficas (muy importante)

Cada realm tiene claves:

* Firma de tokens
* Rotación de keys

Si migras realm:

✔ Las claves viajan en export
✔ Los JWT seguirán siendo válidos

Si generas realm nuevo:

→ Claves cambian
→ Tokens antiguos dejan de validar

---

# 🧠 8️⃣ Estrategia profesional recomendada

En producción real:

```text
Infraestructura como código
+
Export controlado
+
Versionado en Git
```

Nunca configurar manualmente prod.

---

# 🔄 9️⃣ Rolling update en cluster

Estrategia segura:

1️⃣ Quitar nodo del LB
2️⃣ Actualizar nodo
3️⃣ Validar
4️⃣ Reintegrar
5️⃣ Repetir

No actualizar todos a la vez.

---

# 🔥 10️⃣ Backup lógico vs Backup DB

| Tipo              | Uso             |
| ----------------- | --------------- |
| Export Realm      | Configuración   |
| Backup PostgreSQL | Datos completos |

En producción necesitas ambos.

---

# 🧠 11️⃣ Modelo mental correcto

```text
Base de datos = estado completo
Export realm = configuración portable
```

No confundir.

---

# 🎯 Qué debes retener

* Export es lógico, no físico
* No importar en cluster activo
* Versionar realms
* Cuidar claves criptográficas
* Backup DB sigue siendo obligatorio