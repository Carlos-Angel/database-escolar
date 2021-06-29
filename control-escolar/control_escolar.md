# Esquema: control escolar

En este archivo se describen las tablas y sus átributos definidos en el esquema de control escolar.

- [Esquema: control escolar](#esquema-control-escolar)
  - [Atributos obligatorios](#atributos-obligatorios)
- [Tablas](#tablas)
  - [niveles_educativos](#niveles_educativos)
  - [planes_educativos](#planes_educativos)
  - [versiones_planes_educativos](#versiones_planes_educativos)
  - [tipos_materias](#tipos_materias)
  - [materias](#materias)
  - [escuelas](#escuelas)
  - [ofertas_educativas](#ofertas_educativas)
  - [tipos_periodos_escolares](#tipos_periodos_escolares)
  - [periodos_escolares](#periodos_escolares)
  - [alumnos](#alumnos)
  - [tutores](#tutores)
  - [documentos](#documentos)
  - [documentos_alumnos](#documentos_alumnos)
  - [grupos_escolares](#grupos_escolares)
  - [grupos_escolares_materias](#grupos_escolares_materias)
  - [alumnos_inscritos](#alumnos_inscritos)
  - [tipos_actas_calificaciones](#tipos_actas_calificaciones)
  - [actas_calificaciones](#actas_calificaciones)
  - [alumnos_calificaciones](#alumnos_calificaciones)

## Atributos obligatorios

Nota: Todas las tablas de este esquema deben contar con los siguientes 3 atributos.

| tipo      |  atributo   | Nulo | descripción                                                                              |
| :-------- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| bool      |   activo    |  No  | campo para confirmar si la fila ha sido eliminada.(false) de no ser el caso sera (true). |
| timestamp | created_att |  No  | campo que indica cuando fue creada la fila                                               |
| timestamp | updated_att |  No  | campo que indica cuando fue la ultima actualización de algún atributo de la tabla        |

# Tablas

### niveles_educativos

En esta tabla se definen los niveles de estudio de los programas. Ejemplo: Primaria, Secundaria, Preparatoria, Licenciatura, etc.

| tipo        |    atributo     | Nulo | descripción                |
| :---------- | :-------------: | :--: | :------------------------- |
| int         |       id        |  No  | Clave primaria             |
| varchar(50) | nivel_educativo |  No  | Nombre del nivel educativo |

### planes_educativos

En esta tabla se definen los planes de educación, un ejemplo de ellos son: Licenciatura en Ingeniería, Licenciatura en Médicina, Bachillerato Técnico en enfermería, etc. (En el caso de los niveles educativos inferiores o igual al nivel de secundaria, estos definen el nombre del plan educativo con el mismo nombre del nivel educativo).

| tipo         |      atributo      | Nulo | descripción                                                                             |
| :----------- | :----------------: | :--: | :-------------------------------------------------------------------------------------- |
| int          |         id         |  No  | Clave primaria                                                                          |
| int          | nivel_educativo_id |  No  | Clave foránea que hace referencia a la tabla niveles_educativos                         |
| varchar(200) |   nombre_oficial   |  No  | Nombre que se imprimirá en documentos oficiales                                         |
| varchar(50)  |    abreviatura     |  No  | Campo que se visualizara en sistema. Evitando los nombres oficiales que son mas largos. |

### versiones_planes_educativos

En esta tabla se definen las diferentes versiones de un plan de estudio. La versión define los criterios para terminar un plan educativo, así como el orden de materias que se imparten. ya que este suele ser actualizado. Evitando crear un nuevo plan educativo cada que los criterios o el orden de materias se actualice.
Esta tabla solo guarda los datos de criterio de termino de un plan educativo, el orden de las materias se define en la tabla Materias (se vera mas adelante).

| tipo        |          atributo           | Nulo | descripción                                                                                                                                                                     |
| :---------- | :-------------------------: | :--: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| int         |             id              |  No  | Clave primaria                                                                                                                                                                  |
| int         |      plan_educativo_id      |  No  | Clave foránea que hace referencia a la tabla planes_educativos                                                                                                                  |
| varchar(20) |            clave            |  No  | Clave oficial de la versión del plan educativo                                                                                                                                  |
| int(4)      |            anio             |  No  | año en el que se autorizo la versión del plan educativo                                                                                                                         |
| int(2)      |       numero_periodos       |  No  | Numero de periodos para concluir el plan educativo                                                                                                                              |
| int(3)      | minimo_creditos_aprobatorio |  No  | numero mínimo de créditos aprobatorios para concluir el plan educativo                                                                                                          |
| bool        |       version_actual        |  No  | campo que indica que versión esta en uso, solo una versión de cada plan educativo puede esta como (true). En caso de tener mas versiones, estas tendrán este campo como (false) |

### tipos_materias

En esta tabla se definen los diferentes tipos de materias que pueden haber.
Ejemplo:

- Obligatoria: Materia que es indispensable dentro del plan educativo.
- Optativa: Materia opcional que no depende del plan educativo.
- Acreditada: Materia que no cuenta con una calificación minima para ser aprobada.

| tipo        |   atributo   | Nulo | descripción                |
| :---------- | :----------: | :--: | :------------------------- |
| int         |      id      |  No  | Clave primaria             |
| varchar(50) | tipo_materia |  No  | Nombre del tipo de materia |

### materias

En esta tabla se definen las materias que serán impartidas dentro de un plan educativo

| tipo         |            atributo             | Nulo | descripción                                                                |
| :----------- | :-----------------------------: | :--: | :------------------------------------------------------------------------- |
| int          |               id                |  No  | Clave primaria                                                             |
| int          |       materia_anterior_id       |  Si  | Clave foránea que hace referencia a la misma tabla **materias**            |
| int          |         tipo_materia_id         |  No  | Clave foránea que hace referencia a la tabla **tipos_materias**            |
| int          |     version_plan_estudio_id     |  No  | Clave foránea que hace referencia a la tabla **versiones_planes_estudios** |
| char(8)      |              clave              |  No  | Clave que sera impresa en los documentos oficiales.                        |
| varchar(200) |             materia             |  No  | Nombre de la materia                                                       |
| int(3)       |            creditos             |  No  | numero de créditos que vale la materia                                     |
| int(2)       | calificacion_minima_aprobatoria |  No  | Valor mínimo de la calificación para aprobar la materia                    |

### escuelas

En esta tabla se definen los datos de la institución educativa, Se define en caso de que se administre más de una institución.

| tipo         |      atributo       | Nulo | descripción                                                                 |
| :----------- | :-----------------: | :--: | :-------------------------------------------------------------------------- |
| int          |         id          |  No  | Clave primaria                                                              |
| varchar(20)  |        clave        |  No  | Clave que otorga la secretaría de educación a la institución                |
| varchar(200) |   nombre_oficial    |  No  | Nombre de la institución el cuál saldrá impreso en los documentos oficiales |
| varchar(10)  |       siglas        |  No  | abreviatura del nombre de la institución                                    |
| varchar(15)  | telefono_particular |  No  | teléfono oficial de la institución                                          |
| varchar(255) |      direccion      |  No  | Dirección de la institución                                                 |
| varchar(100) |        logo         |  No  | ubicación del archivo logo de la institución                                |

### ofertas_educativas

En esta tabla se definen los planes educativos que están impartidos dentro de una institución.

| tipo |     atributo      | Nulo | descripción                                                        |
| :--- | :---------------: | :--: | :----------------------------------------------------------------- |
| int  |        id         |  No  | Clave primaria                                                     |
| int  |    escuela_id     |  No  | Clave foránea que hace referencia a la misma tabla **escuelas**    |
| int  | plan_educativo_id |  No  | Clave foránea que hace referencia a la tabla **planes_educativos** |

### tipos_periodos_escolares

En esta tabla se definen los tipos de periodos para cursar un plan educativo.
Ejemplo: periodo semanal, cuatrimestral, semestral, anual, etc.

| tipo        |        atributo        | Nulo | descripción                          |
| :---------- | :--------------------: | :--: | :----------------------------------- |
| int         |           id           |  No  | Clave primaria                       |
| varchar(50) | tipo_periodo_educativo |  No  | Nombre del tipo de periodo educativo |

### periodos_escolares

En esta tabla se definen los datos del periodo escolar que se abre en la escuela cade vez que inicia uno nuevo.

| tipo |        atributo         | Nulo | descripción                                                                     |
| :--- | :---------------------: | :--: | :------------------------------------------------------------------------------ |
| int  |           id            |  No  | Clave primaria                                                                  |
| int  | tipo_periodo_escolar_id |  No  | Clave foránea que hace referencia a la misma tabla **tipos_periodos_escolares** |
| int  |       escuela_id        |  No  | Clave foránea que hace referencia a la misma tabla **escuelas**                 |
| date |      fecha_inicio       |  No  | Fecha de inicio del periodo escolar                                             |
| date |      fecha_termino      |  No  | Fecha de termino del periodo escolar                                            |

### alumnos

En esta tabla se definen los datos del alumno que se registra en un plan educativo que este en oferta por parte de la escuela.

| tipo                         |         atributo          | Nulo | descripción                                                                        |
| :--------------------------- | :-----------------------: | :--: | :--------------------------------------------------------------------------------- |
| int                          |            id             |  No  | Clave primaria                                                                     |
| int                          |        escuela_id         |  No  | Clave foránea que hace referencia a la misma tabla **escuelas**                    |
| int                          | version_plan_educativo_id |  No  | Clave foránea que hace referencia a la misma tabla **versiones_planes_educativos** |
| varchar(20)                  |         matricula         |  No  | matricula escolar proporcionada por la institución                                 |
| varchar(200)                 |          nombre           |  No  | nombre(s) del alumno                                                               |
| varchar(200)                 |         apellidos         |  No  | apellidos paterno y materno                                                        |
| varchar(100)                 |        fotografia         |  No  | nombre del archivo de la fotografía, solo extension (jpg, png)                     |
| date                         |     fecha_nacimiento      |  No  | fecha de nacimiento                                                                |
| enum('Masculino','Femenino') |           sexo            |  No  | sexo del alumno                                                                    |
| varchar(255)                 |         direccion         |  No  | dirección de contacto                                                              |
| varchar(15)                  |    telefono_particular    |  No  | numero telefónico de contacto                                                      |
| varchar(200)                 |    correo_electronico     |  No  | Fecha de termino del periodo escolar                                               |

### tutores

En esta tabla se definen los datos del tutor o responsable del alumno si este es menor de edad.

| tipo         |      atributo       | Nulo | descripción                                            |
| :----------- | :-----------------: | :--: | :----------------------------------------------------- |
| int          |         id          |  No  | Clave primaria                                         |
| int          |      alumno_id      |  No  | Clave foránea que hace referencia la tabla **alumnos** |
| varchar(200) |       nombre        |  No  | nombre del tutor o responsable                         |
| varchar(200) |      apellidos      |  No  | apellidos del tutor o responsable                      |
| varchar(15)  | telefono_particular |  No  | teléfono de contacto                                   |
| varchar(200) | correo_electronico  |  Si  | correo electrónico                                     |

### documentos

En esta tabla se definen los tipos de documentos que el alumno debe entregar durante el proceso de de inscripción.
Ejemplo: acta de nacimiento, curp, comprobante de domicilio, etc.

| tipo         | atributo | Nulo | descripción          |
| :----------- | :------: | :--: | :------------------- |
| int          |    id    |  No  | Clave primaria       |
| varchar(100) |  nombre  |  No  | nombre del documento |

### documentos_alumnos

En esta tabla se definen los documentos que el alumno ha entregado durante el proceso de inscripción.

| tipo             |     atributo      | Nulo | descripción                                                   |
| :--------------- | :---------------: | :--: | :------------------------------------------------------------ |
| int              |        id         |  No  | Clave primaria                                                |
| int              |   documento_id    |  No  | Clave foránea que hace referencia la tabla **documentos**     |
| bool             |     original      |  No  | indica si el alumno entrego el documento original o una copia |
| date             |   fecha_entrega   |  Si  | fecha que indica cuando se regreso el documento original      |
| varchar(50)      | documento_digital |  Si  | nombre del documento digital (pdf)                            |
| enum(malo,bueno) | estado_documento  |  No  | indica el estado físico del documento entregado               |
| bool             |     prestamo      |  Si  | indica si el alumno solicito un préstamo de su documento      |
| date             |  fecha_prestamo   |  Si  | fecha en que se entrego el documento.                         |
| date             |  fecha_devuelto   |  Si  | fecha en que se devolvió el documento.                        |

### grupos_escolares

En esta tabla se definen los grupos que se abren en un nuevo periodo escolar.

| tipo                       |         atributo          | Nulo | descripción                                                                |
| :------------------------- | :-----------------------: | :--: | :------------------------------------------------------------------------- |
| int                        |            id             |  No  | Clave primaria                                                             |
| int                        |    periodo_escolar_id     |  No  | Clave foránea que hace referencia la tabla **periodos_escolares**          |
| int                        | version_plan_educativo_id |  No  | Clave foránea que hace referencia la tabla **versiones_planes_educativos** |
| enum(matutino, vespertino) |           turno           |  No  | turno de horario del grupo                                                 |
| varchar(10)                |          nombre           |  Si  | nombre del grupo oficial                                                   |
| int                        |           nivel           |  Si  | nivel del periodo escolar del grupo(Ejemplo, 1 semestre, 2 semestre, etc.) |

### grupos_escolares_materias

En esta tabla se definen los grupos que se abren en un nuevo periodo escolar.

| tipo |     atributo     | Nulo | descripción                                                     |
| :--- | :--------------: | :--: | :-------------------------------------------------------------- |
| int  |        id        |  No  | Clave primaria                                                  |
| int  | grupo_escolar_id |  No  | Clave foránea que hace referencia la tabla **grupos_escolares** |
| int  |    materia_id    |  No  | Clave foránea que hace referencia la tabla **materias**         |

### alumnos_inscritos

En esta tabla se definen los alumnos que se inscriben a un nuevo periodo escolar.

| tipo |     atributo     | Nulo | descripción                                                     |
| :--- | :--------------: | :--: | :-------------------------------------------------------------- |
| int  |        id        |  No  | Clave primaria                                                  |
| int  |    alumno_id     |  No  | Clave foránea que hace referencia la tabla **alumnos**          |
| int  | grupo_escolar_id |  No  | Clave foránea que hace referencia la tabla **grupo_escolar_id** |

### tipos_actas_calificaciones

En esta tabla se definen los diferentes tipos de actas para las calificaciones. El tipo de acta se refiere a los diferentes tipos de calificaciones oficiales que se registran durante un periodo escolar.

**Casos de ejemplo:**

- En un periodo semestral de 6 meses, la escuela debe aplicar 3 exámenes parciales los cuales definen la calificación final de la materia.
- Mientras que un periodo cuatrimestral de 4 meses solo se aplican 2 exámenes parciales.
- También puede aplicarse un solo examen parcial final que representa el total de la calificación sin la necesidad de registrar las calificaciones de los parciales.
- A su vez, también existen actas de exámenes extraordinarios de primer, segundo y tercer intento para los casos de alumnos que reprobaron una materia.

**tipos de actas de calificaciones:**

- Primer parcial (examen aplicativo a los 2 meses)
- Segundo parcial (examen aplicativo a los 4 meses)
- Tercer parcial (examen aplicativo a los 6 meses)
- Parcial final (un solo examen durante todo el periodo escolar)
- Extraordinario primer intento (primer intento del alumno para aprobar una materia reprobada)
- Extraordinario segundo intento (segundo intento del alumno para aprobar una materia reprobada)
- Extraordinario tercer intento (tercer intento del alumno para aprobar una materia reprobada)

| tipo        | atributo | Nulo | descripción                     |
| :---------- | :------: | :--: | :------------------------------ |
| int         |    id    |  No  | Clave primaria                  |
| varchar(30) |  nombre  |  No  | nombre del tipo de calificación |

### actas_calificaciones

En esta tabla se definen los datos de un acta de calificación que hace referencia al tipo de calificaciones que se guardan dentro de ellas.

**Nota:** El campo grupo_escolar_materia_id solo sera nullo en caso de actas extraordinarias. ya que en esos casos, el alumno puede hacer
el examen sin estar registrado en un periodo escolar activo o en un grupo.

| tipo |         atributo          | Nulo | descripción                                                                |
| :--- | :-----------------------: | :--: | :------------------------------------------------------------------------- |
| int  |            id             |  No  | Clave primaria                                                             |
| int  | tipo_acta_calificacion_id |  No  | Clave foránea que hace referencia la tabla **tipos_actas_calificaciones**  |
| int  | grupo_escolar_materia_id  |  Si  | Clave foránea que hace referencia la tabla **grupos_escolares_materias**   |
| bool |          abierto          |  No  | campo que define si el acta esta disponible para captura de calificaciones |

### alumnos_calificaciones

En esta tabla se definen las calificaciones de los alumnos.

| tipo        |       atributo       | Nulo | descripción                                                         |
| :---------- | :------------------: | :--: | :------------------------------------------------------------------ |
| int         |          id          |  No  | Clave primaria                                                      |
| int         | acta_calificacion_id |  No  | Clave foránea que hace referencia la tabla **actas_calificaciones** |
| int         |  alumno_materia_id   |  No  | Clave foránea que hace referencia la tabla **alumnos_materias**     |
| varchar(10) |     calificacion     |  No  | calificación optenida del alumno de una materia                     |
