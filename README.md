# Quiz Universidad

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnosAsc //
    CREATE PROCEDURE alumnosAsc()
    BEGIN
        SELECT nombre, apellido1, apellido2 FROM persona
        WHERE tipo = "alumno"
        ORDER BY nombre ASC, apellido1 ASC, apellido2 ASC;
    END //
    DELIMITER ;

    call alumnosAsc();
    ```

2. Averigua el nombre y los dos apellidos de los alumnos que **no** han dado de alta su número de teléfono en la base de datos.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnosSinTelefono //
    CREATE PROCEDURE alumnosSinTelefono()
    BEGIN
        SELECT nombre, apellido1, apellido2 FROM persona
        WHERE tipo = "alumno" AND telefono IS NULL;
    END //
    DELIMITER ;

    call alumnosSinTelefono();
    ```

3. Devuelve el listado de los alumnos que nacieron en `1999`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnos1999 //
    CREATE PROCEDURE alumnos1999()
    BEGIN
        SELECT * FROM persona WHERE tipo = "alumno" AND YEAR(fecha_nacimiento) = 1999;
    END //
    DELIMITER ;

    call alumnos1999();
    ```

4. Devuelve el listado de `profesores` que **no** han dado de alta su número de teléfono en la base de datos y además su nif termina en `K`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesoresSinTelefono //
    CREATE PROCEDURE profesoresSinTelefono()
    BEGIN
        SELECT * FROM persona WHERE tipo = "profesor" AND telefono IS NULL AND nif LIKE '%k';
    END //
    DELIMITER ;

    call profesoresSinTelefono();
    ```

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador `7`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS asigC1C3G7 //
    CREATE PROCEDURE asigC1C3G7()
    BEGIN
        SELECT * FROM asignatura WHERE cuatrimestre = 1 AND curso = 3 AND id_grado = 7;
    END //
    DELIMITER ;

    call asigC1C3G7();
    ```

6. Devuelve un listado con los datos de todas las **alumnas** que se han matriculado alguna vez en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnasGII2015 //
    CREATE PROCEDURE alumnasGII2015()
    BEGIN
        SELECT DISTINCT p.* FROM persona p
        JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
        JOIN asignatura a ON am.id_asignatura = a.id
        JOIN grado g ON a.id_grado = g.id
        WHERE p.sexo = "M"
        AND p.tipo = "alumno"
        AND g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    END //
    DELIMITER ;

    call alumnasGII2015();
    ```

7. Devuelve un listado con todas las asignaturas ofertadas en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS asigOferGII2015 //
    CREATE PROCEDURE asigOferGII2015()
    BEGIN
        SELECT a.* FROM asignatura a
        JOIN grado g ON a.id_grado = g.id
        WHERE g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    END //
    DELIMITER ;

    call asigOferGII2015();
    ```

8. Devuelve un listado de los `profesores` junto con el nombre del `departamento` al que están vinculados. El listado debe devolver cuatro columnas, `primer apellido, segundo apellido, nombre y nombre del departamento.` El resultado estará ordenado alfabéticamente de menor a mayor por los `apellidos y el nombre.`

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesoresDepartamentosASC //
    CREATE PROCEDURE profesoresDepartamentosASC()
    BEGIN
        SELECT p.nombre AS nombre_profesor,
            p.apellido1, p.apellido2,
            d.nombre AS nombre_departamento
        FROM persona p
        JOIN profesor pro ON p.id = pro.id_profesor
        JOIN departamento d ON pro.id_departamento = d.id
        WHERE p.tipo = "profesor"
        ORDER BY p.nombre ASC, p.apellido1 ASC, p.apellido2 ASC;
    END //
    DELIMITER ;

    call profesoresDepartamentosASC();
    ```

9. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif `26902806M`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS asigInicioFinNif26902806M //
    CREATE PROCEDURE asigInicioFinNif26902806M()
    BEGIN
        SELECT a.nombre, c.anyo_inicio, c.anyo_fin FROM persona p
        JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
        JOIN asignatura a ON am.id_asignatura = a.id
        JOIN curso_escolar c ON am.id_curso_escolar = c.id
        WHERE p.tipo = "alumno" AND p.nif = "26902806M";
    END //
    DELIMITER ;

    call asigInicioFinNif26902806M();
    ```

10. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS depsProfsAsigGII2015 //
    CREATE PROCEDURE depsProfsAsigGII2015()
    BEGIN
        SELECT DISTINCT d.nombre FROM departamento d
        JOIN profesor pro ON d.id = pro.id_departamento
        JOIN persona p ON pro.id_profesor = p.id
        JOIN asignatura a ON p.id = a.id_profesor
        JOIN grado g ON a.id_grado = g.id
        WHERE p.tipo = "profesor" AND g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    END //
    DELIMITER ;

    call depsProfsAsigGII2015();
    ```

11. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnosMatricula20182019 //
    CREATE PROCEDURE alumnosMatricula20182019()
    BEGIN
        SELECT DISTINCT p.* FROM persona p
        JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
        JOIN asignatura a ON am.id_asignatura = a.id
        JOIN curso_escolar c ON am.id_curso_escolar = c.id
        WHERE p.tipo = "alumno" AND c.anyo_inicio = 2018 AND c.anyo_fin = 2019;
    END //
    DELIMITER ;

    call alumnosMatricula20182019();
    ```

12. Devuelve un listado con los nombres de **todos** los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS allProfYDeps //
    CREATE PROCEDURE allProfYDeps()
    BEGIN
        SELECT p.id, p.nombre, p.apellido1, p.apellido2, d.nombre AS nombre_departamento, d.id
        FROM persona p
        LEFT JOIN profesor pro ON p.id = pro.id_profesor
        JOIN departamento d ON pro.id_departamento = d.id
        WHERE p.tipo = "profesor"
        ORDER BY d.nombre ASC, p.nombre ASC, p.apellido1 ASC, p.apellido2 ASC;
    END //
    DELIMITER ;

    call allProfYDeps();
    ```

13. Devuelve un listado con los profesores que no están asociados a un departamento.Devuelve un listado con los departamentos que no tienen profesores asociados.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesoresSinDep //
    CREATE PROCEDURE profesoresSinDep()
    BEGIN
        SELECT p.nombre FROM persona p
        LEFT JOIN profesor pro ON p.id = pro.id_profesor
        WHERE p.tipo = "profesor" AND pro.id_profesor IS NULL;
    END //
    DELIMITER ;

    call profesoresSinDep();
    ```

14. Devuelve un listado con los profesores que no imparten ninguna asignatura.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesoresSinAsig //
    CREATE PROCEDURE profesoresSinAsig()
    BEGIN
        SELECT p.* FROM persona p
        LEFT JOIN asignatura a ON p.id = a.id_profesor 
        WHERE p.tipo = "profesor" AND a.id_profesor IS NULL;
    END //
    DELIMITER ;

    call profesoresSinAsig();
    ```

15. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS asigsSinProfesor1 //
    CREATE PROCEDURE asigsSinProfesor1()
    BEGIN
        SELECT * FROM asignatura WHERE id_profesor IS NULL;
    END //
    DELIMITER ;

    call asigsSinProfesor1();
    ```

16. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS depAsigSinCurso //
    CREATE PROCEDURE depAsigSinCurso()
    BEGIN
        SELECT d.nombre AS nombre_departamento, a.nombre AS nombre_asignatura FROM departamento d
        JOIN profesor pro ON d.id = pro.id_departamento
        JOIN persona p ON pro.id_profesor = p.id
        JOIN asignatura a ON p.id = a.id_profesor
        LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
        WHERE p.tipo = "profesor" AND am.id_asignatura IS NULL;
    END //
    DELIMITER ;

    call depAsigSinCurso();
    ```

17. Devuelve el número total de **alumnas** que hay.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadAlumnas //
    CREATE PROCEDURE cantidadAlumnas()
    BEGIN
        SELECT COUNT(id) AS cantidad_alumnas FROM persona WHERE sexo = "M";
    END //
    DELIMITER ;

    call cantidadAlumnas();
    ```

18. Calcula cuántos alumnos nacieron en `1999`.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadAlumnos1999 //
    CREATE PROCEDURE cantidadAlumnos1999()
    BEGIN
        SELECT COUNT(id) AS cantidad_alumnos FROM persona WHERE YEAR(fecha_nacimiento) = 1999;
    END //
    DELIMITER ;

    call cantidadAlumnos1999();
    ```

19. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadProfDepDESC //
    CREATE PROCEDURE cantidadProfDepDESC()
    BEGIN
        SELECT d.nombre AS nombre_departamento, COUNT(p.id_profesor) AS cantidad_profesores
        FROM departamento d
        JOIN profesor p ON d.id = p.id_departamento
        GROUP BY d.nombre ORDER BY COUNT(p.id_profesor) DESC;
    END //
    DELIMITER ;

    call cantidadProfDepDESC();
    ```

20. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS allCantidadProfDep //
    CREATE PROCEDURE allCantidadProfDep()
    BEGIN
        SELECT d.*, COUNT(p.id_profesor) AS cantidad_profesores
        FROM departamento d
        LEFT JOIN profesor p ON d.id = p.id_departamento
        GROUP BY d.id;
    END //
    DELIMITER ;

    call allCantidadProfDep();
    ```

21. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadAsigGradoDESC //
    CREATE PROCEDURE cantidadAsigGradoDESC()
    BEGIN
        SELECT g.nombre, COUNT(a.id_grado) AS cantidad_asignaturas FROM asignatura a
        LEFT JOIN grado g ON a.id_grado = g.id
        GROUP BY g.nombre ORDER BY COUNT(a.id_grado) DESC;
    END //
    DELIMITER ;

    call cantidadAsigGradoDESC();
    ```

22. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de `40` asignaturas asociadas.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadAsigGrado40 //
    CREATE PROCEDURE cantidadAsigGrado40()
    BEGIN
        SELECT g.*, COUNT(a.id_grado) AS cantidad_asignaturas FROM asignatura a
        LEFT JOIN grado g ON a.id_grado = g.id
        GROUP BY g.id
        HAVING COUNT(a.id_grado) > 40;
    END //
    DELIMITER ;

    call cantidadAsigGrado40();
    ```

23. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS creditosGradoTipoAsig //
    CREATE PROCEDURE creditosGradoTipoAsig()
    BEGIN
        SELECT g.nombre, a.tipo, SUM(a.creditos) FROM asignatura a
        JOIN grado g ON a.id_grado = g.id
        GROUP BY g.nombre, a.tipo
        ORDER BY SUM(a.creditos) DESC;
    END //
    DELIMITER ;

    call creditosGradoTipoAsig();
    ```

24. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS cantidadAlumnosCurso //
    CREATE PROCEDURE cantidadAlumnosCurso()
    BEGIN
        SELECT c.anyo_inicio, COUNT(am.id_alumno) FROM alumno_se_matricula_asignatura am
        JOIN curso_escolar c ON am.id_curso_escolar = c.id
        GROUP BY c.anyo_inicio;
    END //
    DELIMITER ;

    call cantidadAlumnosCurso();
    ```

25. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS allCantAsigProfDESC //
    CREATE PROCEDURE allCantAsigProfDESC()
    BEGIN
        SELECT p.id, p.nombre, p.apellido1, p.apellido2, COUNT(a.id) FROM persona p
        LEFT JOIN asignatura a ON p.id = a.id_profesor
        WHERE p.tipo = "profesor"
        GROUP BY p.id
        ORDER BY COUNT(a.id) DESC;
    END //
    DELIMITER ;

    call allCantAsigProfDESC();
    ```

26. Devuelve todos los datos del alumno más joven.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS alumnoJoven //
    CREATE PROCEDURE alumnoJoven()
    BEGIN
        SELECT * FROM persona WHERE id = (
            SELECT id FROM persona ORDER BY fecha_nacimiento DESC LIMIT 1);
    END //
    DELIMITER ;

    call alumnoJoven();
    ```

27. Devuelve un listado con los profesores que no están asociados a un departamento.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesSinDep //
    CREATE PROCEDURE profesSinDep()
    BEGIN
        SELECT * FROM persona WHERE id IN (
            SELECT p.id FROM persona p
            LEFT JOIN profesor pro ON p.id = pro.id_profesor
            WHERE p.tipo = "profesor" AND pro.id_profesor IS NULL);
    END //
    DELIMITER ;

    call profesSinDep();
    ```

28. Devuelve un listado con los departamentos que no tienen profesores asociados.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS depsSinProfes //
    CREATE PROCEDURE depsSinProfes()
    BEGIN
        SELECT * FROM departamento WHERE id IN (
            SELECT id_departamento FROM profesor WHERE id_profesor IS NULL);
    END //
    DELIMITER ;

    call depsSinProfes();
    ```

29. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS profesDepSinAsig //
    CREATE PROCEDURE profesDepSinAsig()
    BEGIN
        SELECT * FROM persona WHERE id IN (
            SELECT p.id FROM persona p
            JOIN profesor pro ON p.id = pro.id_profesor
            LEFT JOIN asignatura a ON p.id = a.id_profesor
            WHERE p.tipo = "profesor" AND a.id_profesor IS NULL);
    END //
    DELIMITER ;

    call profesDepSinAsig();
    ```

30. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS asigsSinProfesor2 //
    CREATE PROCEDURE asigsSinProfesor2()
    BEGIN
        SELECT * FROM asignatura WHERE id_profesor is NULL;
    END //
    DELIMITER ;

    call asigsSinProfesor2();
    ```

31. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

    ```sql
    DELIMITER //
    DROP PROCEDURE IF EXISTS depsAsigsSinCurso //
    CREATE PROCEDURE depsAsigsSinCurso()
    BEGIN
        SELECT * FROM departamento WHERE id IN (
            SELECT d.id FROM departamento d
            JOIN profesor pro ON d.id = pro.id_departamento
            JOIN persona p ON pro.id_profesor = p.id
            JOIN asignatura a ON p.id = a.id_profesor
            LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
            WHERE p.tipo = "profesor" AND am.id_asignatura IS NULL);
    END //
    DELIMITER ;

    call depsAsigsSinCurso();
    ```

## Modelo Fisico

![](./DER.png)

## Uso del Proyecto

Clona este repositorio en tu maquina local:

```BASH
git clone https://github.com/jstorra/universidadEvaluacion.git
```

---

<p align="center">Developed by <a href="https://github.com/jstorra">@jstorra</a></p>