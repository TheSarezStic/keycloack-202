# 📘 Módulo 7 – Security

## 02 – Estrategias de expiración y gestión de tokens

---

# 🎯 Objetivo

Diseñar correctamente:

* Expiración de Access Tokens
* Vida de Refresh Tokens
* Sesiones SSO
* Timeout de inactividad
* Revocación

Todo en **Keycloak**

Aquí ya hablamos como arquitectos.

---

# 🧠 1️⃣ Problema real

Si los tokens duran demasiado:

❌ Riesgo alto si se filtran
❌ Replay attacks
❌ Acceso prolongado no deseado

Si duran demasiado poco:

❌ Mala experiencia de usuario
❌ Refrescos constantes
❌ Sobrecarga innecesaria

Necesitas equilibrio.

---

# 🔐 2️⃣ Tipos de expiraciones en Keycloak

En:

```text
Realm Settings → Tokens
Realm Settings → Sessions
```

Parámetros importantes:

| Parámetro              | Qué controla             |
| ---------------------- | ------------------------ |
| Access Token Lifespan  | Vida del JWT             |
| Refresh Token Lifespan | Vida del refresh         |
| SSO Session Idle       | Tiempo sin actividad     |
| SSO Session Max        | Duración máxima absoluta |
| Client Session Idle    | Tiempo por cliente       |

---

# 🔎 3️⃣ Estrategia recomendada para SPA

Modelo seguro típico:

| Elemento      | Valor recomendado |
| ------------- | ----------------- |
| Access Token  | 2–5 minutos       |
| Refresh Token | 30–60 minutos     |
| SSO Idle      | 30 minutos        |
| SSO Max       | 8 horas           |

Esto limita impacto si el access token se filtra.

---

# 🏗 4️⃣ Modelo de riesgo

```text id="risk1"
Token robado
   ↓
Validez corta
   ↓
Ventana de ataque limitada
```

Access tokens deben ser cortos porque:

→ Son portadores de permisos.

---

# 🔄 5️⃣ Refresh Token Rotation (concepto avanzado)

Algunas arquitecturas implementan:

* Rotación de refresh tokens
* Invalidación tras uso

Keycloak permite configurar políticas de revocación y reuso.

Esto mitiga replay de refresh token.

---

# 🔥 6️⃣ Sesión SSO vs Token

Muy importante:

```text id="mental1"
Access Token ≠ Sesión
```

La sesión vive en Keycloak.

Aunque el access token expire:
→ Si la sesión sigue viva, puede refrescar.

---

# ⚠️ 7️⃣ Diseños inseguros comunes

❌ Access token de 1 hora
❌ Refresh token sin límite
❌ SSO Max demasiado largo
❌ No forzar re-login tras cambio de password

---

# 🔐 8️⃣ Revocación

Opciones disponibles:

* Revocar sesiones manualmente
* Revocar sesiones por usuario
* Forzar logout global
* Revocar tras cambio de password

En incidentes de seguridad es crítico.

---

# 🧠 9️⃣ Estrategia por tipo de aplicación

| Tipo app             | Access    | Refresh           |
| -------------------- | --------- | ----------------- |
| SPA pública          | Muy corto | Medio             |
| Backend confidencial | Medio     | Medio             |
| Machine-to-machine   | Muy corto | No siempre aplica |
| Alta criticidad      | Muy corto | Corto             |

---

# 🏢 10️⃣ En entornos regulados (sanidad / banca)

Recomendado:

* Access token ≤ 2 minutos
* MFA obligatorio
* SSO Idle ≤ 15 minutos
* Reautenticación para acciones críticas

---

# 🧩 11️⃣ Arquitectura segura final

```text id="securearch"
Login
   ↓
Access Token corto
   ↓
Refresh gestionado por SPA
   ↓
Sesión central controlada
   ↓
Revocación posible
```

---

# 🎯 Qué debes retener

* Access tokens deben ser cortos
* Refresh mantiene sesión
* Sesión vive en Keycloak
* Diseñar expiraciones según riesgo
* Revocación es clave en incidentes
