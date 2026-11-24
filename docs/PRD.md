# PRD · <planix-app>

> Documento de requisitos del producto (Product Requirements Document).
> Complétalo en el Sprint 0 (historia H0.3) y pide la aprobación del profesor en un Issue.
> Cada requisito funcional (RF) se convierte en un Issue con la plantilla "Historia de usuario".

| Campo | Valor |
| --- | --- |
| Alumno | Elisneidy Gabriela Herrera López |
| Proyecto n.º | 6 |
| Sitio de referencia | Trello |
| Compañero revisor | Por asignar |
| URL desplegada | (se completa en el Sprint 5) |

## 1. Problema

Las personas y equipos pequeños que organizan tareas o proyectos suelen perder visibilidad del estado real de sus pendientes cuando dependen de notas dispersas o listas planas sin etapas. Esto genera olvidos, cuellos de botella y dificultad para priorizar qué hacer primero. `planix-app` resuelve este problema ofreciendo tableros visuales estilo Kanban donde cada tarea avanza por columnas claras.

## 2. Usuarios

- **Usuario principal:** Estudiante, profesional independiente o coordinador de proyecto que necesita organizar tareas diarias, prioridades y entregas desde computadora o navegador móvil.
- **Usuario secundario:** Colaborador o compañero de equipo que consulta el estado de las tareas asignadas y actualiza su avance.

## 3. Objetivo y métrica de éxito

Permitir que un usuario cree un tablero, organice columnas y registre o reordene una tarjeta de tarea mediante arrastrar y soltar en menos de 15 segundos con persistencia confiable.

## 4. Alcance del MVP

Entre 5 y 8 funcionalidades que SÍ entran este semestre.

1. Gestión de tableros (crear, listar, renombrar y eliminar tableros).
2. Gestión de columnas/listas dentro de un tablero (crear, reordenar y renombrar etapas como Pendiente, En progreso y Hecho).
3. Gestión de tarjetas/tareas (crear, editar título/descripción, asignar prioridad o etiqueta y eliminar).
4. Reordenamiento de tarjetas mediante lista doble enlazada y arrastrar y soltar (*drag and drop*) dentro de una columna o entre columnas.
5. Filtrado y búsqueda rápida de tarjetas por estado, etiqueta o texto.
6. Registro y consulta de historial/actividad básica de cambios en el tablero.

### Funcionalidades → carpetas de `src/` (screaming architecture)

Cada funcionalidad del alcance será una carpeta en `src/`. Nómbralas en inglés y en plural.

| Funcionalidad | Carpeta |
| --- | --- |
| Tableros Kanban | `src/boards` |
| Columnas o listas de estado | `src/lists` |
| Tarjetas o tareas | `src/cards` |
| Etiquetas y filtros | `src/labels` |
| Historial de actividad | `src/activities` |

## 5. Fuera de alcance

Lo que NO vas a construir.

- Autenticación avanzada con múltiples roles empresariales o permisos granulares por organización.
- Notificaciones en tiempo real por correo electrónico, Slack o mensajería externa.
- Adjuntos pesados de archivos en nube externa o automatizaciones complejas de terceros.

## 6. Requisitos funcionales

| ID | Requisito | Prioridad (alta, media, baja) |
| --- | --- | --- |
| RF-01 | Crear, consultar y eliminar tableros Kanban persistidos en base de datos | alta |
| RF-02 | Crear, renombrar y reordenar columnas dentro de un tablero | alta |
| RF-03 | Crear, editar y eliminar tarjetas con título, descripción y estado/columna | alta |
| RF-04 | Reordenar tarjetas con lista doble enlazada y arrastrar y soltar (*drag and drop*) conservando enlaces y orden | alta |
| RF-05 | Filtrar tarjetas por columna, prioridad/etiqueta o palabra clave | media |
| RF-06 | Registrar eventos clave de creación y movimiento de tarjetas | baja |
## 7. Requisitos no funcionales

- Responsive, móvil primero (375 px y 1280 px).
- Lighthouse ≥ 90 en Accesibilidad y Buenas prácticas.
- 0 errores en la consola del navegador.
- Funciona con 10.000 registros sin congelarse.

## 8. Modelo de datos

Diagrama entidad-relación y tablas PostgreSQL con tipos, claves y restricciones.


```mermaid
erDiagram
  BOARD ||--o{ COLUMN : contiene
  COLUMN ||--o{ CARD : agrupa
  CARD ||--o| CARD : enlaza_siguiente
  BOARD {
    int id PK
    text title
    timestamptz created_at
  }
  COLUMN {
    int id PK
    int board_id FK
    text title
    int position
  }
  CARD {
    int id PK
    int column_id FK
    int prev_card_id FK
    int next_card_id FK
    text title
    text description
    int position
  }

## 9. Reto técnico distintivo

Lista doble enlazada para reordenar tarjetas + arrastrar y soltar. Utilizaré una estructura de lista doblemente enlazada (implementada en src/shared/structures/) para representar y reubicar nodos de tarjetas con referencias anterior/siguiente al arrastrar y soltar entre posiciones o columnas, manteniendo sincronizado el estado visual y la persistencia en PostgreSQL.

## 10. Riesgos y preguntas abiertas

- Mantener consistentes los enlaces (prev_card_id / next_card_id) y posiciones de las tarjetas al mover elementos entre listas mediante drag and drop.

- Asegurar que todas las consultas SQL y migraciones cumplan las reglas del proyecto y pasen las pruebas automatizadas.
