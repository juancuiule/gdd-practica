# Gestión de Datos

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
          <td>nroLeg</td>
          <td>nombre</td>
          <td>codProv</td>
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
          <td>provId</td>
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