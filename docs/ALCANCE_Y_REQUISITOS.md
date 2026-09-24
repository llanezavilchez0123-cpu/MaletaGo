# Alcance y requisitos iniciales

Este documento convierte la idea de Maleta Go en un primer conjunto de funciones. Es una propuesta de trabajo y debe validarse con el docente antes de cerrar el alcance.

## Usuarios previstos

- **Viajero:** publica un viaje, comparte lugares y recomendaciones y declara espacio disponible en su equipaje.
- **Solicitante:** busca un viaje compatible y envía una solicitud para transportar un objeto.
- **Administrador:** mantiene los registros del sistema desde el portal administrativo.

Una misma persona podría usar más de un rol; queda por confirmar si se representarán como roles distintos o como funciones de un único usuario.

## Requisitos funcionales preliminares

| Código | Requisito |
|---|---|
| RF01 | La app permitirá consultar lugares turísticos por destino. |
| RF02 | La app mostrará información y contenido compartido sobre cada lugar. |
| RF03 | El usuario podrá crear un viaje y añadir lugares a su itinerario. |
| RF04 | El usuario podrá publicar fotos y recomendaciones asociadas a un lugar o viaje. |
| RF05 | El viajero podrá publicar origen, destino, fecha y capacidad disponible en su equipaje. |
| RF06 | El solicitante podrá enviar una solicitud de transporte y consultar su estado. |
| RF07 | La app guardará datos seleccionados localmente en SQLite y los sincronizará con la API REST. |
| RF08 | El administrador podrá realizar operaciones CRUD sobre las entidades del sistema. |

## Requisitos técnicos del curso

- Aplicación móvil desarrollada en Android Studio.
- Interfaces Android con layouts y listas personalizadas.
- Persistencia local con SQLite y operaciones CRUD.
- API REST desarrollada con Spring Boot.
- Consumo desde Android de más de un servicio REST.
- Base de datos relacional remota y portal administrativo.

## Entidades iniciales por validar

`Usuario`, `Viaje`, `SolicitudEnvio`, `LugarTuristico`, `Recomendacion` y `ElementoItinerario`.

Antes de implementar, revisaremos atributos, relaciones, reglas de negocio y si se requieren entidades adicionales para fotos, categorías o estados.

## Fuera de alcance por ahora

No se implementarán todavía pagos, chat en tiempo real, verificación documental ni publicación en tiendas. Primero se confirmará su necesidad, viabilidad y relación con los criterios de evaluación.
