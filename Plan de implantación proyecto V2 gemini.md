#Prompt

### Contexto: Actúa como un Arquitecto de Software Senior especializado en desarrollo multiplataforma con Flutter.

### Objetivo: Diseñar un Plan de Implantación detallado para el proyecto "Ferretería", una aplicación robusta que debe funcionar en Android, Web, Windows e iOS utilizando el entorno Antigravity.

# Restricciones Técnicas:

   1. Stack: Flutter & Dart.

   2. Backend: Firebase (Firestore para base de datos y Authentication para login con correo/password).

   3. Gestión de Estado: Provider.

   4. IDE: Configuración específica para Antigravity / VS Code.

   5. Estándares: No utilizar opciones de producción "A", no incluir analíticas. El enfoque debe ser un estándar limpio y escalable.

# Tarea: Antes de generar cualquier línea de código, genera un Plan de Implementación en formato Markdown que incluya:

   1. Requerimientos de Entorno: Herramientas necesarias y configuración del SDK para Antigravity.

   2. Arquitectura de la Información (UI/UX): Estructura de vistas (Home, Inventario, Carrito, Perfil).

   3. Configuración de Firebase: Pasos para Firestore y Auth sin analíticas.

   4. Estructura del Proyecto: Organización de carpetas (Models, Views, Controllers/Providers, Services).

   5. Dependencias Críticas: Listado para el pubspec.yaml (provider, cloud_firestore, firebase_auth, etc.).

   6. Cronograma de Desarrollo (Fases): Desde la inicialización hasta el despliegue multiplataforma.

# Formato de entrega: Tablas de Markdown organizadas y listas de verificación (checklists). No proporciones el código fuente todavía, solo el plan estratégico.








#respuesta IA

¡Hola! Como Arquitecto Senior, entiendo perfectamente el desafío. Implementar una solución para el sector de ferretería bajo el entorno **Antigravity** requiere una estructura rígida en la separación de capas para garantizar que el rendimiento sea óptimo tanto en la web como en sistemas de escritorio (Windows) y móviles.

A continuación, presento el **Plan de Implantación Estratégico** para el proyecto "Ferretería".

---

## 1. Requerimientos de Entorno y Configuración

El entorno Antigravity exige una sincronización precisa entre el SDK de Flutter y las variables de entorno del sistema.

| Componente | Versión / Configuración | Notas |
| --- | --- | --- |
| **Flutter SDK** | Stable Channel (3.x o superior) | Base del framework. |
| **Dart SDK** | 3.0.0+ | Habilitando *records* y *pattern matching*. |
| **Entorno Antigravity** | Workspace Prefects / Dockerized | Configuración de contenedores para build aislada. |
| **VS Code Extensions** | Flutter, Dart, Error Lens | Indispensable para debugging rápido. |
| **Sistemas Operativos** | Win 10/11, macOS, Android SDK | Necesarios para compilación nativa. |

### Checklist de Preparación:

* [ ] Configuración de variables de entorno `ANTIGRAVITY_PATH`.
* [ ] Ejecución de `flutter doctor` para validar compatibilidad nativa (Desktop/Web).
* [ ] Habilitación de soporte para Windows: `flutter config --enable-windows-desktop`.

---

## 2. Arquitectura de la Información (UX/UI)

Dado que es una app multiplataforma, la navegación debe ser adaptativa. Utilizaremos un modelo de **Navegación Lateral** para Desktop/Web y **Bottom Navigation** para Mobile.

| Vista | Funcionalidad Core | Elementos Clave |
| --- | --- | --- |
| **Home** | Dashboard de ventas y ofertas. | Buscador global, categorías, destacados. |
| **Inventario** | Gestión de stock y precios. | Tabla filtrable, CRUD de productos. |
| **Carrito** | Resumen de compra. | Cálculo de impuestos, selección de entrega. |
| **Perfil** | Gestión de usuario y ajustes. | Datos fiscales, historial de pedidos. |

---

## 3. Configuración de Firebase (Sin Analíticas)

Para mantener el estándar solicitado de "código limpio", la integración se limitará estrictamente a la persistencia y seguridad.

### Pasos Críticos:

1. **Firebase Console**: Crear proyecto "Ferreteria-App".
2. **Authentication**: Habilitar únicamente "Email/Password". Desactivar otros proveedores.
3. **Cloud Firestore**: Iniciar en *Production Mode*.
* *Reglas de Seguridad*: Bloquear lectura/escritura global, permitir solo a `request.auth != null`.


4. **Exclusión de Analytics**: Al configurar `Firebase.initializeApp()`, no invocar métodos de recolección de datos. En Android/iOS, asegurar que el archivo de configuración no incluya el ID de medición.

---

## 4. Estructura del Proyecto (Clean Architecture Lite)

Organizaremos el código siguiendo una variante de la arquitectura de capas, optimizada para el uso de **Provider**.

```text
lib/
├── core/               # Constantes, temas y utilidades globales.
├── data/
│   ├── models/         # Clases de datos (Product, User, Order).
│   └── services/       # Comunicación directa con Firebase (Wrappers).
├── providers/          # Lógica de estado (InventoryProvider, AuthProvider).
├── ui/
│   ├── views/          # Pantallas completas (HomeView, LoginView).
│   ├── widgets/        # Componentes reutilizables (CustomButton, Card).
│   └── layouts/        # Wrappers para diseño adaptativo (Desktop vs Mobile).
└── main.dart           # Punto de entrada y configuración de MultiProvider.

```

---

## 5. Dependencias Críticas (`pubspec.yaml`)

Estas son las librerías base necesarias para la operatividad sin incluir herramientas de marketing o seguimiento.

| Package | Propósito |
| --- | --- |
| `provider` | Gestión de estado reactiva y eficiente. |
| `firebase_core` | Inicialización del entorno Firebase. |
| `firebase_auth` | Autenticación robusta de usuarios. |
| `cloud_firestore` | Base de datos NoSQL en tiempo real. |
| `google_fonts` | Estandarización tipográfica multiplataforma. |
| `intl` | Formateo de moneda (MXN/USD) y fechas. |

---

## 6. Cronograma de Desarrollo (Fases)

### Fase 1: Cimientos y Auth (Semana 1)

* [ ] Setup del proyecto en entorno Antigravity.
* [ ] Implementación de `AuthService` y `AuthProvider`.
* [ ] Diseño de vistas de Login y Registro.

### Fase 2: Gestión de Datos (Semana 2)

* [ ] Modelado de colecciones en Firestore (Products, Categories).
* [ ] Desarrollo del `InventoryProvider` (CRUD de productos).
* [ ] Implementación de la vista de Inventario con búsqueda.

### Fase 3: Lógica de Negocio y Carrito (Semana 3)

* [ ] Implementación de `CartProvider` para persistencia local del pedido.
* [ ] Flujo de "Checkout" y generación de órdenes en Firebase.
* [ ] Integración de diseño responsivo (Layouts adaptativos).

### Fase 4: Despliegue y QA (Semana 4)

* [ ] Pruebas de integración en Windows y Web.
* [ ] Generación de binarios para Android (.apk/.aab) e iOS.
* [ ] Auditoría de seguridad en reglas de Firestore.

---

> **Nota de Arquitectura:** Al utilizar el entorno Antigravity, es vital asegurar que el consumo de memoria del `Provider` sea monitoreado, especialmente en la versión Web, evitando fugas de memoria en las suscripciones de los `Streams` de Firestore.

¿Deseas que procedamos con el modelado de la base de datos en Firestore o prefieres comenzar con la estructura base del `main.dart`?
