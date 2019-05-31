# Tarea para BD06.  
Detalles de la tarea de esta unidad.  
  
Para la realización de la tarea de esta unidad nos basaremos en el caso de estudio expuesto en los contenidos de la misma. La tarea que te pedimos que realices consta de 2 actividades:  
  
Actividad 1.  
Queremos crear un subprograma que mueva una familia origen a otra de destino, de la que pasará a ser hija.  
  
Se debe comprobar que la familia destino no es hija de la familia origen. Para ello debemos crear una función recursiva auxiliar que haga dicha comprobación.  
También comprobaremos que tanto la familia origen, como la familia destino existen (el subprograma aceptará como parámetros los identificadores de ambas familias).  
Para hacer las comprobaciones de si ambas familias existen se deberá utilizar un único cursor variable.  
Además si la familia origen pertenecía a una oficina deberá dejar de pertenecer a esa oficina y sólo ser hija de la familia destino.  
El subprograma deberá lanzar todos los errores que se puedan producir en su ejecución mediante errores que identifiquen con un mensaje adecuado por qué se ha producido dicho error.  
```SQL
SET SERVEROUTPUT ON; 
DECLARE
  id_familia_origen familias.identificador%TYPE := '&Familia_Origen'; 
  familia_origen familias.familia%TYPE;
  id_familia_destino familias.identificador%TYPE := '&Familia_Destino';
  nombre_origen familias.nombre%TYPE;
  nombre_destino familias.nombre%TYPE;
  oficina_origen familias.oficina%TYPE;
BEGIN 
  SELECT FAMILIA INTO familia_origen
  FROM FAMILIAS
  WHERE IDENTIFICADOR LIKE id_familia_origen;

  SELECT NOMBRE INTO nombre_origen
  FROM FAMILIAS
  WHERE IDENTIFICADOR LIKE id_familia_origen;
  
  SELECT NOMBRE INTO nombre_destino 
  FROM FAMILIAS
  WHERE IDENTIFICADOR LIKE id_familia_destino;
  
  SELECT OFICINA INTO oficina_origen
  FROM FAMILIAS
  WHERE IDENTIFICADOR LIKE id_familia_origen;

  IF(familia_origen = id_familia_destino OR familia_origen LIKE id_familia_destino || '%') THEN 
    DBMS_OUTPUT.PUT_LINE('La familia origen ' || nombre_origen || ' pertenece a ' || nombre_destino); 
  ELSIF (id_familia_destino LIKE familia_origen || '%') THEN
        DBMS_OUTPUT.PUT_LINE('La familia destino ' || nombre_destino || ' es hija de ' || nombre_origen); 
  ELSE
    DBMS_OUTPUT.PUT_LINE('La familia ' || nombre_origen || ' no pertenece a ' || nombre_destino);
    IF (oficina_origen IS NOT NULL) THEN
        DBMS_OUTPUT.PUT_LINE('La familia ' || nombre_origen || ' pertenece a la oficina ' || oficina_origen);
        UPDATE FAMILIAS SET OFICINA=NULL, FAMILIA=id_familia_destino WHERE IDENTIFICADOR = id_familia_origen;
    END IF;
  END IF; 
EXCEPTION
  WHEN NO_DATA_FOUND THEN 
    DBMS_OUTPUT.PUT_LINE('No se localizó la familia introducida'); 
END;
```
Actividad 2.  
Queremos controlar algunas restricciones a la hora de trabajar con agentes:  
  
El usuario y la clave de un agente no pueden ser iguales.  
```SQL
```
La habilidad de un agente debe estar comprendida entre 0 y 9 (ambos inclusive).  
```SQL
```
La categoría de un agente sólo puede ser igual a 0, 1 o 2.  
```SQL
```
Si un agente pertenece a una oficina directamente, su categoría debe ser igual 2, si un agente no pertenece a una oficina directamente, su categoría no puede ser 2.  
```SQL
```
No puede haber agentes que no pertenezcan a una oficina o a una familia.  
```SQL
```
No puede haber agentes que pertenezcan a una oficina y a una familia a la vez.  
```SQL
```
Debes crear un disparador para asegurar estas restricciones. El disparador deberá lanzar todos los errores que se puedan producir en su ejecución mediante errores que identifiquen con un mensaje adecuado por qué se ha producido dicho error.  
```SQL
```
