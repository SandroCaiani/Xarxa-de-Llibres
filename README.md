# 📚 Proyecto Intermodular – Xarxa de Llibres

## 1. Introducción

El proyecto **Xarxa de Llibres** forma parte del **Proyecto Intermodular** del ciclo formativo y tiene como objetivo el diseño y desarrollo de un **sistema de gestión de banco de libros** para centros educativos.

El sistema permite controlar:

* Libros y ejemplares físicos
* Alumnos y cursos
* Préstamos y devoluciones
* Historial y estado físico de los ejemplares

El enfoque del proyecto es **académico y profesional**, aplicando buenas prácticas de **análisis, modelado entidad–relación, normalización, diseño relacional, DDL en MySQL y modelado de casos de uso UML**.

---

## 2. Alcance del Sistema

El sistema cubre las siguientes funcionalidades principales:

* Gestión de libros y ejemplares físicos
* Asociación de libros a cursos autorizados
* Gestión de alumnos
* Registro y control de préstamos
* Registro de devoluciones y evaluación del estado del libro
* Consulta del historial completo de un ejemplar

Quedan fuera de alcance en este sprint:

* Gestión de usuarios y autenticación
* Penalizaciones económicas
* Gestión de incidencias administrativas

---

## 3. Modelo Entidad–Relación

### 3.1 Entidades Principales

| Entidad         | Descripción                         |
| --------------- | ----------------------------------- |
| **LIBRO**       | Información bibliográfica del libro |
| **EJEMPLAR**    | Copia física identificada por QR    |
| **ALUMNO**      | Alumno del centro educativo         |
| **CURSO**       | Curso académico / nivel             |
| **PRESTAMO**    | Cesión temporal de un ejemplar      |
| **DEVOLUCION**  | Cierre del préstamo y evaluación    |
| **LIBRO_CURSO** | Relación N:M libro–curso            |

---

### 3.2 Entidades y Atributos

#### LIBRO

* ISBN (PK)
* titulo
* autor
* fecha_creacion
* fecha_modificacion

#### EJEMPLAR

* id_ejemplar (PK)
* ISBN (FK)
* codigo_qr (UNIQUE)
* volumen
* estado (disponible / prestado)
* condicion (nuevo / usado / deteriorado)
* fecha_creacion
* fecha_modificacion

#### ALUMNO

* NIA (PK)
* nombre
* apellidos
* id_curso (FK)
* grupo
* fecha_creacion
* fecha_modificacion

#### CURSO

* id_curso (PK)
* nombre_curso

#### PRESTAMO

* id_prestamo (PK)
* id_ejemplar (FK)
* NIA (FK)
* fecha_prestamo
* estado (activo / finalizado)
* condicion (estado del libro al prestar)
* fecha_creacion
* fecha_modificacion

#### DEVOLUCION

* id_devolucion (PK)
* id_prestamo (FK – UNIQUE)
* fecha_devolucion
* condicion (correcto / deteriorado)
* fecha_creacion
* fecha_modificacion

---

### 3.3 Relaciones

| Relación              | Cardinalidad        |
| --------------------- | ------------------- |
| Libro – Ejemplar      | 1 : N               |
| Ejemplar – Préstamo   | 1 : N               |
| Préstamo – Devolución | 1 : 1               |
| Alumno – Préstamo     | 1 : N               |
| Curso – Alumno        | 1 : N               |
| Libro – Curso         | N : M (LIBRO_CURSO) |

> **Nota:** Devolución se considera una **entidad débil**, dependiente del préstamo.

---

## 4. Modelo Relacional

El modelo relacional resultante cumple **3FN**, evita redundancias y garantiza integridad referencial mediante claves primarias, foráneas y restricciones.

Las tablas finales son:

* LIBRO
* EJEMPLAR
* ALUMNO
* CURSO
* LIBRO_CURSO
* PRESTAMO
* DEVOLUCION

---

## 5. DDL – Script SQL (MySQL)

El proyecto incluye un **script SQL versión v3** completamente funcional, con:

* Claves primarias y foráneas
* Restricciones UNIQUE
* ENUMs para estados
* Auditoría temporal (fecha_creacion / fecha_modificacion)

> El script permite crear la base de datos desde cero y garantiza consistencia de datos.

---

## 6. Casos de Uso

### Actor Principal

**Responsable del Banco de Libros**

---

### CU-01 – Registrar préstamo

**Descripción**
Permite registrar el préstamo de un ejemplar a un alumno, validando las normas del programa Xarxa de Llibres.

**Precondiciones**

* El alumno existe
* El ejemplar existe
* El ejemplar está disponible
* El libro está autorizado para el curso

**Flujo principal**

1. Buscar alumno
2. Escanear QR del ejemplar
3. Validar restricciones
4. Registrar préstamo
5. Marcar ejemplar como prestado

**Flujos alternativos**

* Ejemplar ya prestado
* Libro no autorizado para el curso
* Alumno ya dispone del libro

**Postcondiciones**

* Préstamo activo
* Ejemplar prestado

---

### CU-02 – Registrar devolución

Permite registrar la devolución y evaluar el estado físico del libro.

Incluye:

* Localizar préstamo activo
* Registrar condición
* Cerrar préstamo
* Marcar ejemplar como disponible

---

### CU-03 – Dar de alta ejemplar

Permite registrar un nuevo ejemplar físico asociado a un libro existente.

---

### CU-04 – Buscar alumno

Permite localizar alumnos por NIA, nombre o curso.

---

### CU-05 – Buscar libro / ejemplar

Permite localizar libros por ISBN o ejemplares por QR.

---

### CU-06 – Ver historial de ejemplar

Permite consultar todos los préstamos y devoluciones de un ejemplar, incluyendo el estado del libro en cada operación.

---

### CU-07 – Validaciones del sistema (incluido)

Caso de uso transversal incluido en:

* Registrar préstamo
* Registrar devolución

Incluye:

* Validación de disponibilidad
* Validación de curso
* Validación de duplicidad

---

## 7. Estado del Proyecto – Sprint 1

### Completado

* Análisis de requisitos
* Modelo entidad–relación
* Modelo relacional
* Script DDL MySQL
* Definición completa de casos de uso

### Pendiente (Siguientes Sprints)

* Diagramas UML (ER y Casos de Uso)
* Capa de aplicación
* Interfaz de usuario
* Pruebas funcionales

---

## 8. Conclusión

El proyecto **Xarxa de Llibres** presenta una base sólida de análisis y diseño, alineada con estándares académicos y prácticas profesionales. La documentación generada permite una transición directa hacia las fases de diseño UML, implementación y pruebas en los siguientes sprints.

---

**Proyecto Intermodular – Xarxa de Llibres**
Ciclo Formativo – Documentación Técnica.

Autores: Sandro Caiani, Alessandro Fattor, Alex Nat.

