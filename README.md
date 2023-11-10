# Quiz Universidad

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

    ```sql
    SELECT nombre, apellido1, apellido2 FROM persona
    WHERE tipo = "alumno"
    ORDER BY nombre ASC, apellido1 ASC, apellido2 ASC;
    ```

2. Averigua el nombre y los dos apellidos de los alumnos que **no** han dado de alta su número de teléfono en la base de datos.

    ```sql
    SELECT nombre, apellido1, apellido2 FROM persona WHERE tipo = "alumno" AND telefono IS NULL;
    ```

3. Devuelve el listado de los alumnos que nacieron en `1999`.

    ```sql
    SELECT * FROM persona WHERE tipo = "alumno" AND YEAR(fecha_nacimiento) = 1999;
    ```

4. Devuelve el listado de `profesores` que **no** han dado de alta su número de teléfono en la base de datos y además su nif termina en `K`.

    ```sql
    SELECT * FROM persona WHERE tipo = "profesor" AND telefono IS NULL AND nif LIKE '%k';
    ```

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador `7`.

    ```sql
    SELECT * FROM asignatura WHERE cuatrimestre = 1 AND curso = 3 AND id_grado = 7;
    ```

6. Devuelve un listado con los datos de todas las **alumnas** que se han matriculado alguna vez en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT p.* FROM persona p
    JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
    JOIN asignatura a ON am.id_asignatura = a.id
    JOIN grado g ON a.id_grado = g.id
    WHERE p.sexo = "M" AND p.tipo = "alumno" AND g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    ```

7. Devuelve un listado con todas las asignaturas ofertadas en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT a.* FROM asignatura a
    JOIN grado g ON a.id_grado = g.id
    WHERE g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    ```

8. Devuelve un listado de los `profesores` junto con el nombre del `departamento` al que están vinculados. El listado debe devolver cuatro columnas, `primer apellido, segundo apellido, nombre y nombre del departamento.` El resultado estará ordenado alfabéticamente de menor a mayor por los `apellidos y el nombre.`

    ```sql
    SELECT p.nombre AS nombre_profesor, p.apellido1, p.apellido2, d.nombre AS nombre_departamento
    FROM persona p
    JOIN profesor pro ON p.id = pro.id_profesor
    JOIN departamento d ON pro.id_departamento = d.id
    WHERE p.tipo = "profesor"
    ORDER BY p.nombre ASC, p.apellido1 ASC, p.apellido2 ASC;
    ```

9. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif `26902806M`.

    ```sql
    SELECT a.nombre, c.anyo_inicio, c.anyo_fin FROM persona p
    JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
    JOIN asignatura a ON am.id_asignatura = a.id
    JOIN curso_escolar c ON am.id_curso_escolar = c.id
    WHERE p.tipo = "alumno" AND p.nif = "26902806M";
    ```

10. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT DISTINCT d.nombre FROM departamento d
    JOIN profesor pro ON d.id = pro.id_departamento
    JOIN persona p ON pro.id_profesor = p.id
    JOIN asignatura a ON p.id = a.id_profesor
    JOIN grado g ON a.id_grado = g.id
    WHERE p.tipo = "profesor" AND g.nombre = "Grado en Ingeniería Informática (Plan 2015)";
    ```

11. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

    ```sql
    SELECT DISTINCT p.* FROM persona p
    JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
    JOIN asignatura a ON am.id_asignatura = a.id
    JOIN curso_escolar c ON am.id_curso_escolar = c.id
    WHERE p.tipo = "alumno" AND c.anyo_inicio = 2018 AND c.anyo_fin = 2019;
    ```

12. Devuelve un listado con los nombres de **todos** los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

    ```sql
    SELECT p.id, p.nombre, p.apellido1, p.apellido2, d.nombre AS nombre_departamento, d.id
    FROM persona p
    LEFT JOIN profesor pro ON p.id = pro.id_profesor
    JOIN departamento d ON pro.id_departamento = d.id
    WHERE p.tipo = "profesor"
    ORDER BY d.nombre ASC, p.nombre ASC, p.apellido1 ASC, p.apellido2 ASC;
    ```

13. Devuelve un listado con los profesores que no están asociados a un departamento.Devuelve un listado con los departamentos que no tienen profesores asociados.

    ```sql
    SELECT p.nombre FROM persona p
    LEFT JOIN profesor pro ON p.id = pro.id_profesor
    WHERE p.tipo = "profesor" AND pro.id_professor IS NULL;
    ```

14. Devuelve un listado con los profesores que no imparten ninguna asignatura.

    ```sql
    SELECT p.* FROM persona p
    LEFT JOIN asignatura a ON p.id = a.id_profesor 
    WHERE p.tipo = "profesor" AND a.id_profesor IS NULL;
    ```

15. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

    ```sql
    SELECT a.* FROM asignatura a
    LEFT JOIN persona p ON a.id_profesor = p.id
    WHERE p.tipo = "profesor" AND p.id IS NULL;
    ```

16. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

    ```sql
    SELECT d.nombre AS nombre_departamento, a.nombre AS nombre_asignatura FROM departamento d
    JOIN profesor pro ON d.id = pro.id_departamento
    JOIN persona p ON pro.id_profesor = p.id
    JOIN asignatura a ON p.id = a.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
    WHERE p.tipo = "profesor" AND am.id_asignatura IS NULL;
    ```

17. Devuelve el número total de **alumnas** que hay.

    ```sql
    SELECT COUNT(id) AS cantidad_alumnas FROM persona WHERE sexo = "M";
    ```

18. Calcula cuántos alumnos nacieron en `1999`.

    ```sql
    SELECT COUNT(id) AS cantidad_alumnos FROM persona WHERE YEAR(fecha_nacimiento) = 1999;
    ```

19. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

    ```sql
    SELECT d.nombre AS nombre_departamento, COUNT(p.id_profesor) AS cantidad_profesores
    FROM departamento d
    JOIN profesor p ON d.id = p.id_departamento
    GROUP BY d.id ORDER BY COUNT(p.id_profesor) DESC;
    ```

20. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

    ```sql
    SELECT d.*, COUNT(p.id_profesor) AS cantidad_profesores
    FROM departamento d
    LEFT JOIN profesor p ON d.id = p.id_departamento
    GROUP BY d.id;
    ```

21. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

    ```sql
    SELECT g.*, COUNT(a.id_grado) AS cantidad_asignaturas FROM asignatura a
    LEFT JOIN grado g ON a.id_grado = g.id
    GROUP BY g.id ORDER BY COUNT(a.id_grado) DESC;
    ```

22. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de `40` asignaturas asociadas.

    ```sql
    SELECT g.*, COUNT(a.id_grado) AS cantidad_asignaturas FROM asignatura a
    LEFT JOIN grado g ON a.id_grado = g.id
    GROUP BY g.id
    HAVING COUNT(a.id_grado) > 40;
    ```

23. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

    ```sql
    SELECT g.nombre, a.tipo, SUM(a.creditos) FROM asignatura a
    JOIN grado g ON a.id_grado = g.id
    GROUP BY g.nombre, a.tipo
    ORDER BY SUM(a.creditos) DESC;
    ```

24. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

    ```sql
    SELECT c.anyo_inicio, COUNT(am.id_alumno) FROM alumno_se_matricula_asignatura am
    JOIN curso_escolar c ON am.id_curso_escolar = c.id
    GROUP BY c.anyo_inicio;
    ```

25. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

    ```sql
    SELECT p.id, p.nombre, p.apellido1, p.apellido2, COUNT(a.id) FROM persona p
    LEFT JOIN asignatura a ON p.id = a.id_profesor
    WHERE p.tipo = "profesor"
    GROUP BY p.id
    ORDER BY COUNT(a.id) DESC;
    ```

26. Devuelve todos los datos del alumno más joven.

    ```sql
    SELECT * FROM persona WHERE id = (
        SELECT id FROM persona ORDER BY fecha_nacimiento DESC LIMIT 1
    );
    ```

27. Devuelve un listado con los profesores que no están asociados a un departamento.

    ```sql
    SELECT * FROM persona WHERE id IN (
        SELECT p.id FROM persona p
        LEFT JOIN profesor pro ON p.id = pro.id_profesor
        WHERE p.tipo = "profesor" AND pro.id_profesor IS NULL
    );
    ```

28. Devuelve un listado con los departamentos que no tienen profesores asociados.

    ```sql
    SELECT * FROM departamento WHERE id IN (
        SELECT id_departamento FROM profesor WHERE id_profesor IS NULL
    );
    ```

29. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

    ```sql
    SELECT * FROM persona WHERE id IN (
        SELECT p.id FROM persona p
        JOIN profesor pro ON p.id = pro.id_profesor
        LEFT JOIN asignatura a ON p.id = a.id_profesor
        WHERE p.tipo = "profesor" AND a.id_profesor IS NULL
    );
    ```

30. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

    ```sql
    SELECT * FROM asignatura WHERE id IN (
        SELECT id FROM asignatura WHERE id_profesor is NULL
    );
    ```

31. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

    ```sql
    SELECT * FROM departamento WHERE id IN (
        SELECT d.id FROM departamento d
        JOIN profesor pro ON d.id = pro.id_departamento
        JOIN persona p ON pro.id_profesor = p.id
        JOIN asignatura a ON p.id = a.id_profesor
        LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
        WHERE p.tipo = "profesor" AND am.id_asignatura IS NULL
    );
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