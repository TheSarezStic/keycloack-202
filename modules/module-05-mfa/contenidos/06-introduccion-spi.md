# 📘 Módulo 5 – MFA

## 06 – Introducción al SPI (Service Provider Interface)

---

# 🎯 Objetivo

Entender cómo extender
**Keycloak**
más allá de configuración, usando:

* SPI (Service Provider Interface)
* Extensiones Java
* Autenticadores personalizados
* Required Actions custom

Aquí ya hablamos de desarrollo interno del servidor.

---

# 🧠 1️⃣ ¿Qué es un SPI?

Un SPI es:

> Un punto de extensión del servidor donde puedes añadir tu propia lógica.

No modificas el código fuente.
Creas un plugin.

Keycloak carga tu extensión al arrancar.

---

# 🏗 2️⃣ Qué puedes extender

Keycloak permite crear:

* Authenticators (pasos nuevos en login)
* Required Actions custom
* User Storage Providers (LDAP custom)
* Event Listeners
* Protocol Mappers
* REST endpoints internos

---

# 🔐 3️⃣ Ejemplo típico: autenticador personalizado

Caso real:

* Validar certificado interno
* Validar cabecera HTTP corporativa
* Validar IP origen
* Validar token externo
* Integración con sistema legacy

Flujo:

```text id="spi1"
Username/Password
   ↓
Custom Authenticator
   ↓
Success
```

---

# 🧩 4️⃣ Arquitectura conceptual

![Image](https://developers.redhat.com/sites/default/files/styles/share/public/blog/2019/11/Keycloak.png?itok=SnoHQmX6)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AMyuRsiL3JwulirvzPMwW2A.png)

![Image](https://framerusercontent.com/images/rCwggZ3VUyjOtFIz4rwBnZxBvYc.png)

![Image](https://deepaksood619.github.io/assets/images/DevOps-Others-KeyCloak-image2-6e5da83278825fa9c79019de3e1131b5.jpg)

Tu código se integra como:

```text id="spi2"
Keycloak Core
   ↓
SPI Interface
   ↓
Tu implementación
```

---

# 🛠 5️⃣ Cómo se desarrolla

1️⃣ Crear proyecto Java (Maven)
2️⃣ Implementar interfaz SPI
3️⃣ Generar .jar
4️⃣ Copiar a:

```text id="spi3"
/opt/keycloak/providers/
```

5️⃣ Reiniciar servidor

Keycloak detecta proveedor automáticamente.

---

# 🔎 6️⃣ Ejemplo conceptual mínimo

AuthenticatorFactory:

```java
public class MyAuthenticatorFactory implements AuthenticatorFactory {
    public String getId() {
        return "my-authenticator";
    }
}
```

Luego implementas lógica en:

```java
public class MyAuthenticator implements Authenticator {
    public void authenticate(AuthenticationFlowContext context) {
        // lógica personalizada
    }
}
```

---

# 🔥 7️⃣ Required Action personalizada

Caso típico:

* Forzar aceptar política interna
* Forzar completar DNI
* Validar atributo obligatorio

Se implementa:

```text id="spi4"
RequiredActionProvider
```

Y se activa como cualquier Required Action estándar.

---

# 🧠 8️⃣ Event Listener SPI

Permite reaccionar a eventos:

* LOGIN
* LOGOUT
* REGISTER
* UPDATE_PASSWORD

Ejemplo:

```text id="spi5"
Login detectado
   ↓
Enviar log a SIEM
```

Muy útil en entornos regulados.

---

# ⚠️ 9️⃣ Cuándo usar SPI (y cuándo NO)

Usar SPI cuando:

✔ Necesitas lógica imposible vía configuración
✔ Integración corporativa profunda
✔ Requisitos regulatorios especiales

No usar SPI cuando:

❌ Puedes resolverlo con flows
❌ Puedes resolverlo con roles
❌ Puedes resolverlo con Authorization Services

---

# 🧠 10️⃣ Riesgos del SPI

* Es código Java
* Depende de versión interna de Keycloak
* Puede romper en upgrades mayores
* Debe testearse bien

Es extensión de servidor, no simple config.

---

# 🎯 Modelo mental final

```text id="mental1"
Configuración → 80% casos
Flows → 95% casos
SPI → 5% casos extremos
```

SPI es el nivel más profundo.

---

# 🎯 Qué debes retener

* SPI permite extender Keycloak
* Se implementa en Java
* Se despliega como provider
* Útil para integraciones avanzadas
* Debe usarse con criterio

---

Con esto cerramos Módulo 5 completo:

✔ MFA
✔ Flows
✔ Required Actions
✔ Hardening
✔ Themes
✔ Extensiones
