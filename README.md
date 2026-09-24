# Maleta Go

Aplicación móvil para planificar viajes, descubrir lugares turísticos y compartir recomendaciones y fotos. También permitirá publicar viajes con espacio disponible en el equipaje y enviar solicitudes para transportar objetos.

## Estado

Proyecto académico en etapa de definición. Este repositorio reúne la documentación inicial y servirá para desarrollar, paso a paso, la aplicación Android, el backend y el portal administrativo.

## Tecnologías acordadas

- **Aplicación móvil:** Android Studio, Java y XML.
- **Persistencia móvil:** SQLite para guardar información local y sincronizarla con el servidor.
- **Backend:** API REST con Spring Boot.
- **Base de datos central:** relacional, alojada en un servicio gratuito por definir.
- **Portal administrativo:** CRUD para gestionar las tablas del sistema; tecnología por definir.

## Alcance inicial

1. Explorar lugares turísticos por destino y consultar información compartida por usuarios.
2. Crear un viaje y organizar lugares en un itinerario.
3. Publicar fotos y recomendaciones de viaje.
4. Publicar viajes con capacidad disponible en el equipaje y gestionar solicitudes de transporte.
5. Guardar información en el dispositivo y sincronizarla mediante la API REST.
6. Administrar usuarios, viajes, solicitudes, lugares y recomendaciones desde el portal administrativo.

El alcance se ajustará a los lineamientos del curso y a las decisiones que tomemos al diseñar los casos de uso. Pagos, chat, verificación de identidad y proveedor de mapas quedan pendientes de evaluación.

## Estructura del repositorio

```text
MaletaGo/
├── android-app/       # Proyecto Android Studio
├── backend/           # API REST Spring Boot
├── docs/              # Informe, requisitos y diseño
├── .github/           # Plantillas para incidencias y pull requests
├── CONTRIBUTING.md    # Flujo de colaboración
└── README.md
```

## Ruta de trabajo

1. Definir requisitos, alcance y casos de uso.
2. Diseñar el modelo de datos y la base de datos.
3. Crear la API REST y probarla.
4. Construir el portal administrativo.
5. Crear las pantallas Android, listas y almacenamiento SQLite.
6. Conectar Android con más de un servicio REST y sincronizar datos.
7. Probar el sistema, completar el informe y preparar la demostración.

## Documentación

- [Alcance y requisitos iniciales](docs/ALCANCE_Y_REQUISITOS.md)
- [Arquitectura, módulos y capas](docs/ARQUITECTURA_Y_MODULOS.md)
- [Decisiones pendientes](docs/DECISIONES_PENDIENTES.md)
- [Base del informe del curso](docs/Maleta_Go_Documentacion_Base.docx)

## Colaboración

Lee [CONTRIBUTING.md](CONTRIBUTING.md) antes de crear cambios. Las nuevas funciones se trabajarán en ramas y se integrarán mediante pull requests.

## Seguridad

No subas contraseñas, claves de servicios, archivos `.env`, certificados ni datos personales reales. Usa datos ficticios durante el desarrollo.
