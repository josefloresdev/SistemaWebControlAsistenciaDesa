# Sistema de Control de Asistencias

Sistema web para la gestión y control de asistencias de empleados. Desarrollado con React, Spring Boot y MariaDB.

## Descripción

El sistema permite el registro digital de asistencia de empleados y la administración de entradas, salidas, retardos, ausencias y horarios laborales. Incluye geolocalización, generación de reportes en Excel, gestión de incidencias y vacaciones, y auditoría de accesos.

## Tecnologías

### Backend
- Spring Boot 3.2.0
- Java 17
- MariaDB 10.5+
- Spring Data JPA / Hibernate
- Spring Security + JWT
- SpringDoc OpenAPI (Swagger)
- Maven
- Apache POI (exportación Excel)
- Lombok
- Dotenv Java
- Spring Mail

### Frontend
- React 19.2.0
- Vite 7.3.1
- React Router DOM 6.28.0
- Axios 1.7.7
- html5-qrcode 2.3.8
- qrcode 1.5.3
- ESLint 9.39.1

## Estructura del Proyecto

```
SistemaControlAsistencias/
├── backend/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/miptech/
│   │   │   │   ├── config/         # Configuraciones (CORS, Swagger)
│   │   │   │   ├── controller/     # Controladores REST (12)
│   │   │   │   ├── dto/            # Data Transfer Objects (16)
│   │   │   │   ├── model/          # Entidades JPA (10)
│   │   │   │   ├── repository/     # Repositorios JPA (10)
│   │   │   │   ├── scheduler/      # Tareas programadas
│   │   │   │   ├── security/       # Seguridad JWT
│   │   │   │   ├── service/        # Lógica de negocio (14)
│   │   │   │   └── util/           # Utilidades
│   │   │   └── resources/
│   │   │       └── application.properties
│   │   └── test/
│   ├── database/              # Scripts de base de datos
│   ├── uploads/              # Archivos subidos (evidencias)
│   ├── iniciar-servidor.ps1
│   ├── detener-puerto-8080.ps1
│   └── pom.xml
├── frontend/
│   ├── src/
│   │   ├── components/       # Componentes React (33)
│   │   ├── config/          # Configuración (Axios)
│   │   ├── context/         # Context API (AuthContext)
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── public/
│   ├── package.json
│   └── vite.config.js
└── README.md
```

## Requisitos

- JDK 17 o superior (recomendado JDK 22)
- Maven 3.6+
- MariaDB 10.5+
- Node.js 18+
- npm (incluido con Node.js)
- Windows, Linux o macOS

## Instalación

### 1. Obtener el código

Si el proyecto está en un repositorio Git interno:

```bash
git clone <url-del-repositorio-interno>
cd SistemaControlAsistencias
```

O descomprimir el archivo del proyecto en el directorio deseado.

### 2. Base de datos

Crear la base de datos en MariaDB:

```sql
CREATE DATABASE control_asistencias CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Ejecutar scripts de inicialización si están disponibles en `backend/database/`.

### 3. Configuración del backend

#### Opción A: Variables de entorno (recomendado)

Crear archivo `.env` en `backend/`:

```env
# Base de Datos
SPRING_DATASOURCE_URL=jdbc:mariadb://localhost:3306/control_asistencias?useUnicode=true&characterEncoding=UTF-8&serverTimezone=America/Mexico_City&allowPublicKeyRetrieval=true&useSSL=false
SPRING_DATASOURCE_USERNAME=root
SPRING_DATASOURCE_PASSWORD=tu_contraseña

# JWT
JWT_SECRET=tu_clave_secreta_muy_segura_minimo_256_bits
JWT_EXPIRATION=3600000
JWT_REFRESH_EXPIRATION=86400000

# CORS
CORS_ALLOWED_ORIGINS=http://localhost:5173,http://localhost:3000

# Email (Opcional)
SPRING_MAIL_HOST=smtp.gmail.com
SPRING_MAIL_PORT=587
SPRING_MAIL_USERNAME=tu_email@gmail.com
SPRING_MAIL_PASSWORD=tu_contraseña_de_aplicacion

# Puerto del Servidor
SERVER_PORT=8080
```

El script `iniciar-servidor.ps1` carga automáticamente las variables desde `.env`.

#### Opción B: Configuración directa

Editar `backend/src/main/resources/application.properties` con las credenciales correspondientes.

### 4. Ejecutar el backend

#### Windows (PowerShell)

```powershell
cd backend
.\iniciar-servidor.ps1
```

El script configura JAVA_HOME, carga variables de entorno desde `.env`, verifica Java e inicia el servidor Spring Boot.

#### Linux/macOS o manual

```bash
cd backend
mvn clean install
mvn spring-boot:run
```

El servidor queda disponible en `http://localhost:8080`.

### 5. Ejecutar el frontend

```bash
cd frontend
npm install
npm run dev
```

El frontend queda disponible en `http://localhost:5173` (o el puerto asignado por Vite).

### 6. Acceso

- Frontend: http://localhost:5173
- Backend API: http://localhost:8080
- Swagger UI: http://localhost:8080/swagger-ui.html
- API Docs (JSON): http://localhost:8080/api-docs

## Módulos

### Autenticación y Seguridad
- Login con email y contraseña
- Generación de access token y refresh token
- Validación de tokens JWT
- Bitácora de accesos
- Rate limiting

### Gestión de Usuarios
- CRUD de usuarios
- Asignación de roles (ADMINISTRADOR, RECURSOS_HUMANOS, EMPLEADO)
- Activación/desactivación de usuarios
- Gestión de permisos

### Gestión de Empleados
- CRUD de empleados
- Asignación de horarios
- Gestión de información laboral
- Vinculación con usuarios

### Horarios y Turnos
- Creación y gestión de horarios
- Asignación de horarios a empleados
- Configuración de días laborales

### Registro de Asistencia
- Registro de entrada y salida
- Geolocalización (latitud/longitud)
- Validación de horarios
- Cálculo automático de estado (asistencia normal, retardo, falta)
- Consulta de historial personal y por empleado
- Rate limiting para prevenir registros duplicados

### Incidencias
- Registro de retardos, faltas y vacaciones
- Consulta y gestión de incidencias
- Validación de vacaciones activas
- Evidencias adjuntas (archivos PDF)

### Configuración
- Límites configurables para asistencia, retardos y faltas
- Configuración de parámetros del sistema
- Personalización de reglas de negocio

### Reportes
- Generación de reportes de asistencia
- Exportación a Excel (.xlsx)
- Filtros por fecha, empleado, tipo de registro
- Estadísticas y resúmenes

### Auditoría
- Bitácora de accesos al sistema
- Trazabilidad de acciones realizadas
- Registro de cambios importantes

## Roles

- **ADMINISTRADOR**: Acceso completo, gestión de usuarios, configuración del sistema
- **RECURSOS_HUMANOS**: Gestión de empleados, horarios, incidencias y reportes
- **EMPLEADO**: Registro y consulta de su propia asistencia, visualización de historial

## Documentación de la API

La API está documentada con Swagger/OpenAPI. Con el servidor en ejecución, acceder a:

- Swagger UI: http://localhost:8080/swagger-ui.html
- API Docs (JSON): http://localhost:8080/api-docs

La documentación incluye descripción de endpoints, parámetros requeridos y opcionales, ejemplos de requests y responses, códigos de respuesta HTTP, y autenticación JWT integrada.

### Autenticación en Swagger

Para probar endpoints protegidos:

1. Ejecutar `POST /api/auth/login` para obtener el token
2. Clic en "Authorize" (arriba a la derecha)
3. Ingresar: `Bearer {tu_access_token}` (sin llaves, solo el token)
4. Clic en "Authorize" y luego "Close"
5. Probar los endpoints protegidos

## Endpoints Principales

### Autenticación
- `POST /api/auth/login` - Iniciar sesión
- `POST /api/auth/logout` - Cerrar sesión
- `GET /api/auth/` - Obtener usuario actual
- `POST /api/auth/refresh-token` - Renovar token

### Usuarios
- `GET /api/usuarios` - Listar usuarios (paginación)
- `GET /api/usuarios/{id}` - Obtener usuario
- `POST /api/usuarios` - Crear usuario
- `PUT /api/usuarios/{id}` - Actualizar usuario
- `PATCH /api/usuarios/{id}/activar` - Activar usuario
- `PATCH /api/usuarios/{id}/desactivar` - Desactivar usuario

### Empleados
- `GET /api/empleados` - Listar empleados
- `GET /api/empleados/{id}` - Obtener empleado
- `POST /api/empleados` - Crear empleado
- `PUT /api/empleados/{id}` - Actualizar empleado
- `DELETE /api/empleados/{id}` - Eliminar empleado

### Asistencia
- `POST /api/asistencia/entrada` - Registrar entrada
- `POST /api/asistencia/salida` - Registrar salida
- `GET /api/asistencia/mis-registros` - Mis registros (empleado)
- `GET /api/asistencia/empleado/{id}` - Registros de empleado (admin/RH)

### Incidencias
- `GET /api/incidencias` - Listar incidencias
- `POST /api/incidencias` - Crear incidencia
- `GET /api/incidencias/{id}` - Obtener incidencia
- `PUT /api/incidencias/{id}` - Actualizar incidencia

### Reportes
- `GET /api/reportes/asistencia` - Generar reporte de asistencia
- `GET /api/reportes/asistencia/exportar` - Exportar reporte a Excel

### Configuración
- `GET /api/configuracion` - Obtener configuración
- `PUT /api/configuracion` - Actualizar configuración

## Modelo de Datos

Entidades principales:

- **Rol**: Roles del sistema (ADMINISTRADOR, RECURSOS_HUMANOS, EMPLEADO)
- **Usuario**: Cuentas de acceso al sistema
- **Empleado**: Información de empleados
- **Horario**: Horarios laborales
- **RegistroAsistencia**: Registros de entrada/salida
- **Incidencia**: Incidencias laborales (retardos, faltas)
- **Vacacion**: Registro de vacaciones
- **ConfiguracionAsistencia**: Configuración de límites y parámetros
- **BitacoraAccesos**: Auditoría de accesos al sistema

## Seguridad

- Contraseñas encriptadas con BCrypt
- Tokens JWT para autenticación stateless
- Refresh tokens para renovación segura
- Control de acceso basado en roles (RBAC)
- CORS configurado para desarrollo y producción
- Validación de datos de entrada
- Rate limiting para prevenir abusos
- Variables de entorno para información sensible

## Testing

### Backend

```bash
cd backend
mvn test
```

### Frontend

```bash
cd frontend
npm run lint
```

## Build para Producción

### Backend

```bash
cd backend
mvn clean package
java -jar target/sistema-control-asistencias-1.0.0.jar
```

### Frontend

```bash
cd frontend
npm run build
```

Los archivos estáticos se generan en `frontend/dist/`.

## Scripts

### Windows (PowerShell)

- `backend/iniciar-servidor.ps1` - Inicia el servidor Spring Boot con configuración automática
- `backend/detener-puerto-8080.ps1` - Detiene procesos usando el puerto 8080

### Comandos Maven

```bash
mvn clean install          # Compilar y empaquetar
mvn spring-boot:run        # Ejecutar aplicación
mvn test                   # Ejecutar tests
mvn clean                  # Limpiar archivos compilados
```

### Comandos npm

```bash
npm install                # Instalar dependencias
npm run dev                # Ejecutar en modo desarrollo
npm run build              # Compilar para producción
npm run preview            # Previsualizar build de producción
npm run lint               # Ejecutar linter
```

## Solución de Problemas

### El servidor no inicia

1. Verificar que Java esté instalado: `java -version`
2. Verificar que Maven esté instalado: `mvn -version`
3. Verificar que MariaDB esté ejecutándose
4. Revisar credenciales en `.env` o `application.properties`
5. Verificar que el puerto 8080 no esté en uso

### Error de conexión a la base de datos

1. Verificar que MariaDB esté ejecutándose
2. Verificar credenciales en `.env`
3. Verificar que la base de datos `control_asistencias` exista
4. Verificar la URL de conexión (host, puerto, nombre de BD)

### El frontend no se conecta al backend

1. Verificar que el backend esté ejecutándose en `http://localhost:8080`
2. Verificar configuración de CORS en `application.properties`
3. Verificar la URL del API en `frontend/src/config/axios.js`

### Error de autenticación JWT

1. Verificar que `JWT_SECRET` esté configurado en `.env`
2. Usar un secreto seguro (mínimo 256 bits)
3. Verificar que el token no haya expirado

## Licencia

Este proyecto es propiedad de MIPTECH S.A. DE C.V.

## Autor

José Alfredo Flores Rodríguez

## Versión

V1.0.0 - Febrero 2026

## Soporte

Para soporte técnico, consultas o reporte de incidencias, contactar al equipo de desarrollo de MIPTECH S.A. DE C.V.

---

Documentación interna - Uso exclusivo de MIPTECH S.A. DE C.V.
