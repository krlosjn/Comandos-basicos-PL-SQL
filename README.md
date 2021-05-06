ESTRUCTURA 

SET SERVEROUTPUT ON;

DECLARE 

aquí se declaran y/o se inician variables

BEGIN 

aquí se escribe el cuerpo del código 
END;
/

==================================================================
EJEMPLO DE UN IF ELSE

SET SERVEROUTPUT ON;

DECLARE 
  a NUMBER:=522;
  b NUMBER:=10;
  nombreUno NVARCHAR2(100):='Carlos';
  nombreDos NVARCHAR2(100);
   
BEGIN 
  IF(a!=b) THEN 
    dbms_output.put_line('a es diferente a b '); 
  
  END IF;

    IF (nombreUno LIKE 'C%') THEN 
        dbms_output.put_line('Contiene una C');
    END IF;



END;
/

=========================================================================
 EJEMPLO IF ELSIF

SET SERVEROUTPUT ON;

DECLARE 

    salario number:=round(dbms_random.value(600,2000));
    aumento number;
    salarioNeto number;
    aumentoHecho number;

BEGIN 
    DBMS_OUTPUT.PUT_LINE('ESTO ES UN EJERCIICO DE PRUEBA ');
    
    dbms_output.put_line('el salario que tenemos es de ' || salario );
    
    IF(salario<=600) THEN 
        aumento:=0.15;
    ELSIF (salario BETWEEN 601 AND 950) THEN 
        aumento:=0.135;
    ELSIF(salario BETWEEN 951 AND 1400) THEN 
        aumento:=0.10;
    ELSE
        aumento:=0.05;
    END IF;
    
    salarioNeto:=(salario*aumento)+salario;
    aumentoHecho:=salario*aumento;
    
    dbms_output.put_line('aumento aplicado  de ' || aumentohecho);
    dbms_output.put_line('aumento del ' || aumento || ' con un salario de ' || salario);
    dbms_output.put_line('salario total de  ' || salarioNeto);
   
    

END;
/

========================================================================================
EJEMPLO DE UN CASE

SET SERVEROUTPUT ON;

DECLARE 
    numero Number;
    
BEGIN 
  
   numero:=round(dbms_random.value(0,5));

    CASE 
        WHEN (numero=0) THEN 
        dbms_output.put_line('Cero');
     
        WHEN (numero=1) THEN 
        dbms_output.put_line('Uno');
        WHEN (numero=2) THEN 
        dbms_output.put_line('Dos');
        WHEN (numero=3) THEN 
            dbms_output.put_line('Tres');
        WHEN (numero=4) THEN 
            dbms_output.put_line('Cuatro');
        ELSE 
            dbms_output.put_line('Cinco');
    END CASE;
END;
/

======================================
CURSOR CON LA TABLA DE PRUEBA 


SET SERVEROUTPUT ON; 
DECLARE
    
    desa desarrolladores%rowtype;
    
    CURSOR c_desarrolladores IS 
        select * from desarrolladores;


BEGIN
    OPEN c_desarrolladores;
        LOOP
            FETCH c_desarrolladores INTO desa;
            EXIT WHEN c_desarrolladores%notfound;
        END LOOP;
        
          
   DBMS_OUTPUT.PUT_LINE('Nombre desarrollador ' || desa.nombre);
    CLOSE c_desarrolladores;
  
END;



=====================================
CICLO FOR

SET SERVEROUTPUT ON;

DECLARE 

BEGIN 

    FOR i IN 1..10 LOOP
        IF(i=4) THEN
        
            CONTINUE;
        END IF;
        dbms_output.put_line('indice ' || i);
        
    END LOOP;
    

END;
/
==============================================================
FUNCIÓN LAZO
SET SERVEROUTPUT ON;


DECLARE 

    x number;

BEGIN 
        x:=0;
    LOOP
        DBMS_OUTPUT.PUT_LINE (x);
        x:=x+10;
        
        IF(x>30) THEN 
            EXIT;
        END IF;
        
    END LOOP;
END;
/

====================================================================================
EJEMPLO DE UN CURSOR 
SET SERVEROUTPUT ON;

DECLARE

    empleado empleados%rowtype;

    
    CURSOR c_empleados IS 
        SELECT * FROM empleados;
        
    PROCEDURE imprimir_empleado(empleado empleados%rowtype) IS 
        salarioDiario number;
        salarioTotal number;
        
    
    BEGIN 
        salarioDiario:=empleado.salario/30;
        salarioTotal:=empleado.salario*empleado.diasTrabajados;
        dbms_output.put_line('===========================================' );
        dbms_output.put_line('Nombre ' || empleado.nombre);
        dbms_output.put_line('Salario diario  ' || round(salarioDiario,2));
        dbms_output.put_line('Días de trabajo  ' || empleado.diasTrabajados);
        dbms_output.put_line('Salario a pagar ' || round(salarioTotal,2));
      
    END;
    
BEGIN 

    OPEN c_empleados;
    
        LOOP
            FETCH c_empleados INTO empleado;
            EXIT WHEN c_empleados%notfound;
            
            imprimir_empleado(empleado);
        END LOOP;
    
    CLOSE c_empleados;

END;
/

===========================================================================================
CREACIÓN DE EXCEPCIONES PROPIAS 

SET SERVEROUTPUT ON;
DECLARE

    aleatorio number:= round(dbms_random.value(1,3));
    
    error1 exception;
    error2 exception;
    error3 exception;


BEGIN
    CASE 
        WHEN aleatorio=1 THEN 
            RAISE error1;
            
        WHEN aleatorio=2 THEN 
            RAISE error2;
        WHEN aleatorio=3 THEN 
            RAISE error3;
    END CASE;
    

EXCEPTION 

    WHEN error1 THEN 
        dbms_output.put_line('no tenemos el error 1');
     WHEN error2 THEN 
        dbms_output.put_line('no tenemos el error 2');
    
     WHEN error3 THEN 
        dbms_output.put_line('no tenemos el error 3');
    


END;
/


--========================================================================================================================

SET SERVEROUTPUT ON;
DECLARE  
    nombre_desarrollador nvarchar2(100);
    salario_desarrollador  number;
    cargo_desarrollador nvarchar2(100);
    
    CURSOR  cur_informacion_desarrollador IS 
        SELECT a.nombre, a.salario, b.descripcion_cargo FROM desarrolladores a , cargo_desarrolladores b 
            WHERE a.ide_cargo=b.ide_cargo ;
BEGIN 
    OPEN cur_informacion_desarrollador;
        LOOP
            FETCH cur_informacion_desarrollador INTO nombre_desarrollador,salario_desarrollador,cargo_desarrollador;
            
            EXIT WHEN cur_informacion_desarrollador%notfound;
            DBMS_OUTPUT.PUT_LINE('========================');
            DBMS_OUTPUT.PUT_LINE('Nombre ' || nombre_desarrollador);
            DBMS_OUTPUT.PUT_LINE('salario ' || salario_desarrollador);
            DBMS_OUTPUT.PUT_LINE('cargo ' || cargo_desarrollador);
         
        END LOOP;
    CLOSE cur_informacion_desarrollador;
END;
/
SELECT a.*, b.* FROM desarrolladores a ,cargo_desarrolladores b ;
=======================================================================================================================================
CREATE OR REPLACE PROCEDURE informacion_desarrolladores
AS
    nombre_desarrollador nvarchar2(100);
    salario_desarrollador number;
    cargo_desarrollador nvarchar2(100);
    
    CURSOR cur_desarrolladores IS 
        SELECT a.nombre, a.salario, b.descripcion_cargo FROM desarrolladores a , cargo_desarrolladores b  WHERE 
        a.ide_cargo=b.ide_cargo;
BEGIN 

    OPEN cur_desarrolladores;
        LOOP
            FETCH cur_desarrolladores INTO nombre_desarrollador, salario_desarrollador, cargo_desarrollador;
            EXIT WHEN cur_desarrolladores%notfound;
            dbms_output.put_line('==============================================');
            dbms_output.put_line('Nombre de empleado ' || nombre_desarrollador);
            dbms_output.put_line('Salario de empleado ' || salario_desarrollador);
            dbms_output.put_line('Cargo de empleado ' || cargo_desarrollador);
            
        END LOOP;
        
        EXCEPTION 
            WHEN no_data_found THEN 
                DBMS_OUTPUT.PUT_LINE('No se han encontrado data');
    CLOSE cur_desarrolladores;

END;
/
SELECT a.*, b.* FROM desarrolladores a, cargo_desarrolladores b;
