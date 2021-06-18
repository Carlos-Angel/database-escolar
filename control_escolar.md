# Documentación del esquema: control escolar

En este archivo se describen las tablas y sus átributos definidos en el esquema de control escolar.

### **Atributos obligatorios**

Nota: Todas las tablas de este esquema deben contar con los siguientes 3 atributos.

| tipo      |  atributo   | Nulo | descripción                                                                              |
| :-------- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| bool      |   activo    |  No  | campo para confirmar si la fila ha sido eliminada.(false) de no ser el caso sera (true). |
| timestamp | created_att |  No  | campo que indica cuando fue creada la fila                                               |
| timestamp | updated_att |  No  | campo que indica cuando fue la ultima actualización de algún atributo de la tabla        |

### Tabla **niveles_educativos**

En esta tabla se definen los niveles de estudio de los programas. Ejemplo: Primaria, Secundaria, Preparatoria, Licenciatura, etc.

| tipo        |    atributo     | Nulo | descripción                |
| :---------- | :-------------: | :--: | :------------------------- |
| int         |       id        |  No  | Clave primaria             |
| varchar(50) | nivel_educativo |  No  | Nombre del nivel educativo |

### Tabla **planes_educativos**

En esta tabla se definen los planes de educación, un ejemplo de ellos son: Licenciatura en Ingeniería, Licenciatura en Médicina, Bachillerato Técnico en enfermería, etc. (En el caso de los niveles educativos inferiores o igual al nivel de secundaria, estos definen el nombre del plan educativo con el mismo nombre del nivel educativo).

| tipo         |      atributo      | Nulo | descripción                                                                             |
| :----------- | :----------------: | :--: | :-------------------------------------------------------------------------------------- |
| int          |         id         |  No  | Clave primaria                                                                          |
| int          | nivel_educativo_id |  No  | Clave foránea que hace referencia a la tabla niveles_educativos                         |
| varchar(200) |   nombre_oficial   |  No  | Nombre que se imprimirá en documentos oficiales                                         |
| varchar(50)  |    abreviatura     |  No  | Campo que se visualizara en sistema. Evitando los nombres oficiales que son mas largos. |

### Tabla **versiones_planes_educativos**

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

### Tabla **tipos_materias**

En esta tabla se definen los diferentes tipos de materias que pueden haber.
Ejemplo:

- Obligatoria: Materia que es indispensable dentro del plan educativo.
- Optativa: Materia opcional que no depende del plan educativo.
- Acreditada: Materia que no cuenta con una calificación minima para ser aprobada.

| tipo        |   atributo   | Nulo | descripción                |
| :---------- | :----------: | :--: | :------------------------- |
| int         |      id      |  No  | Clave primaria             |
| varchar(50) | tipo_materia |  No  | Nombre del tipo de materia |

### Tabla **materias**

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
