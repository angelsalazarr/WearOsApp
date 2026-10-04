# ⌚ WearOSApp — Smartwatch con módulo de Empleados

Aplicación para **Wear OS** desarrollada con **Kotlin** y **Jetpack Compose for Wear OS**. Simula un reloj inteligente con pantalla de bloqueo, cajón de aplicaciones y varias mini-apps, e incluye un módulo de **control de asistencia de empleados** (Gas Uribe de Puebla) conectado a **Cloud Firestore**.

> Proyecto académico — Universidad Tecnológica de Puebla, Desarrollo de Software Multiplataforma.

---

## ✨ Funcionalidades

| Módulo | Descripción |
|---|---|
| 🔒 Pantalla de bloqueo | Hora y fecha en tiempo real sobre imagen de fondo |
| 🏠 Inicio | Accesos rápidos y entrada al cajón de apps |
| 🧮 Calculadora | Operaciones básicas con teclado optimizado para pantalla circular |
| 📷 Cámara | Interfaz de cámara con flash y contador de fotos |
| ▶️ YouTube / 🎓 Classroom | Listas de contenido y clases |
| 🎵 Música | Reproductor con `MediaPlayer`, carátula giratoria, progreso y controles |
| ❤️ Salud | Ritmo cardiaco, pasos (sensor o acelerómetro), calorías y nivel de estrés |
| 📞 Teléfono / 🛒 Play Store / ⚙️ Ajustes | Atajos a las apps del sistema |
| 🪪 **Empleados** | Login por número de empleado, registro de **entrada/salida**, historial de asistencias y perfil (Firestore) |

## 🛠️ Stack

- **Lenguaje:** Kotlin 2.2
- **UI:** Jetpack Compose, Wear Compose (Material / Material3), Navigation
- **Backend:** Firebase Cloud Firestore (BoM 33.5.1)
- **Build:** Gradle 9.4.1, Android Gradle Plugin 9.2.1
- **SDK:** `minSdk 26` · `targetSdk / compileSdk 37` · JDK 11
- **Sensores:** frecuencia cardiaca, contador de pasos y acelerómetro

## 📁 Estructura

```
WearOSApp/
├── app/
│   ├── google-services.json.example   # plantilla de configuración de Firebase
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/example/wearosapp/presentation/
│       │   ├── MainActivity.kt        # navegación y mini-apps del reloj
│       │   ├── EmpleadosApp.kt        # módulo de empleados + FirebaseRepo
│       │   └── theme/Theme.kt
│       └── res/                       # íconos, fondo, audio y strings
├── gradle/libs.versions.toml          # catálogo de versiones
├── FIREBASE_SETUP.md                  # guía detallada de Firebase
└── README.md
```

## 🚀 Cómo ejecutarlo

### Requisitos
- [Android Studio](https://developer.android.com/studio) (versión reciente con soporte para AGP 9.x)
- Un emulador de **Wear OS** o un reloj físico
- Una cuenta de Google para crear un proyecto de Firebase

### Pasos

1. **Clona el repositorio**
   ```bash
   git clone https://github.com/<tu-usuario>/WearOSApp.git
   cd WearOSApp
   ```

2. **Configura Firebase** *(el proyecto no compila sin este paso)*
   - Crea un proyecto en la [consola de Firebase](https://console.firebase.google.com/).
   - Registra una app Android con el paquete `com.example.wearosapp`.
   - Descarga tu `google-services.json` y colócalo en `app/google-services.json`.
   - Activa **Cloud Firestore** y crea la colección `empleados` (ver abajo).

   Guía completa: [FIREBASE_SETUP.md](FIREBASE_SETUP.md)

3. **Abre el proyecto en Android Studio**, espera la sincronización de Gradle y ejecuta en el emulador de Wear OS.

4. En el cajón de apps entra a **Empleados** e inicia sesión con un número de empleado dado de alta.

## 🗄️ Modelo de datos (Firestore)

**`empleados/{numEmpleado}`**

| Campo | Tipo | Ejemplo |
|---|---|---|
| `nombre` | string | `"Juan Pérez López"` |
| `puesto` | string | `"Técnico de instalación"` |
| `activo` | boolean | `true` |

**`asistencias/{idAutomático}`** *(la app lo genera al registrar entrada/salida)*

`numEmpleado`, `nombre`, `tipo` (`"entrada"` / `"salida"`), `fechaHora`, `fechaTexto`, `horaTexto`

> Al consultar el historial por primera vez, Firestore pedirá un **índice compuesto** (`numEmpleado` + `fechaHora`). El error de Logcat incluye un enlace para crearlo con un clic.

## 🔐 Seguridad

- `google-services.json` y `local.properties` **no se suben** al repositorio (están en `.gitignore`).
- Las reglas de Firestore de ejemplo son abiertas para fines escolares; ajústalas antes de usar la app en producción.

## 🎵 Nota sobre los recursos

Los archivos de `res/raw/` son pistas de prueba para el reproductor. Si publicas el proyecto, reemplázalos por audio propio o con licencia libre.

## 👤 Autor

**Angel Ramos Salazar**
Universidad Tecnológica de Puebla — Desarrollo de Software Multiplataforma
