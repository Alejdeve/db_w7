# Lab Relaciones, constrains y funciones.

## En este ejercicio, realizarás diversas tareas para crear y manipular una base de datos. A continuación, se describen los pasos que debes seguir:

## Paso 1: Crear la Base de Datos
Crea una base de datos llamada empresa.

## Solucion 1:
create database Empresa;<br>
use Empresa;

## Paso 2: Crear las Tablas y Relaciones
### Crea una tabla llamada departamentos con las siguientes columnas:

departamento_id: INT, clave primaria, auto-increment.<br>
nombre: VARCHAR(100), no nulo.<br>

### Solucion 2:
create table Departamentos (<br>
	departamento_id INT auto_increment primary key,<br>
	nombre VARCHAR(100) not NULL<br>
);

### Crea una tabla llamada empleados con las siguientes columnas:

empleado_id: INT, clave primaria, auto-increment.
nombre: VARCHAR(100), no nulo.
salario: DECIMAL(10, 2), no nulo.
departamento_id: INT, clave foránea que referencia a departamento_id en departamentos.

### Solucion 3:
create table Empleados (<br>
	empleado_id INT auto_increment primary key, <br>
	nombre VARCHAR(100) not null, <br>
	salario DECIMAL(10, 2) not null,<br>
	departamento_id INT, <br>
	constraint fk_departamento_empleado foreign key (departamento_id) <br>
	references Departamentos(departamento_id) <br>
);<br> 

### Crea una relación ManyToOne entre empleados y departamentos. Cada empleado debe pertenecer a un solo departamento, pero un departamento puede tener muchos empleados.

En el codigo creamos la relacion ManyToOne tal como se nos pide, exactamente en estas lineas de codigo:<br>
    constraint fk_departamento_empleado foreign key (departamento_id) <br>
	references Departamentos(departamento_id) <br>


### Crea una tabla llamada proyectos con las siguientes columnas:

proyecto_id: INT, clave primaria, auto-increment.
nombre: VARCHAR(100), no nulo.

### Solucion 4:
create table Proyectos (<br>
	id_proyecto INT auto_increment primary key,<br>
	nombre VARCHAR(100) not NULL<br>
);<br>

### Crea una tabla intermedia llamada empleado_proyecto para establecer una relación ManyToMany entre empleados y proyectos. Las columnas de esta tabla serán:

empleado_id: INT, clave foránea que referencia a empleado_id en empleados.
proyecto_id: INT, clave foránea que referencia a proyecto_id en proyectos.

### Solucion 5:
create table Empleado_proyecto (<br>
	empleado_id INT,<br>
	proyecto_id INT,<br>
	primary key (empleado_id, proyecto_id),<br>
	foreign key (empleado_id) references Empleados(empleado_id)<br>
	foreign key (proyecto_id) references Proyectos(proyecto_id)<br>
);<br>

### Crea una tabla llamada detalles_empleado con las siguientes columnas:

empleado_id: INT, clave primaria, clave foránea que referencia a empleado_id en empleados.
direccion: VARCHAR(255), no nulo.
telefono: VARCHAR(15), no nulo.

create table Detalles_empleado (<br>
	empleado_id INT primary key,<br>
	direccion VARCHAR(255) not null,<br>
	telefono VARCHAR(15) not null,<br>
	constraint fk_empleado_detalle foreign key (empleado_id)<br>
	references Empleados(emplado_id)<br>
);<br>

### Crea una relación OneToOne entre empleados y detalles_empleado. Cada empleado debe tener un conjunto único de detalles.

Estas lineas de codigo crean la relacion OneToOne entre Empleado y Detalle_empleados:

    constraint fk_empleado_detalle foreign key (empleado_id)<br>
	references Empleados(emplado_id)<br>

Crea datos falsos para rellenar las tablas.

## Paso 3: Crear una Función

### Crea una función llamada calcular_bonificacion que tome como parámetro el empleado_id y devuelva una bonificación calculada como el 10% del salario del empleado. Utiliza la cláusula DETERMINISTIC en la definición de la función.

    create function Calcular_bonificacion(empleado_id INT)
    returns DECIMAL(10, 2)
    deterministic
    begin
	    declare salario_base DECIMAL(10, 2);
	    declare bonificacion DECIMAL(10, 2);

	    select salario into salario_base
	    from Empleados
	    where empleado_id = empleado_id;

	    set bonificacion = salario_base * 0.10;

	    return bonificacion;
    end

## Paso 4: Usar un Delimiter
Asegúrate de utilizar un delimitador (DELIMITER) diferente al punto y coma (;) al definir la función en el paso anterior. Luego, restablece el delimitador a su valor original.<BR>

    DELIMITER $$

        create function Calcular_bonificacion(empleado_id INT)
        returns DECIMAL(10, 2)
        deterministic
        begin
            declare salario_base DECIMAL(10, 2);
            declare bonificacion DECIMAL(10, 2);

            select salario into salario_base
            from Empleados
            where empleado_id = empleado_id;

            set bonificacion = salario_base * 0.10;

            return bonificacion;
        end$$

    DELIMITER;

## Paso 5: Nombrar una Constrain y Usarla
Crea una restricción (CONSTRAINT) de integridad referencial con nombre fk_departamento_empleado (Creada en Solucion 3) en la relación ManyToOne entre empleados y departamentos. 

Luego, utiliza este nombre para eliminar la restricción.

ALTER TABLE empleados DROP FOREIGN KEY fk_departamento_empleado;


## Paso 6: Declarar Variables y Usarlas
Dentro de la función calcular_bonificacion, declara las siguientes variables locales:

salario_base: DECIMAL(10,2)
bonificacion: DECIMAL(10,2)
Utiliza estas variables para almacenar el salario del empleado y calcular la bonificación.

Este paso ya se implementó en la creación de la función calcular_bonificacion, donde se declararon y usaron las variables salario_base y bonificacion.

## Codigo completo:

    DROP DATABASE IF EXISTS Empresa;
    create database Empresa;

    use Empresa;

    create table Departamentos (
	departamento_id INT auto_increment primary key,
	nombre VARCHAR(100) not NULL
    );

    create table Empleados (
	empleado_id INT auto_increment primary key,
	nombre VARCHAR(100) not null,
	salario DECIMAL(10, 2) not null,
	departamento_id INT,
	constraint fk_departamento_empleado foreign key (departamento_id) references Departamentos(departamento_id)	
    );

    create table Proyectos (
	proyecto_id INT auto_increment primary key,
	nombre VARCHAR(100) not NULL
    );

    create table Empleado_proyecto (
	empleado_id INT,
	proyecto_id INT,
	primary key (empleado_id, proyecto_id),
	foreign key (empleado_id) references Empleados(empleado_id),
	foreign key (proyecto_id) references Proyectos(proyecto_id)
    );

    create table Detalles_empleado (
	empleado_id INT primary key,
	direccion VARCHAR(255) not null,
	telefono VARCHAR(15) not null,
	constraint fk_empleado_detalle foreign key (empleado_id) REFERENCES Empleados(empleado_id)
    );

    INSERT INTO departamentos (nombre) VALUES ('Recursos Humanos'), ('Tecnología'), ('Marketing'), ('Ventas');

    INSERT INTO empleados (nombre, salario, departamento_id) VALUES
    ('Felipe Abril', 2000.00, 1),
    ('Juan Marquez', 2500.00, 2),
    ('Carlos Buitrago', 1500.00, 3),
    ('Juana Rojas', 3000.00, 4);

    INSERT INTO proyectos (nombre) VALUES ('Proyecto A'), ('Proyecto B'), ('Proyecto C');

    INSERT INTO empleado_proyecto (empleado_id, proyecto_id) VALUES
    (1, 1), (2, 1), (3, 2), (4, 3);

    INSERT INTO detalles_empleado (empleado_id, direccion, telefono) VALUES
    (1, '456 Calle Fake', '1234567890'),
    (2, '456 Avenida Mayor', '0987654321'),
    (3, '789 Boulevard Niza', '1122334455'),
    (4, '321 Plaza Menor', '6677889900');

    DELIMITER $$

    create function Calcular_bonificacion(empleado_id INT) RETURNS DECIMAL(10, 2)
    DETERMINISTIC
    BEGIN
	declare salario_base DECIMAL(10, 2);
	declare bonificacion DECIMAL(10, 2);

	select salario into salario_base
	from Empleados
	where empleado_id = empleado_id;

	set bonificacion = salario_base * 0.10;

	return bonificacion;
    END$$

    DELIMITER ;