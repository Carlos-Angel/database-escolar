# Esquema: cursos

En este archivo se describen las tablas y sus átributos definidos en el esquema de cursos.

- [Esquema: cursos](#esquema-cursos)
  - [Atributos obligatorios](#atributos-obligatorios)
- [Tablas](#tablas)
    - [tipos_cursos](#tipos_cursos)
    - [cursos](#cursos)
    - [cursos_escuelas](#cursos_escuelas)
    - [talleres](#talleres)
    - [alumnos_inscritos_talleres](#alumnos_inscritos_talleres)
    - [talleres_horarios](#talleres_horarios)

## Atributos obligatorios

Nota: Todas las tablas de este esquema deben contar con los siguientes 3 atributos.

| tipo      |  atributo   | Nulo | descripción                                                                              |
| :-------- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| bool      |   activo    |  No  | campo para confirmar si la fila ha sido eliminada.(false) de no ser el caso sera (true). |
| timestamp | created_att |  No  | campo que indica cuando fue creada la fila                                               |
| timestamp | updated_att |  No  | campo que indica cuando fue la ultima actualización de algún atributo de la tabla        |

# Tablas

### tipos_cursos

En esta tabla se definen los diferentes tipos de cursos que imparte la institución

| tipo        | atributo | Nulo | descripción    |
| :---------- | :------: | :--: | :------------- |
| int         |    id    |  No  | Clave primaria |
| varchar(30) |  nombre  |  No  | tipo de curso  |

### cursos

En esta tabla se definen los datos de un curso.

| tipo         |            atributo             | Nulo | descripción                                                         |
| :----------- | :-----------------------------: | :--: | :------------------------------------------------------------------ |
| int          |               id                |  No  | Clave primaria                                                      |
| int          |          tipo_curso_id          |  No  | Clave foránea que hace referencia a la misma tabla **tipos_cursos** |
| text         |              clave              |  No  | clave oficial del curso que sera impreso en documentos              |
| varchar(150) |             nombre              |  Si  | nombre del curso                                                    |
| int          | calificacion_minima_aprobatoria |  Si  | calificación minima para aprobar el curso                           |
| int          |          numero_horas           |  Si  | numero de horas para completar el curso                             |
| int          |            creditos             |  Si  | cantidad de créditos que vale el curso                              |

### cursos_escuelas

En esta tabla se definen los cursos que se abrirán dentro de la escuela

| tipo |   atributo   | Nulo | descripción                                                                                 |
| :--- | :----------: | :--: | :------------------------------------------------------------------------------------------ |
| int  |      id      |  No  | Clave primaria                                                                              |
| int  |   curso_id   |  No  | Clave foránea que hace referencia a la misma tabla **cursos**                               |
| int  |  escuela_id  |  No  | Clave foránea que hace referencia a la misma tabla **escuelas** del esquema control_escolar |
| date | fecha_inicio |  No  | fecha de inicio del curso                                                                   |
| date | fecha_final  |  Si  | fecha final del curso                                                                       |

### talleres

En esta tabla se definen los talleres para que los alumnos puedan inscribirse.

| tipo                          |     atributo     | Nulo | descripción                                                            |
| :---------------------------- | :--------------: | :--: | :--------------------------------------------------------------------- |
| int                           |        id        |  No  | Clave primaria                                                         |
| int                           | curso_escuela_id |  No  | Clave foránea que hace referencia a la misma tabla **cursos_escuelas** |
| varchar(20)                   |      nombre      |  No  | nombre del taller                                                      |
| enum('MATUTINO','VESPERTINO') |      turno       |  No  | turno en que se imparte el taller                                      |

### alumnos_inscritos_talleres

En esta tabla se definen los cursos que se abrirán dentro de la escuela

| tipo        |   atributo   | Nulo | descripción                                                                                |
| :---------- | :----------: | :--: | :----------------------------------------------------------------------------------------- |
| int         |      id      |  No  | Clave primaria                                                                             |
| int         |  taller_id   |  No  | Clave foránea que hace referencia a la misma tabla **talleres**                            |
| int         |  alumno_id   |  No  | Clave foránea que hace referencia a la misma tabla **alumnos** del esquema control_escolar |
| varchar(10) | calificacion |  No  | calificación del alumno al finalizar el taller                                             |

### talleres_horarios

En esta tabla se definen los horarios en los que se impartirá el taller.

| tipo |  atributo   | Nulo | descripción                                                                              |
| :--- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| int  |     id      |  No  | Clave primaria                                                                           |
| int  |  taller_id  |  No  | Clave foránea que hace referencia a la misma tabla **talleres**                          |
| int  |   aula_id   |  No  | Clave foránea que hace referencia a la misma tabla **aulas** del esquema control_escolar |
| date | fecha_clase |  No  | fecha de inicio de la clase del taller                                                   |
| time | hora_inicio |  Si  | hora de inicio de la clase                                                               |
| time | hora_final  |  Si  | hora final de la clase                                                                   |
