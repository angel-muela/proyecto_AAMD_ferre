# Prompt:

### Contexto: Actúa como un Arquitecto de Software Senior. Mi proyecto es una aplicación multiplataforma (Android, Web, Windows, iOS) de una Ferretería desarrollada en Flutter/Dart, usando Antigravity como entorno.

### Estado Actual: Ya cuento con un proyecto en Firebase llamado BDcrudferre0290. En Firestore, ya existe una colección principal denominada EMPLEADOS con documentos que contienen los campos: nombre (string), puesto (string), salario (number), numero de telefono (string) y fecha de nacimiento (timestamp).

# Restricciones de la Tarea:

   1. Backend: Firebase Authentication (Login/Password) y Cloud Firestore.

   2. Gestión de Estado: Provider.

   3. Estándares: No utilizar analíticas, no usar la opción de producción "A" (mantener estándar limpio).

   4. Herramientas: Integración con Antigravity / VS Code.

# Tarea: Genera un Plan de Implantación detallado en formato Markdown organizado por tablas que cubra:

   1. Fase de Configuración: Herramientas necesarias para el entorno Antigravity y conexión con el proyecto BDcrudferre0290.

   2. Modelado de Datos: Define la clase Empleado en Dart basándote en los campos que ya tengo en Firestore (incluyendo conversión de Timestamps).

   3. Estructura de UI/UX: Planificación de pantallas para el CRUD de empleados y navegación de la ferretería.

   4. Arquitectura de Carpetas: Estructura profesional (models, services, providers, views).

   5. Dependencias: Listado preciso para pubspec.yaml (firebase_core, cloud_firestore, firebase_auth, provider).

   6. Plan de Pruebas: Verificación del flujo de datos entre Flutter y Firestore.

### Importante: No generes código fuente completo todavía. Solo entrega el plan estratégico paso a paso en tablas de Markdown como se solicita para mi Actividad 3 de la UIII.

# respuesta IA:

¡Entendido! Como Arquitecto Senior, he diseñado este Plan de Implantación estratégico para tu proyecto de la ferretería. Al trabajar con **Antigravity** (que optimiza el flujo de desarrollo en Flutter), es crucial mantener una separación clara entre la lógica de negocio y la interfaz de usuario.

A continuación, presento la hoja de ruta técnica para tu Actividad 3 de la UIII.

---

## 🛠️ Fase 1: Configuración del Entorno y Conexión

Esta fase asegura que el entorno **Antigravity** en VS Code esté sincronizado con tu proyecto existente en Firebase.

| Paso | Acción | Descripción |
| --- | --- | --- |
| **1.1** | **Sync Antigravity** | Vincular el espacio de trabajo de VS Code con el CLI de Antigravity para pre-configuración de plantillas. |
| **1.2** | **Firebase Login** | Ejecutar `firebase login` en la terminal para autenticar la sesión con la cuenta de `BDcrudferre0290`. |
| **1.3** | **FlutterFire Config** | Ejecutar `flutterfire configure` seleccionando el ID `BDcrudferre0290` para generar el archivo `firebase_options.dart`. |
| **1.4** | **Init Core** | Inicializar `Firebase.initializeApp` en el `main.dart` usando las opciones generadas. |

---

## 📊 Fase 2: Modelado de Datos (Dart Class)

Definición de la entidad `Empleado`. Es vital el manejo de `Timestamp` para evitar errores de tipo en Flutter.

| Atributo | Tipo de Dato Dart | Mapeo Firestore | Notas de Conversión |
| --- | --- | --- | --- |
| `id` | `String` | `DocumentID` | Se extrae del `doc.id` de Firestore. |
| `nombre` | `String` | `nombre` | Valor simple. |
| `puesto` | `String` | `puesto` | Valor simple. |
| `salario` | `double` / `int` | `salario` | Manejar como `num` para evitar conflictos de precisión. |
| `telefono` | `String` | `numero de telefono` | Formato String para admitir prefijos. |
| `fechaNac` | `DateTime` | `fecha de nacimiento` | Convertir usando `.toDate()` desde el `Timestamp`. |

---

## 📁 Fase 3: Arquitectura de Carpetas Proporcional

Estructura profesional orientada a la mantenibilidad y escalabilidad del proyecto.

| Directorio | Responsabilidad | Contenido Sugerido |
| --- | --- | --- |
| `lib/models/` | Definición de objetos | `empleado_model.dart` |
| `lib/services/` | Comunicación con Firebase | `auth_service.dart`, `firestore_service.dart` |
| `lib/providers/` | Gestión de estado (Provider) | `empleado_provider.dart`, `auth_provider.dart` |
| `lib/views/` | Pantallas y componentes UI | `login_screen.dart`, `home_screen.dart`, `empleado_form.dart` |
| `lib/utils/` | Helpers y constantes | `constants.dart`, `validators.dart` |

---

## 📱 Fase 4: Estructura de UI/UX y Navegación

Diseño de flujo para una aplicación de gestión administrativa de ferretería.

| Pantalla | Función Principal | Componentes Clave |
| --- | --- | --- |
| **Login** | Acceso seguro | Inputs de correo/pass, validación de campos. |
| **Dashboard** | Menú principal | Botones de acceso a CRUD Empleados, Inventario (futuro). |
| **Lista Empleados** | Visualización (R) | `StreamBuilder` o `Consumer` de Provider, `ListView`. |
| **Formulario** | Alta/Edición (C/U) | Pickers de fecha para nacimiento, teclado numérico para salario. |
| **Detalle** | Baja/Gestión (D) | Modal de confirmación para borrado, visualización extendida. |

---

## 📦 Fase 5: Dependencias (pubspec.yaml)

Listado estricto de librerías necesarias para cumplir con los requerimientos técnicos.

| Paquete | Versión Sugerida | Propósito |
| --- | --- | --- |
| `firebase_core` | `^2.x.x` | Núcleo de conexión con Firebase. |
| `cloud_firestore` | `^4.x.x` | Persistencia y consulta de la colección EMPLEADOS. |
| `firebase_auth` | `^4.x.x` | Gestión de sesiones de usuario. |
| `provider` | `^6.x.x` | Inyección de dependencias y estado reactivo. |
| `intl` | `^0.18.x` | Formateo de fechas y moneda para el salario. |

---

## 🧪 Fase 6: Plan de Pruebas (Data Flow)

Verificación de la integridad del flujo de datos entre la App y `BDcrudferre0290`.

| Caso de Prueba | Resultado Esperado | Herramienta |
| --- | --- | --- |
| **Auth Connection** | El usuario puede loguearse y el UID aparece en Firebase Auth. | Firebase Console |
| **Fetch Read** | Los datos de la colección EMPLEADOS se listan en el móvil. | Debug Console / UI |
| **Type Integrity** | La fecha de nacimiento se muestra como `DD/MM/AAAA` (no como String). | UI (intl package) |
| **Persistence** | Al editar un salario, el cambio se refleja en tiempo real en Firestore. | Firestore Viewer |

---

> **Nota del Arquitecto:** Este plan cumple con los estándares de limpieza solicitados. Una vez aprobado este diseño de arquitectura, el siguiente paso será la implementación del `FirestoreService` para mapear los documentos de la colección EMPLEADOS a nuestra clase de modelo.

¿Deseas que profundicemos en la lógica del **Provider** para la gestión de los empleados antes de pasar al código?
