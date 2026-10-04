# Conexión a Firebase — App de Empleados (Gas Uribe de Puebla)

El código ya está listo (Gradle + Kotlin) para usar **Cloud Firestore**.
Solo falta que conectes TU proyecto de Firebase. Sigue estos pasos en orden.

## 1. Crear el proyecto en Firebase

1. Entra a https://console.firebase.google.com/
2. "Agregar proyecto" → nómbralo, por ejemplo, `gas-uribe-puebla`.
3. Puedes desactivar Google Analytics (no se necesita para este proyecto).

## 2. Registrar la app de Android

1. Dentro del proyecto, clic en el ícono de Android para agregar una app.
2. **Nombre del paquete (applicationId)**: `com.example.wearosapp`
   (es el mismo que ya está en `app/build.gradle.kts`; si tú lo cambiaste,
   usa el que tengas ahí).
3. Descarga el archivo **`google-services.json`** que te da Firebase.
4. Copia ese archivo dentro de la carpeta `app/` del proyecto, es decir:
   `WearOSApp/app/google-services.json`
   (al mismo nivel que `app/build.gradle.kts`).

   ⚠️ Sin este archivo el proyecto **no compilará** — es lo único que falta
   para que la conexión funcione, todo lo demás (plugin, dependencias,
   permisos) ya está configurado en el código que te entrego.

## 3. Activar Cloud Firestore

1. En el menú lateral de Firebase → **Build → Firestore Database**.
2. "Crear base de datos" → elige **modo de prueba** (test mode) para
   empezar rápido, o define reglas propias (ver punto 5).
3. Elige la ubicación del servidor (por ejemplo `us-central` o la más
   cercana a Puebla que te ofrezca).

## 4. Crear las colecciones y datos de ejemplo

La app espera dos colecciones:

### Colección `empleados`
El **ID del documento** debe ser el número de empleado (el mismo que se
teclea al iniciar sesión en el reloj). Ejemplo, documento con ID `1024`:

| Campo    | Tipo    | Ejemplo                 |
|----------|---------|--------------------------|
| nombre   | string  | "Juan Pérez López"      |
| puesto   | string  | "Técnico de instalación"|
| activo   | boolean | true                     |

Crea 2 o 3 empleados de prueba así para poder iniciar sesión en el reloj.

### Colección `asistencias`
La app la llena sola cada vez que un empleado registra entrada/salida
(no necesitas crear nada aquí manualmente). Cada documento se genera con
ID automático y contiene: `numEmpleado`, `nombre`, `tipo` ("entrada" o
"salida"), `fechaHora` (timestamp), `fechaTexto`, `horaTexto`.

Firestore te pedirá crear un **índice compuesto** la primera vez que se
consulte el historial (porque se filtra por `numEmpleado` y se ordena por
`fechaHora`). Cuando eso pase, verás un error en Logcat con un enlace
directo tipo `https://console.firebase.google.com/.../indexes?create...` —
solo ábrelo y confirma, Firebase lo crea automáticamente en un par de
minutos.

## 5. Reglas de seguridad (recomendado para no dejarlo abierto)

En modo de prueba, cualquiera podría leer/escribir tu base de datos.
Para un proyecto escolar puedes dejarlo así mientras haces pruebas, pero
antes de entregar/publicar te recomiendo reglas más cerradas, por ejemplo:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /empleados/{empleadoId} {
      allow read: if true;
      allow write: if false; // los empleados se dan de alta desde la consola
    }
    match /asistencias/{registroId} {
      allow read, write: if true; // simplificado para el proyecto escolar
    }
  }
}
```

(Estas reglas son intencionalmente simples porque la app no usa
Firebase Authentication; el "login" es solo consultar el número de
empleado en la colección `empleados`.)

## 6. Compilar

1. Abre el proyecto en Android Studio (Open → carpeta `WearOSApp`).
2. Deja que Gradle sincronice (con el `google-services.json` ya copiado).
3. Ejecuta en un emulador de Wear OS.
4. En el cajón de apps del reloj, entra a **"Empleados"**, teclea el
   número de empleado que diste de alta y ya puedes registrar
   entrada/salida.

## Resumen de lo que ya viene configurado en el código

- Plugin `com.google.gms.google-services` agregado en `build.gradle.kts`
  (raíz) y aplicado en `app/build.gradle.kts`.
- Dependencias `firebase-bom` + `firebase-firestore-ktx` agregadas.
- Permisos `INTERNET` y `ACCESS_NETWORK_STATE` en el `AndroidManifest.xml`.
- Toda la lógica de lectura/escritura en `FirebaseRepo`
  (`EmpleadosApp.kt`), lista para usarse en cuanto exista
  `google-services.json`.
