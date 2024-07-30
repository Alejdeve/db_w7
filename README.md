# Lab Relaciones, constrains y funciones.

## En este ejercicio, realizarás diversas tareas para crear y manipular una base de datos. A continuación, se describen los pasos que debes seguir:

## Paso 1: Crear la Base de Datos
Crea una base de datos llamada empresa.

## Paso 2: Crear las Tablas y Relaciones
### Crea una tabla llamada departamentos con las siguientes columnas:

departamento_id: INT, clave primaria, auto-increment.
nombre: VARCHAR(100), no nulo.

### Crea una tabla llamada empleados con las siguientes columnas:

empleado_id: INT, clave primaria, auto-increment.
nombre: VARCHAR(100), no nulo.
salario: DECIMAL(10, 2), no nulo.
departamento_id: INT, clave foránea que referencia a departamento_id en departamentos.

### Crea una relación ManyToOne entre empleados y departamentos. Cada empleado debe pertenecer a un solo departamento, pero un departamento puede tener muchos empleados.

### Crea una tabla llamada proyectos con las siguientes columnas:

proyecto_id: INT, clave primaria, auto-increment.
nombre: VARCHAR(100), no nulo.

### Crea una tabla intermedia llamada empleado_proyecto para establecer una relación ManyToMany entre empleados y proyectos. Las columnas de esta tabla serán:

empleado_id: INT, clave foránea que referencia a empleado_id en empleados.
proyecto_id: INT, clave foránea que referencia a proyecto_id en proyectos.

### Crea una tabla llamada detalles_empleado con las siguientes columnas:

empleado_id: INT, clave primaria, clave foránea que referencia a empleado_id en empleados.
direccion: VARCHAR(255), no nulo.
telefono: VARCHAR(15), no nulo.

### Crea una relación OneToOne entre empleados y detalles_empleado. Cada empleado debe tener un conjunto único de detalles.

Crea datos falsos para rellenar las tablas.

## Paso 3: Crear una Función

### Crea una función llamada calcular_bonificacion que tome como parámetro el empleado_id y devuelva una bonificación calculada como el 10% del salario del empleado. Utiliza la cláusula DETERMINISTIC en la definición de la función.

## Paso 4: Usar un Delimiter
Asegúrate de utilizar un delimitador (DELIMITER) diferente al punto y coma (;) al definir la función en el paso anterior. Luego, restablece el delimitador a su valor original.

## Paso 5: Nombrar una Constrain y Usarla
Crea una restricción (CONSTRAINT) de integridad referencial con nombre fk_departamento_empleado en la relación ManyToOne entre empleados y departamentos. Luego, utiliza este nombre para eliminar la restricción.

## Paso 6: Declarar Variables y Usarlas
Dentro de la función calcular_bonificacion, declara las siguientes variables locales:

salario_base: DECIMAL(10,2)
bonificacion: DECIMAL(10,2)
Utiliza estas variables para almacenar el salario del empleado y calcular la bonificación.