# SQL Queries — Consultas y programación en PostgreSQL

Colección de trabajos prácticos de bases de datos con SQL y PL/pgSQL. Incluye funciones almacenadas, disparadores, auditoría, mantenimiento de datos derivados, consultas con funciones de ventana y ejercicios de administración y rendimiento.

El material utiliza principalmente los esquemas de ejemplo **Northwind** y **DellStore2**. Los archivos conservan distintas soluciones, pruebas y borradores de los ejercicios; se deben revisar y ejecutar por fragmentos según la base y las dependencias de cada práctica.

Estos ejercicios prácticos fueron realizados durante el cursado de diversas materias mientras estudiaba la **Licenciatura en Análisis de Sistemas (Plan 2010)** de la **Universidad Nacional de Salta**.

**Materia: Base de Datos I & II**

## Contenido

- [SQL Queries — Consultas y programación en PostgreSQL](#sql-queries--consultas-y-programación-en-postgresql)
  - [Contenido](#contenido)
  - [Estructura](#estructura)
  - [Trabajos prácticos principales](#trabajos-prácticos-principales)
  - [Material de introducción](#material-de-introducción)
  - [Requisitos y preparación](#requisitos-y-preparación)
  - [Cómo trabajar con los ejercicios](#cómo-trabajar-con-los-ejercicios)
  - [Estado y limitaciones](#estado-y-limitaciones)
  - [Licencia](#licencia)

## Estructura

```text
.
├── BDII-TP1.sql
├── BDII-TP2.sql
├── TP3-BDII.sql
├── TP4-BDII.sql
├── Introduction/
│   ├── tp1-funciones.sql
│   ├── tp2-triggers.sql
│   ├── tp2-triggers2.sql
│   ├── tp3.sql
│   ├── tp4-postprocessing.sql
│   ├── tp4dellstore.sql
│   ├── Ejercicio5.sql
│   └── Ejercicio6.sql
├── LICENSE
└── README.md
```

## Trabajos prácticos principales

| Archivo | Temas |
| --- | --- |
| [BDII-TP1.sql](BDII-TP1.sql) | Funciones PL/pgSQL para limpieza de texto, resumen de órdenes, totales, consultas entre fechas, datos de empleados y clientes, acumulados y generación de órdenes de compra. Incluye variantes con SQL dinámico. |
| [BDII-TP2.sql](BDII-TP2.sql) | Disparadores para validar proveedores y cantidades, actualizar existencias, registrar auditoría y mantener totales derivados de los detalles de órdenes. |
| [TP3-BDII.sql](TP3-BDII.sql) | Inserción de regiones y territorios, `INSERT ... SELECT`, columnas de cantidad e importe de órdenes por cliente, actualización de redundancias y alternativas para eliminar el historial de un cliente. |
| [TP4-BDII.sql](TP4-BDII.sql) | Funciones de ventana para conteos, acumulados, promedios y rankings; creación de `customersnew` y ejercicios de selección y eliminación por grupos de país y estado. |

Los tres primeros trabajos utilizan tablas como `customers`, `orders`, `orderdetails`, `products`, `employees`, `suppliers`, `region` y `territories`. El cuarto combina ejercicios de Northwind con una sección de estructura de clientes correspondiente a DellStore2.

## Material de introducción

La carpeta [Introduction](Introduction/) conserva enunciados parciales, otras resoluciones y experimentos sobre los mismos temas.

| Archivo | Contenido |
| --- | --- |
| [tp1-funciones.sql](Introduction/tp1-funciones.sql) | Funciones de limpieza, resumen y totalización de órdenes, ventas entre fechas, conteos mensuales, compras y resultados por país. |
| [tp2-triggers.sql](Introduction/tp2-triggers.sql) | Validaciones de proveedores y cantidades, y actualización del stock mediante disparadores. |
| [tp2-triggers2.sql](Introduction/tp2-triggers2.sql) | Variante ampliada de los disparadores, con una tabla y función de auditoría de clientes. |
| [tp3.sql](Introduction/tp3.sql) | Regiones, territorios, tablas auxiliares y datos redundantes de órdenes por cliente. |
| [tp4-postprocessing.sql](Introduction/tp4-postprocessing.sql) | Consultas de acumulados y rankings sobre Northwind, junto con consignas para DellStore2. Algunos puntos solo contienen el enunciado. |
| [tp4dellstore.sql](Introduction/tp4dellstore.sql) | Copia de clientes a `customersnew`, particiones por país y estado, ranking y alternativas para conservar un máximo de filas por grupo. |
| [Ejercicio5.sql](Introduction/Ejercicio5.sql) | Exportación e importación con `COPY`, restricciones, funciones de validación, referencias a `dblink` y creación de un tablespace. |
| [Ejercicio6.sql](Introduction/Ejercicio6.sql) | Apuntes y pruebas de rendimiento: configuración de `pg_stat_statements`, consultas e índices sobre DellStore2. Mezcla instrucciones en texto, parámetros de configuración y SQL. |

Las versiones de la raíz y de `Introduction/` son material alternativo. No forman una secuencia de migraciones que pueda aplicarse completa y automáticamente.

## Requisitos y preparación

Se necesita PostgreSQL con PL/pgSQL y un cliente como `psql` o un editor de consultas. Para cada práctica hay que disponer de una base de laboratorio con el esquema y los datos de Northwind o DellStore2 correspondientes.

El repositorio no incluye los respaldos completos ni un script de instalación de esas bases. Algunas consultas necesitan columnas agregadas durante los ejercicios, como `ordersquantity`, `ordersamount` o `salary`; disponer de una versión básica del esquema puede no ser suficiente.

Para obtener el material desde Bash:

```bash
git clone https://github.com/GabrielUnsa/SQL-Queries.git
cd SQL-Queries
```

Los ejercicios de administración tienen requisitos adicionales: rutas del servidor adaptadas al equipo, permisos para `COPY` o tablespaces y, donde corresponda, extensiones y configuración del servidor. Las rutas `/home/gabriel/...` que aparecen en los fuentes corresponden al entorno original.

## Cómo trabajar con los ejercicios

1. Elegir un archivo y determinar si el fragmento utiliza Northwind o DellStore2.
2. Revisar las tablas, columnas y funciones que espera. Crear primero las funciones de las que dependan los disparadores o consultas.
3. Seleccionar una resolución cuando haya varias alternativas del mismo ejercicio y corregir los bloques incompletos antes de ejecutarlos.
4. Ejecutar el fragmento en una base de laboratorio y comprobar los resultados con consultas antes y después del cambio.

Por ejemplo, si la base de laboratorio Northwind ya está cargada, se puede conectar con `psql`:

```bash
psql -d northwind
```

Si tiene otro nombre o requiere parámetros de conexión, ajustar el comando al servidor utilizado. Dentro del cliente, inspeccionar las relaciones y probar una consulta de lectura:

```sql
SELECT customerid, companyname
FROM customers
ORDER BY customerid
LIMIT 10;
```

Para explorar una modificación de datos de manera reversible, se puede ejecutar un fragmento revisado dentro de una transacción y comprobar su efecto antes de revertirla:

```sql
BEGIN;

-- Ejecutar aquí el fragmento elegido y las consultas de comprobación.

ROLLBACK;
```

Esta forma de trabajo aplica a los ejercicios de datos y funciones que admiten transacciones. Las instrucciones de administración, como la creación de tablespaces o los cambios de configuración, se tratan por separado.

## Estado y limitaciones

La documentación se basa en una revisión estática de los doce archivos SQL. No se conectó a un servidor ni se ejecutaron cambios sobre bases de datos; no se verificó la compatibilidad con una versión concreta de PostgreSQL.

Los archivos conservan funciones redefinidas, disparadores declarados antes de sus funciones, sentencias sin terminar y fragmentos señalados como incorrectos. Por ejemplo, `TP4-BDII.sql` incluye un `DELETE ... NOT IN` incompleto y `Ejercicio6.sql` contiene texto explicativo sin comentar. Su extensión `.sql` no implica que puedan ejecutarse íntegramente mediante `psql -f`.

Las prácticas incluyen `INSERT`, `UPDATE`, `DELETE`, cambios de esquema y operaciones de administración que modifican datos o recursos del servidor. Las consultas de eliminación y los disparadores requieren revisar especialmente las relaciones y condiciones para asegurar que afectan solo las filas previstas. No se incluyen pruebas automatizadas ni un proceso de restauración de las bases de ejemplo.

## Licencia

Distribuido bajo la [licencia MIT](LICENSE). Copyright © 2025 Gabriel Marmanillo.
