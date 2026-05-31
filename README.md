# jimenez-post1-u8

**Programación Web — Unidad 8: Persistencia con JPA/Hibernate**  
**Post-Contenido 1 — CRUD de Estudiantes**  
**Autor:** Andres Felipe Jimenez Ramirez  
**Universidad Francisco de Paula Santander — 2026**

---

## Descripción

Aplicación web desarrollada con **Spring Boot 3.2**, **Spring Data JPA** e **Hibernate** como proveedor ORM, conectada a **MySQL 8**. Implementa un CRUD completo de la entidad `Estudiante` con validaciones, arquitectura en capas y vistas Thymeleaf.

---

## Tecnologías utilizadas

| Tecnología      | Versión  |
|-----------------|----------|
| Java            | 17       |
| Spring Boot     | 3.2.5    |
| Spring Data JPA | 3.2.5    |
| Hibernate       | 6.x      |
| MySQL           | 8.x      |
| Thymeleaf       | 3.1.x    |
| Maven           | 3.x      |

---

## Estructura del proyecto

```
src/main/java/com/universidad/estudiantes/
├── EstudiantesApplication.java       ← Clase principal
├── controller/
│   └── EstudianteController.java     ← Rutas HTTP (CRUD)
├── model/
│   └── Estudiante.java               ← Entidad JPA
├── repository/
│   └── EstudianteRepository.java     ← Acceso a datos (JpaRepository)
└── service/
    └── EstudianteService.java        ← Lógica de negocio (@Transactional)

src/main/resources/
├── application.properties            ← Configuración BD y JPA
└── templates/estudiantes/
    ├── lista.html                    ← Lista de estudiantes
    ├── formulario.html               ← Crear / Editar
    └── confirmar-eliminar.html       ← Confirmación de borrado
```

---

## Configuración de MySQL

### 1. Crear la base de datos y el usuario

```sql
$ mysql -u root -p

mysql> CREATE DATABASE estudiantes_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
mysql> CREATE USER 'appuser'@'localhost' IDENTIFIED BY 'apppass';
mysql> GRANT ALL PRIVILEGES ON estudiantes_db.* TO 'appuser'@'localhost';
mysql> FLUSH PRIVILEGES;
mysql> EXIT;
```

> **Alternativa con Docker:**
> ```bash
> docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root --name mysql8 mysql:8
> ```

### 2. Configurar `application.properties`

El archivo ya está preconfigurado en `src/main/resources/application.properties`.  
Ajusta las credenciales si usas valores distintos:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/estudiantes_db?useSSL=false&serverTimezone=UTC
spring.datasource.username=appuser
spring.datasource.password=apppass
spring.jpa.hibernate.ddl-auto=update
```

> `ddl-auto=update` hace que Hibernate cree la tabla `estudiantes` automáticamente al iniciar.

---

## Ejecución

```bash
# Clonar o descomprimir el proyecto
cd jimenez-post1-u8

# Ejecutar con Maven Wrapper
./mvnw spring-boot:run

# O en Windows:
mvnw.cmd spring-boot:run
```

La aplicación quedará disponible en: **http://localhost:8080/estudiantes**

---

## Verificación de la base de datos

```sql
USE estudiantes_db;
SHOW TABLES;
-- Resultado esperado: tabla 'estudiantes'

DESCRIBE estudiantes;
-- Muestra columnas: id, nombre, apellido, correo (UNIQUE), carrera

SELECT * FROM estudiantes;
-- Muestra los registros guardados
```

---

## Rutas disponibles

| Método | URL                          | Descripción                  |
|--------|------------------------------|------------------------------|
| GET    | /estudiantes                 | Lista todos los estudiantes  |
| GET    | /estudiantes/nuevo           | Formulario de creación       |
| POST   | /estudiantes/guardar         | Guarda nuevo / actualiza     |
| GET    | /estudiantes/editar/{id}     | Formulario de edición        |
| GET    | /estudiantes/eliminar/{id}   | Pantalla de confirmación     |
| POST   | /estudiantes/eliminar/{id}   | Elimina el estudiante        |

---

