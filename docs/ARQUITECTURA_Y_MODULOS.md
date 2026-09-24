# Arquitectura y módulos de Maleta Go

## 1. Decisión de arquitectura

Maleta Go se organizará por **módulos funcionales** y, dentro de cada aplicación, por **capas**. Los módulos describen qué hace el sistema; las capas separan la interfaz, las reglas del proyecto y el acceso a los datos.

La aplicación Android guardará y consultará información localmente con SQLite. Cuando corresponda sincronizar, enviará solicitudes HTTP a la API REST de Spring Boot. El backend validará esas solicitudes y trabajará con MySQL. Android no se conectará directamente a MySQL.

```text
Android (Java/XML)
  Vista → Caso de uso → Repositorio → SQLite o API REST
                                      ↓
                               Spring Boot
                         Controller → Use case/service
                                    → Repository → MySQL
```

## 2. Módulos funcionales

| Módulo | Función principal | Prioridad |
|---|---|---|
| Cuenta y perfil | Registrar al usuario y permitir que gestione sus datos básicos. | Inicial |
| Destinos y lugares | Consultar destinos y lugares de interés con su descripción. | Inicial |
| Planificación de viaje | Crear un viaje personal y guardar lugares en un itinerario. | Inicial |
| Viajes con espacio en equipaje | Publicar origen, destino, fecha y capacidad disponible. | Inicial |
| Solicitudes de transporte | Buscar viajes compatibles, solicitar el traslado y consultar su estado. | Inicial |
| Portal administrativo | Mantener usuarios, destinos, lugares, viajes y solicitudes. | Inicial del sistema |
| Comunidad de viajeros | Publicar fotos, comentarios y recomendaciones de usuarios. | Ampliación posterior |

La planificación turística y el transporte de equipaje se conectan mediante el mismo viaje: quien planifica una ruta puede indicar si ofrece espacio en su maleta.

## 3. Capas y términos

| Carpeta o concepto | Responsabilidad | Ejemplo |
|---|---|---|
| `presentation` o `view` | Pantallas, navegación, listas y captura de datos. | `ViajesActivity.java`, `activity_viajes.xml` |
| `domain/model` | Objetos que representan conceptos y reglas del negocio en la app. | `Viaje.java`, `Destino.java` |
| `usecase` | Acciones concretas que realiza la persona usuaria. | `BuscarViajesUseCase.java` |
| `repository` | Decide de dónde obtener o dónde guardar datos. | Leer SQLite y enviar cambios a la API. |
| `entity` | Representación de un registro persistido en una base de datos. | `ViajeLocalEntity.java` en SQLite o `ViajeEntity.java` en Spring/JPA. |
| `dto` | Estructura de los datos que entran o salen de la API. | `CrearViajeRequest.java`, `ViajeResponse.java` |
| `controller` | Recibe acciones HTTP y las dirige al caso de uso del backend. | `ViajeRestController.java` |

No conviene crear `entity` y `model` como dos nombres para la misma clase. En Android, `domain/model` representa el concepto de negocio y `data/local/entity` representa cómo se guarda localmente. En Spring Boot, `entity` representa las tablas y `dto` define el JSON de la API.

## 4. Estructura propuesta de Android

```text
android-app/
└── app/src/main/
    ├── java/com/maletago/app/
    │   ├── core/
    │   │   ├── database/       # SQLiteOpenHelper y creación/actualización de tablas
    │   │   ├── network/        # Configuración del cliente HTTP
    │   │   └── common/         # Constantes y utilidades compartidas
    │   ├── data/
    │   │   ├── local/
    │   │   │   ├── entity/     # Filas locales de SQLite
    │   │   │   └── dao/        # Consultas CRUD a SQLite
    │   │   ├── remote/
    │   │   │   ├── api/        # Interfaces para los endpoints REST
    │   │   │   └── dto/        # JSON de solicitud y respuesta
    │   │   └── repository/     # Implementaciones que coordinan origen local/remoto
    │   ├── domain/
    │   │   ├── model/          # Usuario, Destino, Lugar, Viaje, SolicitudEnvio
    │   │   ├── repository/     # Contratos de acceso a datos
    │   │   └── usecase/        # Acciones de la aplicación
    │   └── presentation/
    │       ├── auth/           # Registro, ingreso y perfil
    │       ├── destinations/   # Destinos y lugares
    │       ├── trips/          # Planificación e itinerario
    │       ├── luggage/        # Viajes publicados y espacio disponible
    │       └── shipments/      # Solicitudes y estados de envío
    └── res/
        ├── layout/             # XML de pantallas y elementos de listas
        ├── drawable/           # Iconos y fondos
        └── values/              # Colores, textos y temas
```

En una primera versión Java/XML, cada módulo de `presentation` tendrá Activities o Fragments, adaptadores de listas y sus layouts XML. SQLite se implementará con `SQLiteOpenHelper`, como se trabaja en el curso. Para consumir la API usaremos la biblioteca REST que indique el docente; el manual del curso presenta Retrofit y Volley.

## 5. Estructura propuesta de Spring Boot

El portal administrativo y la API pueden vivir inicialmente en el mismo proyecto Spring Boot. Se recomienda usar Thymeleaf para las páginas administrativas si el docente no pide otro framework.

```text
backend/
└── src/
    ├── main/
    │   ├── java/com/maletago/backend/
    │   │   ├── config/                 # Configuración de Spring
    │   │   ├── common/exception/       # Errores y respuestas comunes
    │   │   ├── usuario/
    │   │   │   ├── entity/             # UsuarioEntity (tabla MySQL)
    │   │   │   ├── dto/                # Peticiones y respuestas JSON
    │   │   │   ├── repository/         # Acceso a datos con Spring Data JPA
    │   │   │   ├── usecase/            # Reglas y acciones de usuario
    │   │   │   └── controller/         # Endpoints REST
    │   │   ├── destino/                # Mismas capas para destinos y lugares
    │   │   ├── viaje/                  # Mismas capas para viajes e itinerarios
    │   │   ├── solicitud/              # Mismas capas para envíos y estados
    │   │   └── admin/                  # Controladores de páginas administrativas
    │   └── resources/
    │       ├── templates/admin/        # Vistas HTML Thymeleaf del portal
    │       ├── static/                 # CSS, JavaScript e imágenes del portal
    │       └── application.properties  # Configuración local; sin claves en Git
    └── test/java/com/maletago/backend/ # Pruebas por módulo
```

En Spring Boot, `usecase` puede implementarse como servicios de aplicación. Cada controller valida la petición, llama al caso de uso y devuelve una respuesta; el caso de uso trabaja con el repository, y el repository persiste las entidades en MySQL.

## 6. Primeros casos de uso

- `RegistrarUsuarioUseCase`
- `ListarDestinosUseCase`
- `ListarLugaresPorDestinoUseCase`
- `CrearViajeUseCase`
- `AgregarLugarAlItinerarioUseCase`
- `BuscarViajesCompatiblesUseCase`
- `CrearSolicitudEnvioUseCase`
- `ActualizarEstadoSolicitudUseCase`
- `SincronizarViajesUseCase` y `SincronizarSolicitudesUseCase`

Las fotos y publicaciones sociales se dejarán para una ampliación, una vez que el flujo principal funcione.

## 7. Flujo de datos

1. La vista Android captura los datos y llama al caso de uso.
2. El caso de uso aplica las reglas del módulo.
3. El repository guarda primero en SQLite cuando se requiere persistencia local.
4. El repository usa la API REST para enviar o consultar información del servidor.
5. Spring Boot recibe la petición, ejecuta su lógica y guarda los datos en MySQL.
6. La respuesta vuelve a Android y la pantalla actualiza la información.

## 8. Orden recomendado de construcción

1. Definir las entidades y relaciones: Usuario, Destino, LugarTuristico, Viaje, ElementoItinerario y SolicitudEnvio.
2. Diseñar el modelo entidad-relación y el esquema físico de MySQL.
3. Crear los proyectos Android y Spring Boot en sus carpetas.
4. Implementar CRUD local de SQLite y sus pruebas manuales.
5. Crear endpoints REST para destinos, viajes y solicitudes.
6. Crear las vistas CRUD del portal administrativo.
7. Consumir más de un endpoint desde Android y probar sincronización.
8. Completar las pantallas, validaciones, pruebas e informe del curso.

