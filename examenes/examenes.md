# Esquema: exámenes

En este archivo se describen las tablas y sus átributos definidos en el esquema de exámenes.

- [Esquema: exámenes](#esquema-exámenes)
  - [Atributos obligatorios](#atributos-obligatorios)
- [Tablas](#tablas)

## Atributos obligatorios

Nota: Todas las tablas de este esquema deben contar con los siguientes 3 atributos.

| tipo      |  atributo   | Nulo | descripción                                                                              |
| :-------- | :---------: | :--: | :--------------------------------------------------------------------------------------- |
| bool      |   activo    |  No  | campo para confirmar si la fila ha sido eliminada.(false) de no ser el caso sera (true). |
| timestamp | created_att |  No  | campo que indica cuando fue creada la fila                                               |
| timestamp | updated_att |  No  | campo que indica cuando fue la ultima actualización de algún atributo de la tabla        |

# Tablas
