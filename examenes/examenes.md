# Esquema: exámenes

En este archivo se describen las tablas y sus átributos definidos en el esquema de exámenes.

- [Esquema: exámenes](#esquema-exámenes)
  - [Atributos obligatorios](#atributos-obligatorios)
- [Tablas](#tablas)
    - [tipos_preguntas](#tipos_preguntas)
    - [preguntas](#preguntas)
    - [respuestas](#respuestas)

## Atributos obligatorios

Nota: Todas las tablas de este esquema deben contar con los siguientes 3 atributos.

| tipo      |  atributo   | Nulo | descripción                                                                              |
| :-------- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| bool      |   activo    |  No  | campo para confirmar si la fila ha sido eliminada.(false) de no ser el caso sera (true). |
| timestamp | created_att |  No  | campo que indica cuando fue creada la fila                                               |
| timestamp | updated_att |  No  | campo que indica cuando fue la ultima actualización de algún atributo de la tabla        |

# Tablas

### tipos_preguntas

En esta tabla se definen los diferentes tipos de preguntas que pueden hacerse, preguntas de opción multiple, preguntas de verdadero o falso, etc.

| tipo        | atributo | Nulo | descripción      |
| :---------- | :------: | :--: | :--------------- |
| int         |    id    |  No  | Clave primaria   |
| varchar(20) |  nombre  |  No  | tipo de pregunta |

### preguntas

En esta tabla se definen los datos de las preguntas.

| tipo         |     atributo     | Nulo | descripción                                                            |
| :----------- | :--------------: | :--: | :--------------------------------------------------------------------- |
| int          |        id        |  No  | Clave primaria                                                         |
| int          | tipo_pregunta_id |  No  | Clave foránea que hace referencia a la misma tabla **tipos_preguntas** |
| text         |   descripcion    |  No  | descripción de la pregunta                                             |
| varchar(100) |      imagen      |  Si  | ruta de la imagen para hacer mas descriptiva la pregunta               |

### respuestas

En esta tabla se definen los respuestas asociadas a una pregunta.

| tipo |  atributo   | Nulo | descripción                                                      |
| :--- | :---------: | :--: | :--------------------------------------------------------------- |
| int  |     id      |  No  | Clave primaria                                                   |
| int  | pregunta_id |  No  | Clave foránea que hace referencia a la misma tabla **preguntas** |
| text | descripcion |  Si  | descripción de la respuesta                                      |
| bool |  correcta   |  No  | indica que la respuesta es la correcta a la pregunta asociada    |
