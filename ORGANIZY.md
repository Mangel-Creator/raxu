# Lo que Raxu hereda de Organizy

Recopilado el 1/10/2026 leyendo todo Organizy (`C:\proyectos\organizy`: su `CLAUDE.md`,
`BRIEF.md`, `AGENTS.md`, configuración, workflows y servidor) y la carpeta del usuario
(`OneDrive\PERSONAL\Organizy`). Organizy es **solo de lectura**: se copia lo que sirva, nunca
se modifica.

Este archivo dice **qué existe ya, qué funciona, qué trampas se pisaron y cómo se
resolvieron**, para no repetirlas. Para el detalle de una pieza concreta, la fuente es el
`CLAUDE.md` de Organizy (se cita la sección entre comillas).

---

## 1. Cómo trabaja el usuario

Esto vale para Raxu igual que para Organizy:

- **Es principiante.** Hay que explicarle todo en español sencillo y paso a paso. Lo que
  tenga que hacer él fuera del código (crear cuentas, poner claves) se le explica despacio.
  **Nunca se le piden contraseñas ni claves en el chat**: las escribe él en el panel de cada
  servicio o en su terminal.
- **Una sesión por tema.** En Organizy se llamaban "Organizy · NN Nombre" y estaban en un
  grupo de la barra lateral. En Raxu, la skill `/app` las crea y las numera, y lleva el
  registro en `.claude/app-sesiones.md`.
- **Sus indicaciones valen para todas las sesiones.** Si afectan a todo el proyecto, se
  apuntan en `CLAUDE.md` > "Decisiones del usuario", se hace commit en `main` y se avisa a las
  otras sesiones abiertas (ListAgents + SendMessage). Si dos indicaciones chocan, manda la más
  reciente; si hay duda, se le pregunta.
- **Sin worktrees.** Obligan a reinstalar `node_modules` y a fusionar, y en Organizy causaron
  líos: librerías que no llegaban a la carpeta principal, la web enseñando rutas viejas…
- **Al terminar cada parte, la web y Expo Go tienen que quedar al día y funcionando.** Se
  sube a `main` y se comprueba que se publica. Lo decidió el 25/09, después de no poder abrir
  la app en el iPhone.
- **Su carpeta de OneDrive, ordenada.** Al terminar cada parte se dejan allí su guía de
  prueba y su resumen. Nada suelto en la raíz.
- **No quiere pagar todavía la cuenta de Apple (99 €/año).** Prueba en Expo Go y la web. No
  se lo propongas salvo que lo pida o una función lo necesite de verdad. Tampoco tiene Android.
- **La clave de Anthropic la pondrá él más adelante** (5 $ de saldo). No se la vuelvas a
  pedir: todo lo de IA tiene que funcionar sin ella y encenderse solo cuando la ponga.
- **Gustos de diseño.** Lo que "se nota hecho por una IA" le chirría, sobre todo las
  tarjetas todas iguales. Quiere la app **visual y poco cargada**, con formularios cortos
  (lo esencial a la vista y lo opcional plegado) y valores por defecto en vez de preguntas.
- **Tono de la app.** Tuteo, frases cortas y algún giro coloquial suave ("¿Qué toca hoy?",
  "Día libre"). Sin emojis, sin exclamaciones y sin "Buenos días".
- **Licencia.** El código es suyo, con todos los derechos reservados (`LICENSE` +
  `"license": "UNLICENSED"`). No pongas una licencia libre sin que lo pida.
- Tiene 100 € de créditos para sesiones en la nube. Quiere gastarlos en construir cosas que
  sigan funcionando gratis cuando caduquen, nada recurrente.

---

## 2. Cuentas y servicios que ya existen

| Servicio | Lo que hay | Para Raxu |
|---|---|---|
| GitHub | Usuario `Mangel-Creator`, con `gh` ya autenticado en este ordenador. Repo público `Mangel-Creator/organizy` (público por GitHub Pages). | Repo nuevo `Mangel-Creator/raxu`. **Pregúntale antes de crearlo.** |
| GitHub Pages | Web en `https://mangel-creator.github.io/organizy/` | Será `https://mangel-creator.github.io/raxu/`, con `baseUrl` `/raxu`. |
| Expo | Cuenta `mangel_creator`, con la CLI ya iniciada. Proyecto `@mangel_creator/organizy` (id `bf4c1bdd-…`). | Proyecto nuevo con `eas init`; su id saldrá en `app.json`. |
| Supabase | Proyecto de Organizy `hwemrpexabisyueyizjz`. La CLI la inicia el usuario (`npx supabase login`). | **Proyecto nuevo** para Raxu (decidido). Lo crea el usuario. |
| TomTom | Cuenta del usuario, plan Evaluation (gratis y sin tarjeta). La clave de rutas "My First API key" (rotada el 27/09) está en los secretos de Supabase de Organizy como `TOMTOM_KEY`. Hay otra clave **pública**, solo para mapas y restringida al dominio `mangel-creator.github.io`. | La de rutas se puede reutilizar: el usuario la pega en los secretos del Supabase nuevo y **comparten el cupo diario**. La pública vale tal cual, porque Raxu va en el mismo dominio. |
| Anthropic | Consola creada; **la clave aún no está puesta**. | Igual: la pondrá él. Todo debe funcionar sin ella. |
| Meta / WhatsApp Business | No hay cuentas. | Para la primera versión no hacen falta (WhatsApp con el mensaje escrito). |
| Google Cloud / Microsoft Entra | Sin registrar (pendiente del usuario en Organizy). | Solo si algún día hace falta entrar con Google o vincular cuentas. |

**Secretos de GitHub y variables.** `EXPO_TOKEN` es un secreto que crea y pega el usuario en
cada repo (expo.dev > Access tokens). `SUPABASE_URL`, `SUPABASE_KEY` y `TOMTOM_MAPA_KEY` son
**variables** (no secretos) del repo: Settings > Secrets and variables > Actions > Variables.
Los workflows las pasan como `EXPO_PUBLIC_*`.

---

## 3. Tecnología y versiones exactas de Organizy

- **Expo SDK 57** (`expo ~57.0.24`), React Native 0.86.3, React 19.2.3, TypeScript ~6.0.3,
  Expo Router ~57.0.22, React Compiler activado (`experiments.reactCompiler`) y
  `typedRoutes`.
- Instala siempre con `npx expo install <paquete>`. Antes de tocar una API de Expo, lee la
  documentación de la v57 (`AGENTS.md` de Organizy: "Expo has changed — do not trust your
  training data"). Conviene copiar ese `AGENTS.md` y cargarlo desde `CLAUDE.md` con
  `@AGENTS.md`.
- Librerías que Raxu probablemente necesite, todas incluidas en Expo Go:

  | Para qué | Librerías |
  |---|---|
  | Guardar datos | `expo-sqlite`, `@react-native-async-storage/async-storage` 2.2.0 |
  | Avisos y ubicación | `expo-notifications`, `expo-location` |
  | Abrir WhatsApp, Waze y Maps | `expo-linking`, `expo-web-browser` |
  | Copia de seguridad | `expo-file-system`, `expo-sharing`, `expo-document-picker` |
  | Calendarios iCal | `ical.js` |
  | Servidor | `@supabase/supabase-js` |
  | Letras e iconos | `@expo-google-fonts/ibm-plex-sans`, `@expo-google-fonts/ibm-plex-mono`, `@expo/vector-icons` |
  | Animaciones y gestos | `react-native-reanimated` 4.5.1 (+ `react-native-worklets`), `react-native-gesture-handler` |
  | Mapas | `react-native-maps` 1.27.2 en el móvil, `leaflet` en la web |
  | Pruebas | `jest ~29.7.0`, `jest-expo ~57.0.5`, `@types/jest 29.5.14` |

- **Copia la configuración tal cual, cambiando el nombre:**
  - `tsconfig.json`: `strict`, alias `@/*` → `./src/*` y `exclude` de `supabase`.
  - `eslint.config.js`: `eslint-config-expo/flat`, sin mirar `dist/*` ni `supabase/*`.
  - `metro.config.js`: bloquea la carpeta `.claude/`, porque recorrerla dejaba a Metro sin
    memoria.
  - Jest en `package.json`: preset `jest-expo`, `moduleNameMapper` para `@/`,
    `testPathIgnorePatterns` con `/dist/` y `<rootDir>/.claude/`.
  - Las pruebas importan `describe`, `it` y `expect` de `@jest/globals`, porque TypeScript 6
    no carga los tipos globales solo. Van junto al código, en carpetas `__tests__`.
  - `eas.json`: `appVersionSource: remote`. Perfil `preview` (interno; APK en Android, canal
    `preview`) y perfil `production` (`autoIncrement`, canal `production`).
- **`app.json` de Raxu, siguiendo a Organizy:**
  - `scheme: "raxu"`, `userInterfaceStyle: "light"`, `web.output: "static"`,
    `experiments.baseUrl: "/raxu"`, `owner: "mangel_creator"`.
  - `ios.bundleIdentifier` y `android.package` = `com.mangelcreator.raxu`. **No se puede
    cambiar después de publicar**: decídelo antes de ir a las tiendas.
  - `ios.infoPlist.ITSAppUsesNonExemptEncryption: false`, porque solo usa HTTPS y así App
    Store Connect no pregunta por el cifrado en cada subida.
  - Frases de permiso en español, que digan para qué sirve cada uno. Ver el `app.json` de
    Organizy (ubicación, micrófono, contactos).
- **Rutas y estructura:** rutas en `src/app/`, que solo reexportan pantallas. Pantallas en
  `src/screens/`, piezas en `src/components/` (se importan desde `@/components`), tema en
  `src/theme/`, guardado en `src/data/` y lógica sin pantalla en `src/services/`.

---

## 4. Publicar: web y Expo Go

### Web en GitHub Pages (`.github/workflows/pages.yml`)

- Se ejecuta en cada subida a `main`, con Node 24: `npm ci` → `npm test` (si falla una
  prueba, no se publica) → `npx expo export --platform web` con las variables
  `EXPO_PUBLIC_*`.
- Después crea `dist/.nojekyll`, porque Pages ignora las carpetas que empiezan por `_`.
- Copia `dist/index.html` a `dist/404.html`, para que funcionen las rutas abiertas
  directamente.
- Al final, `upload-pages-artifact@v3` + `deploy-pages@v4`.
- **Rutas relativas.** Como la web cuelga de `/raxu/`, nada puede empezar por `/` a pelo.
- **La web se actualiza sola** (`src/services/actualizacionWeb.ts`): al abrirla, o al volver
  tras 30 s fuera, compara el archivo principal publicado y recarga una vez por versión.
- **En la web del móvil hay que añadirla a la pantalla de inicio.** Si no, Safari borra sus
  datos tras 7 días sin usarla. Organizy lo explica con `AvisoPantallaInicio` y con
  `src/app/+html.tsx`.

### Expo Go sin el ordenador (`.github/workflows/expo-go.yml`)

- **Funciona y el usuario lo usa a diario desde el 26/09.** Expo Go sí carga publicaciones
  de EAS Update de un proyecto propio si su `runtimeVersion` es `exposdk:57.0.0`.
- El workflow pone ese `runtimeVersion` **solo durante la publicación** y publica:
  `eas update --branch expo-go --platform all --environment production`, con
  `EAS_SKIP_AUTO_FINGERPRINT=1`.
- La dirección fija sigue este patrón:
  `exp://u.expo.dev/<projectId>?runtime-version=exposdk%3A57.0.0&channel-name=expo-go`.
  Para Raxu, guarda la suya y su QR en `OneDrive\PERSONAL\Raxu\QR y enlaces`.
- Si falta el secreto `EXPO_TOKEN`, el trabajo se salta con un aviso, sin ponerse en rojo.
  Mientras tanto se puede publicar a mano desde una copia hecha con `git archive`, con
  `node_modules` enlazado y `EAS_NO_VCS=1`.
- **Reglas:**
  - **No pongas `runtimeVersion` en `app.json`**: rompe Expo Go en local.
  - **No instales `expo-updates`** mientras se use Expo Go.
  - Si se sube de SDK, cambia `RUNTIME_EXPO_GO` en el workflow.
  - Cada variable `EXPO_PUBLIC_*` nueva añádela también a este workflow.

### Túnel (para ver cambios al momento; solo con el ordenador encendido)

- Organizy usa `preview_start` "organizy-tunel" en el puerto 8083 y la web en el 8081.
- La dirección fija sale de `.expo/settings.json` (`urlRandomness`, que no está en git: no
  lo borres), más el puerto y la cuenta de Expo.
- **Para Raxu usa otros puertos** para que las dos apps puedan ir a la vez; por ejemplo,
  web 8091 y túnel 8093. Después, nunca cambies el puerto, porque cambiaría la dirección.
- **Trampa del 25/09:** la app de Claude apaga los servidores de `preview_start` cuando
  termina la sesión que los arrancó. Además, si otra sesión añade una librería y no se
  instala, la app no compila. Organizy lo comprueba con `scripts/comprobar-expo-go.ps1`:
  instala lo que falte, mira que el túnel responda y pide la app como el iPhone, y tiene que
  acabar en "OK". Cópialo adaptando la ruta y la dirección.

---

## 5. El servidor (Supabase), como se montó en Organizy

- **Los datos del usuario se quedan en su dispositivo.** El servidor solo se usa para lo que
  necesita claves secretas (TomTom, Claude) y no guarda frases ni eventos: solo cuenta usos.
  Cada excepción a esta regla se apuntó en `CLAUDE.md` con el permiso del usuario.
- **Claves:**
  - En la app solo van la URL y la clave **pública** (`sb_publishable_...`), en `.env` (no
    va a git; plantilla en `.env.example`) y en las variables del repo.
  - Las secretas (`ANTHROPIC_API_KEY`, `TOMTOM_KEY`, `sb_secret_...`) van solo en los
    secretos de Supabase.
  - Después de cambiar `.env`, reinicia el servidor de Expo, túnel incluido.
- **Usuario anónimo, sin registro:**
  - `src/data/supabase.ts`: `obtenerSupabase()` devuelve null si no hay `.env`, y la app
    funciona igual sin lo del servidor. `asegurarSesion()` crea la sesión anónima y la
    guarda en AsyncStorage.
  - Hay que activar "Anonymous" en Authentication > Sign In / Providers.
  - Límite: 30 altas anónimas por hora y por IP.
  - Ojo: si se borran los datos del navegador, sale un usuario nuevo con los límites a
    cero. Para límites de verdad por persona (y para cobrar) hará falta iniciar sesión.
- **Funciones** (`supabase/functions/<nombre>/index.ts`, en Deno):
  - Cada una lleva `verify_jwt = false` en `config.toml` y comprueba al usuario por dentro
    (`_shared/usuario.ts`). Así pasa la petición previa (OPTIONS) de CORS.
  - `_shared/cors.ts` solo deja entrar a `https://mangel-creator.github.io` y
    `http://localhost:*`.
  - `_shared/limite.ts` (`sumarUso`) pone un límite por usuario y día y otro global.
  - Desplegar: `npx supabase functions deploy <nombre> --project-ref <ref> --use-api`.
- **Base de datos:**
  - Migraciones en `supabase/migrations/`. Se aplican pegando el SQL en el SQL Editor o con
    `npx supabase db query --linked --project-ref <ref> --file <archivo.sql>`.
  - Las reglas (RLS) se prueban con SQL que se deshace solo (`supabase/tests/`).
  - **Trampa de las reglas:** las de "ver" tienen que mirar las columnas de la fila, no
    buscarla por su id. Al crearla con `insert(...).select()` todavía no se ve y la regla
    falla.
  - El borrado automático se hace con `pg_cron`; las llamadas del servidor hacia fuera, con
    `pg_net`.
- **IA (Claude):**
  - Siempre a través del servidor, nunca con la clave en la app. Modelo Claude Haiku 4.5
    (`claude-haiku-4-5`), con salida estructurada (JSON con esquema); se cambia en el
    servidor sin publicar otra versión.
  - La app vuelve a validar lo que devuelve la IA (`services/captura/validar.ts`).
  - Sin `ANTHROPIC_API_KEY`, la función contesta 503 `sin-clave` sin gastar nada y la app
    dice "aún no está encendida". Se enciende sola al poner la clave.
  - **Límites de IA como los de Claude** (`_shared/limiteIA.ts` + migración
    `20260929000000_limites_ia.sql`): una ventana de 5 horas y otra semanal, contadas en
    millonésimas de dólar. **Toda llamada a Claude pasa por `permitirIA` antes y por
    `apuntarIA` después.** La app lo enseña en porcentaje (`services/ia/`).
  - Coste de la captura con IA: unos 0,2 céntimos de dólar por frase, unas 500 frases por
    dólar.
- **Antes de las tiendas:** Apple pide una pantalla de permiso (una vez, con "Ahora no")
  antes de mandar datos personales a una IA de otra empresa, y que se mencione en la
  política de privacidad. Organizy lo tiene pendiente; en Raxu hazlo desde el principio.

---

## 6. Piezas de Organizy aprovechables para Raxu

El estado viene de lo que dice el `CLAUDE.md` de Organizy. Antes de copiar, lee el archivo:
puede haber cambiado.

### Para la primera versión (sesiones 02 a 05)

| Pieza | Dónde, en Organizy | Qué hace |
|---|---|---|
| Tema y componentes | `src/theme/index.ts`, `src/components/*` | `Boton`, `Tarjeta`, `Titulo`, `Texto`, `CampoTexto`, `Selector`, `SelectorVisual`, `SelectorDias`, `SelectorHora`, `SelectorFecha`, `SelectorCantidad`, `Plegable`, `Interruptor`, `Casilla`, `BotonFlotante`, `Pantalla`, `BarraProgreso`, `AvisoPantallaInicio`, `BotonInicial`, `Proximamente` |
| Guardar ajustes | `src/data/ajustes.ts` | `leerAjuste`, `guardarAjuste` y `borrarAjuste` en AsyncStorage, con prefijo (en Raxu, `raxu:`) |
| Base de datos del móvil | `src/data/db.ts` | `obtenerBD()` y migraciones numeradas con `PRAGMA user_version`. Para crear tablas, añade una función al final de `MIGRACIONES` |
| Eventos (citas) | `src/data/eventos/` | Tipo `Evento`, `useEventos()`, `guardarEvento`, `borrarEvento`… El guardado real está en `repositorio.ts` (SQLite) y `repositorio.web.ts` (AsyncStorage), y **Metro elige el archivo según la plataforma** |
| Fechas en español | `src/services/fechas.ts` | Formatos, semana que empieza en lunes, 24 h y `saludoSegunHora` |
| Repeticiones y huecos | `src/services/agenda/` (puro, con pruebas) | `ocurreEnDia` (cada día, semana o mes), `eventosDelDia`, `calcularHuecos`, `cargaDelDia`, `siguienteEvento`, `resolverLugar`. Los tiempos van en minutos desde medianoche |
| Dirección → coordenadas | `src/services/lugares.ts` | `buscarCoordenadas`: `Location.geocodeAsync` en el móvil (sin permiso de ubicación) y Nominatim de OpenStreetMap en la web. Sin conexión guarda `coordenadas: null` y las completa después |
| Permisos y ubicación | `src/services/permisos.ts`, `src/services/ubicacion.ts` | `ubicacionActual()` nunca pide permiso: solo la usa si ya lo hay |
| Avisos locales | `src/services/avisos/` (con pruebas) | `planificarAvisos` recorre de ayer a 7 días con `GENERADORES` y se queda con 60 (`MAX_AVISOS`; iOS admite 64). `programar.ts` / `programar.web.ts`. `iniciarAvisos()` en `_layout` |
| Recordatorio a clientes | `src/services/clientes/`, `src/screens/clientes/`, `src/data/recordatorios.ts`, `src/services/avisos/clientes.ts` | `telefonoWhatsapp` (9 cifras → prefijo 34), `puedeRecordar` (solo si el cliente acepta), `textoRecordatorioCliente`, estado `enviado` / `fallido` / `pendiente` ("nunca dos veces") |
| Tráfico y hora de salida | `src/services/rutas/`, `src/data/salidas.ts`, `supabase/functions/rutas` | `calcularRutas` (con caché de 5 min), `horaDeSalida` (llegada − trayecto − 5 min), `proximasCitas`, `hayQueRecalcular`, `iniciarTrafico()`, `useSalida(eventoId, dia)`. El servidor usa TomTom con `arriveAt` |
| Enlaces | `src/services/rutas/textos.ts` | `enlaceWaze`, `enlaceGoogleMaps` y `enlaceWhatsapp(texto, telefono)` → `wa.me/34…?text=`. `MENSAJE_RETRASO` es el "Voy con unos 10 min de retraso" del "Sal ya" |
| WhatsApp en la web | `prepararWhatsapp()` en `screens/planes/piezas.tsx` | Abre la pestaña **en el mismo toque** y le pone la dirección después; si no, el navegador la bloquea |
| Traer Google / iCloud | `src/services/calendarios/` (con pruebas), `supabase/functions/calendario` | `ics.ts` (con `ical.js`) y `sincronizar`, que respeta lo cambiado aquí. En el móvil se descarga directamente; en la web, a través de la función, porque el navegador no deja leerlo |
| Copia de seguridad | `src/services/copia/`, `screens/perfil/SeccionCopia.tsx` | Un archivo JSON que guarda la persona. `NO_VIAJAN` excluye lo calculado y lo propio de cada dispositivo. Recuperar cambia todo y reinicia la app |
| Supabase en la app | `src/data/supabase.ts` | `obtenerSupabase`, `asegurarSesion` |
| Servidor común | `supabase/functions/_shared/` | `cors.ts`, `usuario.ts`, `limite.ts`, `limiteIA.ts` + migraciones `usos_diarios` y `limites_ia` |
| Comprobar Expo Go | `scripts/comprobar-expo-go.ps1` | Ver el apartado 4 |

### Para más adelante

- **Captura con IA (apuntar hablando):** `src/services/captura/`, `src/screens/captura/` y la
  función `captura`. Aún no se ha probado con la clave.
- **Dictado por voz:** `src/services/dictado/`. Funciona en la web y en la app propia, no en
  Expo Go.
- **Alarma inteligente:** `src/services/alarmas/` y `src/data/alarmas.ts`. Las alarmas de
  verdad solo funcionan en la app propia.
- **Límites de IA en Perfil:** `screens/perfil/SeccionIA.tsx`.

---

## 7. Trampas ya pisadas (no las repitas)

### Web y Expo Go

- **SQLite no funciona en GitHub Pages**: necesita `SharedArrayBuffer` y cabeceras que Pages
  no permite. Por eso hay dos versiones: SQLite en el móvil y AsyncStorage en la web, con la
  misma interfaz (`archivo.ts` / `archivo.web.ts`). Nada de la web importa `db.ts`.
- **`Alert` no hace nada en la web.** Las confirmaciones son tarjetas dentro de la pantalla.
- **No hay selector de hora nativo** que funcione igual en Expo Go y en la web. Se usan
  botones − y + de 15 en 15 minutos (`SelectorHora`).
- **Módulos nativos que Expo Go no trae** (alarmas, dictado…): se cargan con
  `requireOptionalNativeModule('Nombre')` de `expo`, que devuelve null en Expo Go. Así la
  app no se rompe y se puede ocultar o explicar lo que no funciona.
- **En la web no hay avisos**: el navegador no puede con la web cerrada. Hay que decirlo en
  pantalla.
- **Push en Expo Go de Android**: no existen desde el SDK 53. En Expo Go de iPhone sí.
- **`Linking.createURL` no es estable** en Expo Go con EAS Update. Los enlaces de vuelta van
  a la web.
- **`.expo/types` anticuado**: si `tsc` se queja de una ruta nueva, se regenera al arrancar
  `expo start`.

### Avisos y segundo plano

- **iOS no deja ejecutar código desde un botón de una notificación** con la app cerrada (ni
  en Expo Go): el botón abre la app y la app hace lo que toque. Por eso "llego tarde" y el
  recordatorio abren la app, que abre WhatsApp.
- **Nada fiable en segundo plano**: iOS lanza las tareas cuando quiere (o nunca) y Expo Go
  no las permite. Lo que funciona: programar el aviso con el tráfico **previsto** para esa
  hora (TomTom `arriveAt`) y recalcular al abrir la app, al volver a ella, al cambiar datos
  y cada 10 minutos con ella abierta.
- **El límite de 64 avisos de iOS**: Organizy se queda con 60. Si algo es más importante
  (como las alarmas), se reserva su sitio antes de recortar.
- **El sonido de los avisos**: `sound: 'default'`. Un sonido propio no funciona en Expo Go y
  no suena con el móvil en silencio.

### Configuración y servidor

- **`expo-audio` con `microphonePermission: false`** borra el permiso del micro del iPhone,
  y la app se cierra al dictar.
- **El plugin de `react-native-alarm-scheduler` 1.0.1 rompe `expo prebuild` de Android.**
  Organizy usa uno propio (`plugins/alarmas.js`).
- **WhatsApp:** la app nunca envía sola ni lee chats. Abre `wa.me` con el texto escrito y lo
  envía la persona. Como no puede saber si de verdad se envió, el estado dice "enviado" al
  abrir WhatsApp y hay un "No llegué a enviarlo".
- **Nominatim (OpenStreetMap):** como mucho una búsqueda por segundo, nunca mientras se
  escribe, y con la atribución visible (`ATRIBUCION_OPENSTREETMAP`).
- **TomTom no calcula transporte público.** Con ese modo no se piden rutas y se ofrece Google
  Maps. Waze no da datos a otras apps: solo se abre con un enlace.
- **Mapa en Android (app propia):** necesitará la clave de Google Maps SDK. En iPhone no.

### Código

- **React Compiler:** con Reanimated se usa `valor.get()` y `valor.set()`, no `valor.value`.
  A las funciones que combinan datos se les pasa todo lo que usan, porque si no el
  compilador se queda con lo viejo.
- **Lugares por referencia:** Casa y los sitios del perfil se guardan como referencia
  (`{ tipo: 'sitio', sitioId }`), no como copia. Así, si cambia la dirección, las citas se
  actualizan solas.
- **El mensaje al cliente lleva la dirección**, no el nombre del sitio: el cliente no sabe qué
  es "Oficina" en tu perfil.
- **Copia de seguridad:** cada clave nueva de AsyncStorage que sea propia del dispositivo o se
  calcule sola va a `NO_VIAJAN`. Si no, viaja en la copia.

### Tiendas y marcas

- **Marcas de terceros:** los botones de entrar con Google o Microsoft tienen que ser los
  oficiales. El logo de Apple no se puede usar en la app.
- **Google Play:** hay que declarar los permisos especiales (pantalla completa, servicio en
  primer plano, ubicación en segundo plano). Antes de publicar, quita con
  `android.blockedPermissions` los que Expo añade y no se usan.

---

## 8. Diseño de Organizy (punto de partida; Raxu puede tener el suyo)

Raxu necesitará su propia identidad (nombre, icono y quizá colores). De Organizy merece la
pena conservar **las reglas**, que gustaron al usuario. Está explicado en el `BRIEF.md` de
Organizy y en `OneDrive\PERSONAL\Organizy\Diseño\Brief de diseño.md`.

- **"Agenda nocturna":**
  - Cabecera oscura (tinta `#1A1C24`) a todo lo ancho en Hoy, con "Lo siguiente" dentro.
  - Filas planas y blancas con una barra de 4 px del color de su tipo, en vez de tarjetas.
  - Esquinas rectas: 6 px en chips y campos, 8 en botones y 10 en cajas.
  - Sin degradados y solo modo claro.
- **Letra:** IBM Plex Sans. **Las horas, en IBM Plex Mono**, como un reloj.
- **Cada color significa una sola cosa.** El azul tinta `#2B4BD8` es pulsar; el granate
  `#A1172F`, aviso o error. El verde `#25D366` solo en los botones de WhatsApp, con texto
  oscuro encima. Fuera del mapa, los colores del tráfico no se usan.
- **Hoy como panel de casillas grandes** con icono y número: al tocar una, su lista sale
  debajo. Lo pidió el usuario porque la app estaba "supercargada".
- **Formularios cortos:** `SelectorVisual` con iconos para lo esencial, y `Plegable` ("Más
  ajustes") con un resumen para lo opcional.
- **Detalles:**
  - Todo lo pulsable mide como mínimo 44 px.
  - Nada de `opacity` para apagar texto (baja del contraste mínimo): usa `textoSecundario`.
  - Animaciones pocas y suaves con Reanimated (`LinearTransition`, un pequeño salto al
    marcar). Nada infinito.

---

## 9. Costes y límites conocidos

- **TomTom gratis:** 2.500 peticiones al día **entre todos los usuarios** y 50.000 trozos de
  mapa al día. Si se pasa, deja de responder hasta el día siguiente y nunca cobra.
  - Organizy gastaba unas 15-30 peticiones por persona y día, así que daba para unas 80-150
    personas.
  - Si Raxu y Organizy comparten la clave, comparten el cupo.
  - Raxu calculará más trayectos (entre cada par de citas), así que hay que vigilarlo.
- **Supabase gratis:** 500.000 llamadas a funciones al mes y 30 altas anónimas por hora y
  por IP.
- **Claude Haiku 4.5:** unos 0,2 céntimos de dólar por frase en la captura. Conviene poner
  un límite de gasto mensual en la consola de Anthropic.
- **WhatsApp Business (Cloud API):** unos 0,017 € por mensaje de plantilla de "Utilidad" en
  España (tarifa del 1/07/2026). Exige una cuenta de Meta, un número propio y una plantilla
  aprobada. El plan está en el `CLAUDE.md` de Organizy, "Fase 10" > "Parte B".
- **Apple:** 99 €/año. **Google Play:** 25 $ una sola vez.

---

## 10. Lo que Organizy dejó pendiente y afecta a Raxu

- Antes de publicar en las tiendas: **comprobar el nombre en TMview**, **hacer un icono
  propio** (Organizy usaba el de la plantilla de Expo), escribir la política de privacidad y
  la pantalla de permiso de la IA.
- **Cobrar:** todavía no hay nada. RevenueCat para Apple y Google, o Stripe en la web (Stripe
  pide ser autónomo). En Organizy, el saldo extra de IA ya existe en el servidor
  (`ia_recargar`), pero está apagado.
- **Sin probar en el iPhone:** la copia de seguridad (menú de compartir y selector de
  Archivos), el micro con su voz y la IA con la clave.
- **"Llego tarde" en Organizy** es el botón "Avisar de retraso" del aviso "Sal ya", **sin el
  teléfono del cliente**: abre `wa.me/?text=` y la persona elige el chat. En Raxu tiene que
  ir directo al chat del cliente, que ya estará en su ficha.
