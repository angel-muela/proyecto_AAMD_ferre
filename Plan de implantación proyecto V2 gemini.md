# Prompt:

Actúa como un creador de software. Quiero crear una aplicación multiplataforma en flutter dart (y firebase utilizando vs code o Antigravity, que herramientas se requieren para UI, UX, dependencias, login autenticación usuario password, base de datos firestore, privider, dependencias en pubspec.yaml), antes de que proporciones código, quiero crear un plan de implementación en formato markdown, para desarrollar la aplicación "ferreteria el patito", no código, procedimiento paso a paso para el desarrollo

# respuesta IA:

# 🔩 Plan de Implementación — Ferretería El Patito
### Aplicación Multiplataforma con Flutter + Firebase

---

## 📋 Índice

1. [Visión General del Proyecto](#1-visión-general-del-proyecto)
2. [Herramientas y Entorno de Desarrollo](#2-herramientas-y-entorno-de-desarrollo)
3. [Arquitectura de la Aplicación](#3-arquitectura-de-la-aplicación)
4. [Configuración del Proyecto Flutter](#4-configuración-del-proyecto-flutter)
5. [Dependencias — pubspec.yaml](#5-dependencias--pubspecyaml)
6. [Configuración de Firebase](#6-configuración-de-firebase)
7. [Módulo de Autenticación](#7-módulo-de-autenticación)
8. [Estructura de Base de Datos Firestore](#8-estructura-de-base-de-datos-firestore)
9. [Módulos de la Aplicación](#9-módulos-de-la-aplicación)
10. [UI/UX — Diseño e Identidad Visual](#10-uiux--diseño-e-identidad-visual)
11. [Gestión de Estado con Provider](#11-gestión-de-estado-con-provider)
12. [Fases de Desarrollo](#12-fases-de-desarrollo)
13. [Buenas Prácticas y Seguridad](#13-buenas-prácticas-y-seguridad)

---

## 1. Visión General del Proyecto

**Nombre:** Ferretería El Patito
**Tipo:** Aplicación multiplataforma (Android, iOS, Web)
**Stack:** Flutter + Dart + Firebase

### Funcionalidades Principales

| Módulo | Descripción |
|---|---|
| Autenticación | Login, registro, recuperación de contraseña |
| Inventario | Catálogo de productos por categoría |
| Ventas / POS | Punto de venta, carrito, facturación |
| Clientes | Registro y historial de clientes |
| Reportes | Estadísticas de ventas e inventario |
| Roles de Usuario | Admin, vendedor, almacenista |

---

## 2. Herramientas y Entorno de Desarrollo

### 2.1 Software Requerido (instalar en orden)

```
1. Flutter SDK         https://docs.flutter.dev/get-started/install
2. Dart SDK            (incluido con Flutter)
3. Visual Studio Code  https://code.visualstudio.com/
4. Android Studio      (para emulador Android + Android SDK)
5. Xcode               (solo macOS — para compilar en iOS)
6. Google Chrome       (para desarrollo web)
7. Git                 https://git-scm.com/
8. Node.js             https://nodejs.org/ (requerido por Firebase CLI)
9. Firebase CLI        npm install -g firebase-tools
10. FlutterFire CLI    dart pub global activate flutterfire_cli
```

### 2.2 Extensiones de VS Code (esenciales)

| Extensión | Propósito |
|---|---|
| `Flutter` (Dart Code) | Soporte oficial Flutter/Dart |
| `Dart` (Dart Code) | Análisis de código Dart |
| `Firebase Explorer` | Ver Firestore desde VS Code |
| `Pubspec Assist` | Agregar dependencias fácilmente |
| `Error Lens` | Resaltar errores en línea |
| `Thunder Client` | Probar endpoints REST |
| `GitLens` | Control de versiones avanzado |
| `Material Icon Theme` | Íconos para carpetas y archivos |
| `Bracket Pair Colorizer` | Legibilidad de código |

### 2.3 Verificación del Entorno

```bash
# Ejecutar en terminal para verificar instalación
flutter doctor -v
```
> ✅ Todos los ítems deben aparecer en verde antes de continuar.

---

## 3. Arquitectura de la Aplicación

Se usará **Clean Architecture** adaptada a Flutter:

```
lib/
├── core/                    # Utilidades globales
│   ├── constants/           # Colores, rutas, strings
│   ├── errors/              # Manejo de excepciones
│   ├── theme/               # ThemeData, estilos
│   └── utils/               # Helpers, formatters
│
├── data/                    # Capa de datos
│   ├── models/              # Modelos de datos (fromJson/toJson)
│   ├── repositories/        # Implementación de repositorios
│   └── datasources/         # Firebase, local storage
│
├── domain/                  # Lógica de negocio
│   ├── entities/            # Entidades puras
│   ├── repositories/        # Contratos (interfaces)
│   └── usecases/            # Casos de uso
│
├── presentation/            # Capa de UI
│   ├── providers/           # Estado con Provider
│   ├── screens/             # Pantallas principales
│   └── widgets/             # Widgets reutilizables
│
├── firebase_options.dart    # Generado por FlutterFire CLI
└── main.dart                # Punto de entrada
```

### Patrón de Estado
- **Provider** como gestor de estado principal
- **Repository Pattern** para abstracción de datos
- **Dependency Injection** manual vía `MultiProvider`

---

## 4. Configuración del Proyecto Flutter

### Paso 1 — Crear el proyecto

```bash
flutter create ferreteria_el_patito --org com.ferreteria.patito
cd ferreteria_el_patito
```

### Paso 2 — Crear la estructura de carpetas

```bash
mkdir -p lib/core/{constants,errors,theme,utils}
mkdir -p lib/data/{models,repositories,datasources}
mkdir -p lib/domain/{entities,repositories,usecases}
mkdir -p lib/presentation/{providers,screens,widgets}
```

### Paso 3 — Habilitar plataformas objetivo

```bash
flutter config --enable-web
flutter config --enable-android
flutter config --enable-ios   # Solo en macOS
```

---

## 5. Dependencias — pubspec.yaml

### 5.1 Firebase (núcleo)

```yaml
firebase_core: ^3.x.x          # Inicialización de Firebase
firebase_auth: ^5.x.x          # Autenticación (email/password, Google)
cloud_firestore: ^5.x.x        # Base de datos NoSQL en tiempo real
firebase_storage: ^12.x.x      # Almacenamiento de imágenes/archivos
firebase_analytics: ^11.x.x    # Analíticas de uso
```

### 5.2 Estado y Navegación

```yaml
provider: ^6.x.x               # Gestión de estado reactivo
go_router: ^14.x.x             # Enrutamiento declarativo con deep links
```

### 5.3 UI / Componentes Visuales

```yaml
flutter_svg: ^2.x.x            # Íconos y gráficos SVG
cached_network_image: ^3.x.x   # Caché de imágenes de red
shimmer: ^3.x.x                # Efecto de carga skeleton
lottie: ^3.x.x                 # Animaciones Lottie
fl_chart: ^0.x.x               # Gráficas de reportes
data_table_2: ^2.x.x           # Tablas avanzadas para inventario
badges: ^3.x.x                 # Notificaciones y contadores
flutter_staggered_grid_view: ^0.x.x  # Grid para catálogo
```

### 5.4 Formularios y Validación

```yaml
flutter_form_builder: ^9.x.x   # Formularios declarativos
form_builder_validators: ^11.x.x # Reglas de validación
```

### 5.5 Almacenamiento Local

```yaml
shared_preferences: ^2.x.x     # Preferencias simples del usuario
hive: ^2.x.x                   # BD local rápida (caché offline)
hive_flutter: ^1.x.x           # Integración Hive + Flutter
```

### 5.6 Utilidades

```yaml
intl: ^0.19.x                  # Formateo de fechas y moneda (MXN)
uuid: ^4.x.x                   # Generación de IDs únicos
logger: ^2.x.x                 # Logging estructurado
connectivity_plus: ^6.x.x      # Detectar estado de red
image_picker: ^1.x.x           # Captura/selección de imágenes
pdf: ^3.x.x                    # Generación de facturas PDF
printing: ^5.x.x               # Imprimir/compartir PDF
barcode_scanner: ^5.x.x        # Escaneo de código de barras
equatable: ^2.x.x              # Comparación de objetos
```

### 5.7 Dev Dependencies

```yaml
dev_dependencies:
  flutter_test:
    sdk: flutter
  build_runner: ^2.x.x         # Generación de código
  hive_generator: ^2.x.x       # Adaptadores Hive
  mockito: ^5.x.x              # Mocks para testing
  flutter_launcher_icons: ^0.x.x  # Ícono de la app
  flutter_native_splash: ^2.x.x   # Pantalla de inicio
```

> ⚠️ **Nota:** Reemplazar `x.x` con la versión más reciente estable al momento de iniciar el proyecto consultando [pub.dev](https://pub.dev)

---

## 6. Configuración de Firebase

### Paso 1 — Crear proyecto en Firebase Console

1. Ir a [console.firebase.google.com](https://console.firebase.google.com)
2. Crear nuevo proyecto: `ferreteria-el-patito`
3. Desactivar Google Analytics (opcional para inicio)

### Paso 2 — Habilitar servicios en la consola

```
Firebase Console > Authentication > Sign-in method:
  ✅ Email/Password
  ✅ Google Sign-In (opcional)

Firebase Console > Firestore Database:
  ✅ Crear base de datos en modo producción
  ✅ Seleccionar región: us-central1

Firebase Console > Storage:
  ✅ Habilitar Firebase Storage
```

### Paso 3 — Conectar Flutter con Firebase (FlutterFire CLI)

```bash
# Autenticarse en Firebase
firebase login

# Configurar automáticamente todas las plataformas
flutterfire configure --project=ferreteria-el-patito
```
> Esto genera `firebase_options.dart` automáticamente.

### Paso 4 — Reglas de seguridad Firestore

```javascript
// Reglas iniciales — personalizar por rol
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Solo usuarios autenticados acceden
    match /{document=**} {
      allow read, write: if request.auth != null;
    }

    // Solo admins escriben en productos
    match /productos/{productoId} {
      allow write: if get(/databases/$(database)/documents/usuarios/$(request.auth.uid)).data.rol == 'admin';
    }
  }
}
```

---

## 7. Módulo de Autenticación

### Flujo de Autenticación

```
Splash Screen
    │
    ├──► Usuario autenticado ──► Home Screen (según rol)
    │
    └──► Sin sesión ──► Login Screen
                            │
                            ├──► Registro ──► Verificación Email
                            │
                            └──► Recuperar contraseña ──► Email enviado
```

### Pantallas del módulo Auth

| Pantalla | Descripción |
|---|---|
| `SplashScreen` | Logo animado + verificación de sesión |
| `LoginScreen` | Email, contraseña, botón login, link a registro |
| `RegisterScreen` | Nombre, email, contraseña, confirmar contraseña |
| `ForgotPasswordScreen` | Ingresa email → Firebase envía link de reset |

### Roles de usuario

| Rol | Permisos |
|---|---|
| `admin` | Acceso total: ventas, inventario, usuarios, reportes |
| `vendedor` | Crear ventas, consultar productos, ver clientes |
| `almacenista` | Gestionar inventario, entradas/salidas de stock |

---

## 8. Estructura de Base de Datos Firestore

### Colecciones Principales

```
firestore/
│
├── usuarios/
│   └── {uid}/
│       ├── nombre: String
│       ├── email: String
│       ├── rol: String          // admin | vendedor | almacenista
│       ├── activo: Boolean
│       └── createdAt: Timestamp
│
├── productos/
│   └── {productoId}/
│       ├── nombre: String
│       ├── descripcion: String
│       ├── categoria: String    // tornilleria | herramienta | plomeria | etc.
│       ├── precio: Number
│       ├── costo: Number
│       ├── stock: Number
│       ├── stockMinimo: Number
│       ├── codigoBarras: String
│       ├── imagen: String       // URL de Firebase Storage
│       └── activo: Boolean
│
├── categorias/
│   └── {categoriaId}/
│       ├── nombre: String
│       └── icono: String
│
├── clientes/
│   └── {clienteId}/
│       ├── nombre: String
│       ├── telefono: String
│       ├── email: String
│       ├── rfc: String
│       └── direccion: String
│
├── ventas/
│   └── {ventaId}/
│       ├── folio: String
│       ├── clienteId: String    // referencia
│       ├── vendedorId: String   // uid del vendedor
│       ├── fecha: Timestamp
│       ├── total: Number
│       ├── descuento: Number
│       ├── metodoPago: String   // efectivo | tarjeta | transferencia
│       ├── estado: String       // completada | cancelada | pendiente
│       └── productos: Array
│               └── { productoId, nombre, cantidad, precioUnitario, subtotal }
│
└── configuracion/
    └── general/
        ├── nombreNegocio: String
        ├── rfc: String
        ├── direccion: String
        └── telefono: String
```

---

## 9. Módulos de la Aplicación

### 9.1 Dashboard / Home
- Resumen de ventas del día
- Gráfica de ingresos semanal
- Alertas de stock bajo
- Accesos directos por rol

### 9.2 Inventario
- Listado de productos con buscador
- Filtros por categoría
- Agregar / editar / desactivar productos
- Subir imagen desde cámara o galería
- Escanear código de barras
- Control de stock (entradas y salidas)

### 9.3 Punto de Venta (POS)
- Buscador de productos por nombre o código
- Escaneo de código de barras
- Carrito de compra en tiempo real
- Aplicar descuento global o por ítem
- Seleccionar cliente
- Elegir método de pago
- Generar e imprimir ticket PDF

### 9.4 Clientes
- Listado y búsqueda de clientes
- Ficha de cliente con historial de compras
- CRUD completo

### 9.5 Reportes
- Ventas por día / semana / mes
- Productos más vendidos (gráfica)
- Ganancias vs costos
- Inventario bajo stock mínimo
- Exportar a PDF

### 9.6 Usuarios y Configuración
- Gestión de usuarios del sistema (solo admin)
- Cambiar contraseña
- Configuración del negocio
- Cierre de sesión

---

## 10. UI/UX — Diseño e Identidad Visual

### 10.1 Identidad Visual

| Elemento | Valor Sugerido |
|---|---|
| Color primario | Amarillo ferretería `#F5C518` |
| Color secundario | Azul oscuro `#1A237E` |
| Color de acento | Naranja `#F57C00` |
| Color de fondo | Gris claro `#F5F5F5` |
| Fuente principal | `Poppins` (Google Fonts) |
| Fuente secundaria | `Roboto Mono` (para precios/códigos) |

### 10.2 Principios UX

- **Velocidad:** El POS debe funcionar en menos de 3 taps para agregar producto
- **Offline-first:** Hive como caché local para funcionar sin internet
- **Responsive:** Adaptar layout a tablet (split-view) y móvil (stack)
- **Feedback visual:** Shimmer en carga, toasts de confirmación, diálogos de error claros
- **Accesibilidad:** Contraste mínimo WCAG AA, texto escalable

### 10.3 Navegación

```
BottomNavigationBar (móvil) / NavigationRail (tablet)
  ├── 🏠 Dashboard
  ├── 📦 Inventario
  ├── 🛒 Punto de Venta
  ├── 👥 Clientes
  └── 📊 Reportes
```

---

## 11. Gestión de Estado con Provider

### Providers necesarios

```dart
MultiProvider(
  providers: [
    ChangeNotifierProvider(create: (_) => AuthProvider()),
    ChangeNotifierProvider(create: (_) => ProductoProvider()),
    ChangeNotifierProvider(create: (_) => VentaProvider()),
    ChangeNotifierProvider(create: (_) => ClienteProvider()),
    ChangeNotifierProvider(create: (_) => CarritoProvider()),
    ChangeNotifierProvider(create: (_) => ReporteProvider()),
  ],
)
```

### Responsabilidades por Provider

| Provider | Responsabilidad |
|---|---|
| `AuthProvider` | Usuario actual, login, logout, rol |
| `ProductoProvider` | Lista de productos, búsqueda, CRUD |
| `VentaProvider` | Historial de ventas, nueva venta |
| `ClienteProvider` | Lista de clientes, CRUD |
| `CarritoProvider` | Ítems del carrito, total, descuentos |
| `ReporteProvider` | Cálculos y agregaciones para reportes |

---

## 12. Fases de Desarrollo

### 🔵 Fase 1 — Fundamentos (Semana 1-2)
- [ ] Configurar Flutter, VS Code y extensiones
- [ ] Crear proyecto y estructura de carpetas
- [ ] Instalar y configurar Firebase (Auth + Firestore)
- [ ] Implementar pantallas de autenticación (login, registro, recuperar)
- [ ] Configurar Provider y rutas con GoRouter
- [ ] Implementar control de roles

### 🟡 Fase 2 — Inventario (Semana 3-4)
- [ ] Modelo y repositorio de Producto
- [ ] Listado de productos con búsqueda y filtros
- [ ] CRUD de productos + subida de imágenes
- [ ] Escaneo de código de barras
- [ ] Sistema de categorías

### 🟠 Fase 3 — Punto de Venta (Semana 5-6)
- [ ] Modelo y repositorio de Venta
- [ ] Pantalla de POS con carrito
- [ ] Selección de cliente
- [ ] Aplicar descuentos
- [ ] Generación de ticket en PDF
- [ ] Actualización automática de stock al vender

### 🟢 Fase 4 — Clientes y Reportes (Semana 7-8)
- [ ] CRUD de clientes
- [ ] Historial de compras por cliente
- [ ] Dashboard con métricas
- [ ] Pantalla de reportes con gráficas
- [ ] Exportar reportes a PDF

### 🔴 Fase 5 — Pulido y Despliegue (Semana 9-10)
- [ ] Soporte offline con Hive
- [ ] Pantalla de splash y onboarding
- [ ] Pruebas unitarias y de widget
- [ ] Configurar ícono y splash nativo
- [ ] Build y publicación en Play Store / App Store / Web

---

## 13. Buenas Prácticas y Seguridad

### Código
- Usar `const` constructores donde sea posible
- Separar lógica de negocio de la UI (no llamar Firestore desde un Widget)
- Manejar siempre los estados: `loading`, `success`, `error`
- Validar formularios en cliente Y en reglas de Firestore

### Seguridad
- Nunca guardar claves API en el código fuente — usar variables de entorno
- Configurar reglas de Firestore según rol antes de publicar
- Habilitar verificación de email antes de dar acceso completo
- Rotar tokens y usar Firebase App Check para proteger APIs

### Control de versiones
```bash
# Inicializar repositorio Git
git init
echo ".env" >> .gitignore
echo "*.keystore" >> .gitignore
git add .
git commit -m "feat: initial project setup"
```

### Testing
- Unit tests para cada `usecase` y `repository`
- Widget tests para flujos críticos (login, agregar al carrito)
- Usar Firebase Emulator Suite para pruebas locales sin costo

---

## ✅ Próximo Paso

Con este plan aprobado, el siguiente paso es **Fase 1**: crear el proyecto Flutter, instalar dependencias y configurar Firebase. ¿Deseas continuar con el código de esa fase?

---
*Plan generado para: Ferretería El Patito | Flutter + Firebase | Mayo 2026*
