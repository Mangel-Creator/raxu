# Raxu

Asistente para **autónomos que van a casa de sus clientes** (fisio, entrenador personal,
profesor particular, técnico, peluquería a domicilio…). En español de España.

Nació el 1/10/2026 de una sesión de estrategia de Organizy: el usuario vio que Organizy
intentaba servir a demasiados públicos y eligió centrarse en este. Lo que se hace, lo que no
y por qué está en [IDEA.md](IDEA.md). **Léelo antes de proponer nada.**

**Raxu es un nombre de trabajo** (comprobado el 1/10/2026: ninguna app con ese nombre en la
App Store; `raxu.app`, `raxu.io` y `raxu.ai` libres; `raxu.com` y `raxu.net` cogidos; marca
registrada sin comprobar). Se puede cambiar hasta publicar en las tiendas. Antes de publicar:
comprobar la marca en TMview y registrarla.

## Propuesta de valor

> "Tu agenda sabe dónde estás y cuánto tráfico hay: llegas a tiempo, avisas cuando te
> retrasas, no pierdes llamadas y metes más clientes al día sin hacer más kilómetros."

**Regla para añadir algo**: ¿ayuda a un autónomo que va a domicilio a no perder clientes,
tiempo ni dinero en su día de trabajo? Si no, fuera. Si el usuario propone algo que no la
cumple, díselo antes de hacerlo (Organizy acabó haciendo de todo y eso es lo que se evita).

## Normas de trabajo

- El usuario es principiante: explica en español sencillo y paso a paso.
- Si algo no se puede hacer como se pide, dilo antes de empezar y propón lo más sencillo.
- Si el usuario tiene que hacer algo fuera del código (crear una cuenta, conseguir una
  clave, instalar algo), para y explícaselo paso a paso.
- Nunca escribas claves secretas en el código ni las subas a git.
- Cambios pequeños y probados. No rehagas lo que ya funciona.
- Antes de dar algo por terminado: `npx tsc --noEmit`, lint y pruebas sin errores.
- Windows + PowerShell 5.1: encadena comandos con `;`, no con `&&`. Mensajes de commit
  en un archivo y `git commit -F`.
- **Sin worktrees**: se trabaja directamente en `C:\proyectos\raxu`, rama `main`.

## Sesiones

El usuario trabaja con una sesión de Claude por tema. Los mensajes de arranque de cada una
están en `C:\Users\usuario\OneDrive\PERSONAL\Raxu\Prompts` (`Raxu-NN-tema.md`). Las partes
nuevas se abren con la skill `/app`, que numera las funciones ("06 · Bonos") y deja las
herramientas sin número ("Supabase"); el registro de sesiones está en
[.claude/app-sesiones.md](.claude/app-sesiones.md).

- Al empezar o retomar, relee este archivo e `IDEA.md`: pueden haber cambiado.
- Si el usuario da una indicación que afecta a todo el proyecto, apúntala aquí (en
  "Decisiones del usuario"), haz commit en `main` y avisa a las otras sesiones de Raxu
  abiertas (ListAgents + SendMessage) con un resumen corto.
- Si una indicación nueva choca con otra anterior, manda la más reciente; si hay duda,
  pregunta.

## Organizy, como referencia (solo lectura)

En `C:\proyectos\organizy` está la app anterior del usuario (Expo SDK 57, TypeScript, Expo
Router, Supabase). **Puedes leerla y copiar lo que sirva, pero nunca la modifiques.** Su
`CLAUDE.md` explica cada pieza.

**[ORGANIZY.md](ORGANIZY.md) resume todo lo que Raxu hereda**: cómo trabaja el usuario, cuentas
y servicios que ya existen, versiones y configuración exactas, cómo se publica la web y Expo Go
sin ordenador, el servidor, piezas reutilizables con sus funciones, trampas ya resueltas,
diseño, costes y pendientes. **Léelo antes de montar o copiar nada.** Lo más aprovechable:

- Tráfico y hora de salida con TomTom: `src/services/rutas/`, `src/data/salidas.ts` y la
  función de Supabase `supabase/functions/rutas`.
- WhatsApp con el mensaje escrito: `enlaceWhatsapp` en `src/services/rutas/textos.ts` y el
  recordatorio a clientes en `src/services/clientes/` y `src/screens/clientes/`.
- Avisos locales: `src/services/avisos/`.
- Traer Google Calendar / iCloud por iCal: `src/services/calendarios/`.
- Captura con IA y límites de IA: `src/services/captura/`, `src/services/ia/` y sus
  funciones de Supabase (`captura`, `ia`, `_shared/limiteIA.ts`).
- Alarma inteligente: `src/services/alarmas/`.
- Guardado en el móvil (SQLite) y en la web (AsyncStorage): `src/data/eventos/`, `db.ts`.
- Componentes y tema: `src/components/`, `src/theme/`.
- Publicar la web en GitHub Pages y en Expo Go sin ordenador: `.github/workflows/`.

## Tecnología (propuesta; la confirma la sesión 02)

- Expo (la versión que use Organizy, SDK 57) con TypeScript y Expo Router.
- Web en GitHub Pages y app probada en Expo Go, como Organizy.
- Datos del usuario en su dispositivo. Servidor solo para lo que necesita claves secretas
  (tráfico con TomTom, IA con Claude): un proyecto **nuevo** de Supabase.
- **Nada compartido con Organizy** (ver "Decisiones del usuario", 01/10): repositorio,
  proyecto de Expo, Supabase y claves de TomTom y Anthropic, todos nuevos y solo de Raxu.

## Carpeta del usuario

`C:\Users\usuario\OneDrive\PERSONAL\Raxu` (índice en su `LEEME.md`): Prompts, Guías de
prueba, Diseño, Capturas, QR y enlaces, Notas y decisiones, Ventas y marketing. Lo que no
sea código y sea para el usuario va a su subcarpeta, nunca suelto en la raíz. El código va
solo aquí.

## Decisiones del usuario

- 01/10/2026 — Nuevo proyecto para autónomos en ruta, con el nombre de trabajo **Raxu**.
  Organizy sigue como está (no se poda ni se borra nada de Organizy sin que él lo diga).
- 01/10/2026 — **Raxu empieza desde cero: no se vincula a nada de Organizy.** Mismas cuentas
  personales (GitHub `Mangel-Creator`, Expo `mangel_creator`, consola de Anthropic, TomTom,
  Supabase), pero **todo lo de dentro es nuevo y solo de Raxu**: repositorio, proyecto de
  Expo, proyecto de Supabase y claves de TomTom y de Anthropic. No reutilices ninguna clave,
  proyecto ni secreto de Organizy; de Organizy solo se copia código (ver `ORGANIZY.md`).
- 01/10/2026 — **Cada herramienta tiene su propia sesión** (sin número: "GitHub", "Expo",
  "Supabase", "TomTom", "Anthropic", "Higgsfield"; registro en `.claude/app-sesiones.md`).
  Montar la cuenta, el proyecto, las claves y los secretos de una herramienta lo hace **su**
  sesión; las demás sesiones solo usan lo que esa deja montado y, si falta algo, se lo dicen
  al usuario (o a esa sesión) en vez de montarlo ellas. Las herramientas nuevas que vayan
  haciendo falta (Apple, Google Play, cobros, WhatsApp Business…) se abren con `/app`.
- 01/10/2026 — **Word de herramientas siempre al día.** La lista de herramientas, planes,
  precios y costes está en `OneDrive\PERSONAL\Raxu\Organización\herramientas.json`; el Word
  `Herramientas.docx` sale de ahí con `regenerar.ps1` (misma carpeta; nunca se edita el Word
  a mano). **Cualquier sesión que añada, quite o cambie de plan una herramienta** (o vea que
  cambia su precio o su límite) actualiza ese JSON —también la fila de la tabla de
  estimación si afecta— y ejecuta `regenerar.ps1`. Si no puede, avisa a la sesión
  "Organización" (SendMessage) con el nombre, plan, precio, límites y enlace al panel. Sin
  contraseñas ni claves en el JSON.

## Estado

Nada programado todavía. Primer paso: sesión 01, resumen de la app. Después, la 02 monta
la base del proyecto.
