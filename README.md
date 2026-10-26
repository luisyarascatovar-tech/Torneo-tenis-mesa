# Torneo de Tenis de Mesa · Todos contra todos

Aplicativo web para organizar torneos de tenis de mesa todos contra todos. Incluye:

- Lista de jugadores del club guardada.
- Calendario por mesas y turnos.
- Resultados en vivo.
- Tabla de posiciones.
- Historial de torneos y ranking acumulado.
- Respaldo de datos.

La página se publica gratis con **GitHub Pages**. Los datos se guardan en **Firebase** (Google), con el plan gratuito.

- **Cualquier persona con el enlace** ve el torneo en vivo, el ranking y el historial.
- **Solo los organizadores**, que inician sesión con su cuenta de Google, pueden anotar resultados y editar datos.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La página del aplicativo. |
| `firebase-config.js` | Conecta la página con tu proyecto de Firebase (paso 3). |
| `firestore.rules` | Reglas de seguridad: quién puede leer y quién puede escribir (paso 2). |
| `README.md` | Esta guía. |

---

## Paso 1 · Subir la página a GitHub

1. Entra a <https://github.com> e inicia sesión (o crea una cuenta gratuita).
2. Arriba a la derecha pulsa **+** › **New repository**.
   - **Repository name:** `torneo-tenis-mesa`
   - Marca **Public**. GitHub Pages es gratis solo para repositorios públicos.
   - Pulsa **Create repository**.
3. En el repositorio nuevo pulsa **uploading an existing file**. Arrastra los cuatro archivos y pulsa **Commit changes**.
4. Ve a **Settings** › **Pages**.
   - En **Source** elige **Deploy from a branch**.
   - En **Branch** elige `main` y la carpeta `/ (root)`. Pulsa **Save**.
5. Espera uno o dos minutos y recarga. Aparecerá la dirección de tu página, parecida a esta:
   `https://TU-USUARIO.github.io/torneo-tenis-mesa/`

Al abrirla verás un aviso amarillo: "Falta conectar Firebase". Es normal, lo resuelves en los pasos 2 y 3. Mientras tanto la página funciona, pero no guarda nada.

## Paso 2 · Crear el proyecto en Firebase

1. Entra a <https://console.firebase.google.com> con tu cuenta de Google.
2. Pulsa **Crear un proyecto** (o **Agregar proyecto**).
   - Ponle un nombre, por ejemplo `torneo-tenis-mesa`.
   - Google Analytics no es necesario; puedes desactivarlo.
   - Pulsa **Crear proyecto**.

### 2a · Base de datos (Firestore)

1. En el menú izquierdo: **Compilación** › **Firestore Database** › **Crear base de datos**.
2. Ubicación: elige `southamerica-east1 (São Paulo)`, la más cercana a Perú. Esta elección no se puede cambiar después.
3. Elige **Comenzar en modo de producción** y pulsa **Crear**.
4. Abre la pestaña **Reglas**:
   1. Borra todo y pega el contenido del archivo `firestore.rules`.
   2. Reemplaza `correo-organizador@gmail.com` por el correo de Google de cada organizador, entre comillas y separados por comas. Por ejemplo:
      ```
      'luis@gmail.com',
      'arbitro@gmail.com'
      ```
   3. Pulsa **Publicar**.

No hace falta poner tu correo en el `firestore.rules` del repositorio. El correo solo tiene que estar en las reglas de la consola de Firebase. Así no queda visible en GitHub.

### 2b · Inicio de sesión con Google

1. En el menú izquierdo: **Compilación** › **Authentication** › **Comenzar**.
2. Pestaña **Método de acceso** › **Google** › activa **Habilitar**, elige tu correo de asistencia y pulsa **Guardar**.
3. Pestaña **Configuración** › **Dominios autorizados** › **Agregar dominio**. Escribe `TU-USUARIO.github.io`, sin `https://` y sin la parte `/torneo-tenis-mesa`.

## Paso 3 · Conectar la página con Firebase

1. En Firebase, pulsa el engranaje ⚙ › **Configuración del proyecto**.
2. En **General**, baja a **Tus apps** y pulsa el ícono web **`</>`**.
3. Ponle un apodo, por ejemplo `web`. **No** marques Firebase Hosting. Pulsa **Registrar app**.
4. Aparecerá un bloque `const firebaseConfig = { ... }`. Copia solo lo que está entre las llaves.
5. En GitHub, abre `firebase-config.js` y pulsa el lápiz ✎ para editarlo:
   1. Reemplaza los valores de ejemplo por los tuyos.
   2. Pulsa **Commit changes**.
6. Espera un minuto y recarga tu página. El aviso amarillo desaparece y arriba aparece el botón **Iniciar sesión para anotar**.

Los datos de `firebase-config.js` no son secretos. Pueden estar en un repositorio público sin problema. Lo que protege la información son las reglas del paso 2a.

## Paso 4 · Probar

1. Abre tu página y pulsa **Iniciar sesión para anotar** con un correo que esté en las reglas.
2. Agrega los jugadores del club, genera un calendario y anota un resultado.
3. Abre la página en otro celular **sin** iniciar sesión. Verás el torneo en vivo, en modo solo lectura.

Si al anotar aparece "Tu cuenta no tiene permiso para anotar", ese correo no está en las reglas de Firestore. Revisa el paso 2a.

---

## Uso diario

- **Compartir con jugadores:** envía la dirección de GitHub Pages. No necesitan cuenta para ver los resultados.
- **Agregar otro organizador:** añade su correo en Firebase › Firestore Database › Reglas y pulsa **Publicar**.
- **Respaldo:** en la pestaña Historial, pulsa **Exportar datos** cada cierto tiempo y guarda el archivo `.json`. Con **Importar respaldo** puedes restaurarlo o pasarlo a otro proyecto de Firebase.
- **Actualizar la página:** reemplaza `index.html` en GitHub (Add file › Upload files). Los datos no se pierden, porque están en Firebase.

## Costos

El plan gratuito de Firebase (Spark) permite unas 50 000 lecturas y 20 000 escrituras al día. Eso alcanza de sobra para torneos de club. No necesitas registrar una tarjeta.
