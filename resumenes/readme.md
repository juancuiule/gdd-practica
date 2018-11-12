# Gestión de Datos

## Indice

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