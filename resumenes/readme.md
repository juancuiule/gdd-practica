# Gestión de Datos

### Indice

* [Modelo Relacional](#modelo-relacional)
  * [Dominio](#dominio)
  * [Tabla](#tabla)
    * [Propiedades](#propiedades)
* [Integridad](#integridad)
* [Independencia de Datos](#independencia-de-datos)
* [Normalización](#normalización)
  * [1° Forma Normal](#1-forma-normal)
  * [2° Forma Normal](#2-forma-normal)
  * [3° Forma Normal](#3-forma-normal)
* [SQL](#sql)
  * [DDL](#ddl-data-definition-language)
  * [DML](#dml-data-management-language)
* [Tablas Temporales](#tablas-temporales)

### Modelo Relacional

- Definición de estrucuta: tablas
- Integridad de datos: obtener precisión y consistencia
- Manipulación de datos: SQL

#### Dominio
Conjunto de datos posibles, asociado a la semantica de los datos. No es **solo** el tipo de datos que puede tomar un campo. Esta relacionado con los valores posibles y validos dentro de nuestro negocio.

Por ejemplo:
- el legajo de un alumno de la facultad se compone de 6 digitos y un digito verificador.
- el telefono de una casa se compone de 8 digitos.
- un cuit tiene dos digitos - un dni - un digito verificador.

#### Tabla

Una tabla esta compuesta principalmente por una cabecera y un cuerpo

Cabecera: son el conjunto de columnas o campos que tiene un elemento de la tabla.
Cuerpo: es un conjunto de tuplas o filas.
Fila: es un conjunto de atributos correspondientes a los campos de la cabecera.

La **cardinalidad** de una table hace referencia a la cantidad de filas de una tabla y la **gradualidad** es la cantidad de columnas de la tabla.

##### Propiedades
* No hay tuplas repetidas, toda relación tiene clave primaria
* Las tuplas no estan ordenadas
* Los atributos no estan ordenados

### Integridad

Reglas de identidad:
* Clave Primaria (PK)
* Clave Foranea (hace referencia a la PK de otra relación) (FK)

En este ejemplo `codProv` es una FK a la tabla de `Provincias`

<table border="0">
 <tr>
    <td><b style="font-size:30px">Alumnos</b></td>
    <td><b style="font-size:30px">Provincias</b></td>
 </tr>
 <tr>
    <td>
      <table>
        <tr>
          <td>nroLeg (PK)</td>
          <td>nombre</td>
          <td>codProv (FK)</td>
        </tr>
        <tr>
          <td>1</td>
          <td>Juan Cuiule</td>
          <td>1</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Pedro Perez</td>
          <td>1</td>
        </tr>
        <tr>
          <td>3</td>
          <td>Maria Gonzales</td>
          <td>2</td>
        </tr>
      </table>
    </td>
    <td>
      <table>
        <tr>
          <td>provId (PK)</td>
          <td>provDesc</td>
        </tr>
        <tr>
          <td>1</td>
          <td>CABA</td>
        </tr>
        <tr>
          <td>2</td>
          <td>Buenos Aires</td>
        </tr>
        <tr>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <td>3</td>
          <td>T. Del Fuego</td>
        </tr>
      </table>
    </td>
 </tr>
</table>

### Independencia de Datos

* Independencia lógica: las aplicaciones no fallan cuando agrego o quito campos
* Independencia física: distribución, indices

### Normalización

#### 1° Forma Normal

Busca evitar elementos multivaluados o atributos repetitivos. Se reemplazaria la primer tabla por la una estructura similar a la segunda donde tenemos dos tablas (`Facturas` y `Items`) relacionadas por medio de una FK.

<table>
  <tr>
    <td>nroFactura (PK)</td>
    <td>fecha</td>
    <td>codProd1</td>
    <td>precio1</td>
    <td>codProd2</td>
    <td>precio2</td>
    <td>codProd...</td>
    <td>precio...</td>
    <td>codProd20</td>
    <td>precio20</td>
  </tr>
  <tr>
    <td>1</td>
    <td>11-12-2018</td>
    <td>2</td>
    <td>10</td>
    <td>3</td>
    <td>30</td>
    <td>...</td>
    <td>...</td>
    <td>null</td>
    <td>null</td>
  </tr>
</table>

<table border="0">
 <tr>
    <td><b style="font-size:30px">Facturas</b></td>
    <td><b style="font-size:30px">Items</b></td>
 </tr>
 <tr>
    <td>
      <table>
        <tr>
          <td>nroFactura (PK)</td>
          <td>fecha</td>
        </tr>
        <tr>
          <td>1</td>
          <td>11-12-2018</td>
        </tr>
      </table>
    </td>
    <td>
      <table>
        <tr>
          <td>nroFactura (PK)</td>
          <td>codProd (PK)</td>
          <td>precio</td>
        </tr>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>10</td>
        </tr>
          <td>1</td>
          <td>3</td>
          <td>30</td>
        </tr>
      </table>
    </td>
 </tr>
</table>

#### 2° Forma Normal

Busca que todos los atributos de la tabla dependan solo de la clave primaria (en caso que sea una PK compuesta tiene que depender de ambas claves). En este caso solo los campos **precio** y **cantidad** depénden a la vez del `nroFactura` y el `codProd`. Por eso es conveniente tener otra tabla para todo lo relacionado al producto en sí.

<table>
  <tr>
    <td>nroFactura (PK)</td>
    <td>codProd (PK)</td>
    <td>precio</td>
    <td>cantidad</td>
    <td>descrProd</td>
    <td>codFamilia</td>
    <td>descrFamilia</td>
    <td>codSubFamilia</td>
    <td>descrSubFamilia</td>
  </tr>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>999</td>
    <td>1</td>
    <td>iPhone X - 256GB</td>
    <td>1</td>
    <td>Tecnología</td>
    <td>1</td>
    <td>Smartphones</td>
  </tr>
</table>

<table border="0">
 <tr>
    <td><b style="font-size:30px">Items</b></td>
    <td><b style="font-size:30px">Productos</b></td>
 </tr>
 <tr>
    <td>
      <table>
        <tr>
          <td>nroFactura (PK)</td>
          <td>codProd (PK) (FK)</td>
          <td>precio</td>
          <td>cantidad</td>
        </tr>
        <tr>
          <td>1</td>
          <td>2</td>
          <td>999</td>
          <td>1</td>
        </tr>
      </table>
    </td>
    <td>
      <table>
        <tr>
          <td>codProd (PK)</td>
          <td>descrProd</td>
          <td>codFamilia</td>
          <td>descrFamilia</td>
          <td>codSubFamilia</td>
          <td>descrSubFamilia</td>
        </tr>
        <tr>
          <td>2</td>
          <td>iPhone X - 256GB</td>
          <td>1</td>
          <td>Tecnología</td>
          <td>1</td>
          <td>Smartphones</td>
        </tr>
      </table>
    </td>
 </tr>
</table>

#### 3° Forma Normal

Busca evitar que haya atributos (que no son PKs) que tengan una dependencia entre sí. En el ejemplo anterior esto se ve en la tabla de productos donde `descrFamilia` depende de `codFamilia` y a la vez tanto `codSubFamilia` como `descrSubFamilia` dependen de `codFamilia`.

### SQL

Se compone de dos grandes "lenguajes":
* Data Definition Language:
  * `CREATE`
  * `ALTER`
  * `DROP`
* Data Management Language


##### DDL - Data Definition Language
```sql
create database PracticaPacial;
use PracticaParcial;
create table Facturas (
  nroFactura INTEGER PRIMARY KEY,
  fecha DATE NOT NULL
)
```

###### Constrains

* `PK`
* `FK`
* `NOT NULL` / `NULL` (default)
* `UNIQUE`
* `CHECK`
* `DEFAULT`

###### Data types

* Numericos
  * Enteros
    * `BIGINT`
    * `INTEGER`
    * `SMALLINT`
    * `TINYINT`
  * Decimales
    * `NUMERIC(P, S)`
    * `DECIMAL(P, S)`
* Alfanumericos
  * Longitud Fija `char(n)`
  * Longitud Variable `varchar(n)`
* Fecha `DATE`

```sql
create table Facturas (
  nroFactura BIGINT PRIMARY KEY,
  fechaEmision DATE NOT NULL,
  fechaVencimiento DATE NOT NULL,
  codClienter INT NOT NULL REFERENCES Clientes,
  CHECK (fechaVencimiento >= fechaEmision)
)

create table Clientes (
  codCliente INT PRIMARY KEY,
  razonSocial varchar(100) NOT NULL
)

create table Items (
  nroFactura BIGINT,
  codProducto INT
  cantidad DECIMAL(10,3) NOT NULL,
  precio DECIMAL(10,2) CHECK(precio > 0),
  PRIMARY KEY (nroFactura, codProducto)
)
```

##### DML - Data Management Language

* `SELECT`
* `INSERT`
* `UPDATE`
* `DELETE`
* `MERGE`

###### Select

```sql
SELECT *, lista_de_columnas, col1, col2, col3, col4 as alias, distinct(codProd)
# distinct => lo que se se repite lo pone una sola vez
FROM nombre_de_tabla
WHERE condiciones # >, <, >=, <=, =, !=, and, or, not, between, in, like
ORDER BY columna, posicionColumna ASC/DESC
GROUP BY campo # count(*), sum, avg, max, min # funciones agregadas que van en el select
HAVING condicion # similar al where pero actua sobre lo agregado
```

###### Insert

```sql
INSERT INTO tabla (col1, col2, col3)
VALUES (val1, val2, val3), (val1, val2, val3);

INSERT INTO tabla
SELECT col1, col2
FROM otra_tabla
# Todo lo del select va a "tabla"
```

###### Delete

```sql
DELETE FROM tabla # esta línea sola borra TODO
WHERE condicion
```

###### Update

```sql
UPDATE tabla
SET col1 = val1
    col2 = val2
    ...
    ...
WHERE condicion
```

### Tablas Temporales

Son tablas de sesión, es decir, se borran cuando se destruye la sesión.
Sirven para dividir selects de grandes joins.

Su creación, al igual que las tablas comunes, puede ser explicita o implicita.

###### Explicita
```sql
create table #tabla_temporal (
  id BIGINT PRIMARY KEY,
  campo INT
);
insert into #tabla_temporal
select * from tabla where condicion;
```

###### Implicita
```sql
select *
into #tabla_temporal
from tabla
where condicion;
```

### Transacciones

Una transaccion es un conjunto de instrucciones que se tienen que llevar de manera **atomica**, es decrir, se realizan todas o ninguna, si falla alguna de ellas se revierte lo hecho por todas las anteriores y no se efectuan las siguientes.

```sql
# Estado consistente
BEGIN TRANSACTION
  ...
  ...
  ...
  COMMIT TRANSACTION # => nuevo estado, todo salio bien
  ROLLBACK TRANSACTION # => vuelve al estado anterior, se deshacen los cambios
```

Lo importante es que por cualquiera de los dos caminos se llega a un estado que sigue siendo consistente.

Tienen que seguir el concepto `ACID`:
* Atomicidad
* Consistencia (va más allá de la integridad, tiene que ver con el negocio)
* Isolation - Aislamiento (las transacciones corren en paralelo)
* Durabilidad

Se maneja por medio de logs transaccionales que guardan lo que se hizo.