# Esquema: exámenes

En este archivo se describen las tablas y sus átributos definidos en el esquema de exámenes.

- [Esquema: exámenes](#esquema-exámenes)
  - [Atributos obligatorios](#atributos-obligatorios)
- [Tablas](#tablas)
  - [tipos_preguntas](#tipos_preguntas)
  - [preguntas](#preguntas)
  - [respuestas](#respuestas)
  - [examenes](#examenes)
  - [examenes_preguntas](#examenes_preguntas)
  - [programaciones_examenes](#programaciones_examenes)
  - [tiempos_extras](#tiempos_extras)

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

### examenes

En esta tabla se definen los datos del un examen.

atributo: estado, tipo: enum('captura','revisión','autorizado','eliminado')

| tipo         |          atributo           | Nulo | descripción                                                                                                  |
| :----------- | :-------------------------: | :--: | :----------------------------------------------------------------------------------------------------------- |
| int          |             id              |  No  | Clave primaria                                                                                               |
| int          |  tipo_acta_calificacion_id  |  No  | Clave foránea que hace referencia a la tabla **tipos_actas_calificaciones** del esquema **control_escolar**  |
| int          |         materia_id          |  Si  | Clave foránea que hace referencia a la tabla **materias** del esquema **control_escolar**                    |
| int          |     periodo_escolar_id      |  No  | Clave foránea que hace referencia a la tabla **periodos_escolares** del esquema **control_escolar**          |
| int          |     oferta_educativa_id     |  No  | Clave foránea que hace referencia a la tabla **ofertas_educativas** del esquema **control_escolar**          |
| varchar(255) |           nombre            |  Si  | Nombre del examen, este campo es requerido si el examen es especial y no es una materia de un plan educativo |
| text         |        instrucciones        |  No  | tipo de pregunta                                                                                             |
| int          | numero_preguntas_asignables |  No  | numero de preguntas que se asignaran al generar los examenes de los alumnos                                  |
| int          |         porcentaje          |  No  | porcentaje de calificación del examen                                                                        |
| enum         |           estado            |  No  | estados o etapas de captura del examen                                                                       |

### examenes_preguntas

En esta tabla se definen los respuestas asociadas a una pregunta.

| tipo |  atributo   | Nulo | descripción                                                |
| :--- | :---------: | :--: | :--------------------------------------------------------- |
| int  |     id      |  No  | Clave primaria                                             |
| int  |  examen_id  |  No  | Clave foránea que hace referencia a la tabla **examenes**  |
| int  | pregunta_id |  No  | Clave foránea que hace referencia a la tabla **preguntas** |
| bool |  cancelada  |  Si  | cancelar una pregunta del examen                           |

### programaciones_examenes

En esta tabla se definen las fechas de programación para aplicar un examen a los alumnos.
Los campos grupo escolar_id y alumno_id puedes ser nulos en los casos de que la programación sea para un grupo o para un solo alumno.

| tipo |      atributo      | Nulo | descripción                                                                                   |
| :--- | :----------------: | :--: | :-------------------------------------------------------------------------------------------- |
| int  |         id         |  No  | Clave primaria                                                                                |
| int  |     examen_id      |  No  | Clave foránea que hace referencia a la tabla **examenes**                                     |
| int  |  grupo_escolar_id  |  Si  | Clave foránea que hace referencia a la tabla **grupos_escolares** del esquema control_escolar |
| int  |     alumno_id      |  Si  | Clave foránea que hace referencia a la tabla **alumnos** del esquema control_escolar          |
| date | fecha_programacion |  No  | fecha en que se aplicara el examen                                                            |
| time |    hora_inicio     |  No  | hora de inicio del examen                                                                     |
| time |     hora_final     |  No  | hora final del examen                                                                         |

### tiempos_extras

En esta tabla se definen los tiempos extras (solo en caso de ser necesario) para aumentar el tiempo de programación de un examen.

| tipo |        atributo        | Nulo | descripción                                                              |
| :--- | :--------------------: | :--: | :----------------------------------------------------------------------- |
| int  |           id           |  No  | Clave primaria                                                           |
| int  | programacion_examen_id |  No  | Clave foránea que hace referencia a la tabla **programaciones_examenes** |
| time |      tiempo_extra      |  Si  | tiempo extra de una programación                                         |
