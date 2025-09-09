Continuando con la practica de Power bi a continuacion evaluaremos un archivo excel de recursos humenos en una empresa [Datos.xlsx](https://github.com/user-attachments/files/22234752/Datos.xlsx) que contiene las siguienes columnas ID Empleado
ID Empleado,

Nombre del empleado

Edad

Genero

Departamento

Nombre Supervisor

Fecha Ingreso

Salario

Evaluacion

Nivel de Satisfaccion, Ausencia(hs)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Segun las especificaciones del negocio necesitamos los siguentes indicadores y las siguientes visualizaciones
----------------------------------
Indicadores - KPI's 

Total de Colaboradores por Género (en % y Cantidad).

Tiempo en el Cargo (Años)

Porcentaje de Personal Insatisfecho

Tasa de Ausencia

Promedio de Evaluación, Edad

Costo de Planilla.

Visualizaciones

Grafico de Barras - Total de colaboradores por departamento

Grafico de Barras - Total de colaboradores por grupo de edad

Gráfico de Treemap - Cantidad de Colaboradores por Supervisor 

Matriz - Análisis de indicadores por departamento

Gráfico de Dispersión - Relación entre sueldo y edad de los colaboradores.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Una vez cargado el excel en Power bi, procedemos a crear dentro de la Vista de modelo una nueva tabla para depositar ahi las medidas que vamos realizando segun las especificaciones del negocio
<img width="1366" height="726" alt="Vista modelo" src="https://github.com/user-attachments/assets/6185fac5-a976-4778-821c-ae6f3f271b40" />

Dentro de la nueva tabla creamos una nueva medida para poder saber la cantidad de registros que poseemos 

            Total Colaboradores = COUNTA(Colaboradores[Nombre Empleado]) 

Con una nueva nueva mediante una expresion DAX sabemos cuando registros hay que sea masculino en el genero y lo mismo realizamos con la medida para saber el total de mujeres 

            Total Hombres = CALCULATE([Total Colaboradores], Colaboradores[Género]= "Masculino")

            Total Mujeres = CALCULATE([Total Colaboradores], Colaboradores[Género]= "Femenino")

Usando la sentencia DIVIDE creamos una nueva medida para saber el porcentaje de masculinos, el porcetajes de femeninos y el porcentaje de insatisfechos

            % Hombres = DIVIDE([Total Hombres], [Total Colaboradores],0)

            % Mujeres = DIVIDE([Total Mujeres], [Total Colaboradores],0)

            % Insatisfecho = DIVIDE([Total insatisfechos], [Total Colaboradores],0)

Para la medida horas planificadas tomamos 8 horas de trabajo diarias y por mes los dias de trabajo 25

           Horas planificadas = 
               VAR horas_trabajo_diario= 8
               VAR dia_trabajo_mensual= 25
           RETURN
           [Total Colaboradores]*horas_trabajo_diario*dia_trabajo_mensual

Creamos la medida 

            Total de horas de ausentismo = SUM(Colaboradores[Ausencia(horas)])

Usando Horas planificadas obtenemos la tasa de ausencia  y el total de horas de ausentismo

            Tasa de ausencia = [Total de horas de ausentismo]/ [Horas planificadas]

Por ultimo realizamos la medida para saber la cantidad de tiempo (años) que tiene en el cargo

            Tiempo en el cargo (Años) = DIVIDE(sum(Colaboradores[Total de años en el puesto]),[Total Colaboradores],0)

Quedando de esta manera completa la nueva tabla
<img width="1366" height="768" alt="medidas" src="https://github.com/user-attachments/assets/2056e9c5-3560-40ee-a6a6-d4ed429a57f5" />

Una vez completa todas las medidas a usar procedemos a realizar los graficos correspondientes 

<img width="1366" height="722" alt="imagen dashboard2" src="https://github.com/user-attachments/assets/2a29c854-b58c-4ecb-aaa6-f093a63221d5" />

En la grafica podemos observar 4 segmentadores del tipo menu desplegable
------------------------------------------------------------------------
1 Genero

2 Nombre del supervisor

3 Grupo de edad (que lo creamos con una nueva columna dividida por rango de edades)

           Grupo de edad = 
               IF(Colaboradores[Edad]<= 30, "20-30",
               IF(Colaboradores[Edad]<=40, "31-40",
               IF(Colaboradores[Edad]<=50, "41-50",
               IF(Colaboradores[Edad]<=60, "51-60",
               "+60"
              ))))

4 Departamento 

De la pagina https://www.flaticon.es/ descargamos los iconos para darle una mejor el dashboard de manera visual

Visualizaciones con tarjeta 
----------------------------------------
1 debajo del icono de una mujer el porcentaje de mujeres y la cantidad en numeros

2 Debajo del icono de un hombre podemos observar el porcentaje como el total de hombres 

3 Observamos tambien con una tarjeta el tiempo en el cargo (años)

4 % de insatisfaccion 

5 el porcentaje de la tasa de ausencias 

6 A la izquierda vemos el promedio de evaluacion

7 promedio de edad

8 el costo total de la planilla 

En una matriz 
------------
por departamento visualizamos el timpo del cargo en años, % insatisfaccion, tasa de ausencias, promedio de evaluacion y la suma del salario

Mediante los graficos de barra podemos ver 2 graficos
-----------------------------------------------------
1 los colaboradores por departamento

2 colaboradores por grupo de edad

En un treemap 
--------------------------------------------------------------------
observamos la cantidad de colaboradores por supervisor

por ultimo mediante un grafico de dispersion
--------------------------------------------------------------------------------------------------
observamos la relacion entra la edad y el salario 
 
Concluyendo de esta menera con los requisitos implementados por el negocio  
--------------------------------------------------------------------------

