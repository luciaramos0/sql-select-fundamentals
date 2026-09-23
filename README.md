# sql-select-fundamentals
¿Por qué es mala práctica usar SELECT * en producción? Mencioná al menos dos razones concretas (rendimiento, mantenibilidad, seguridad)
Rendimiento:
SELECT * trae todas las columnas de la tabla, aunque no las necesites. Esto significa más datos viajando entre la base de datos y la aplicación, más uso de memoria y de ancho de banda, y consultas más lentas (especialmente si la tabla tiene columnas pesadas o millones de filas). 
Ejemplo de rendimiento:
Mala práctica: trae las 20 columnas aunque uses solo 2
SELECT * FROM sales;
Buena práctica: trae solo lo necesario
SELECT product_name, total_amount FROM sales;

Mantenibilidad:
Si alguien modifica la estructura de la tabla (agrega, elimina o renombra una columna), una consulta con SELECT * puede romperse silenciosamente o devolver datos distintos a los que el código espera, sin que quede claro por qué. Además, cuando otra persona lee tu código, no tiene idea de qué columnas estás usando realmente — tiene que ir a mirar la tabla para entenderlo, en vez de simplemente leer la consulta.
Ejemplo de mantenibilidad:
Una aplicación espera que la consulta devuelva las columnas en este orden: product_name, total_amount. Si usás SELECT * y alguien le agrega una columna nueva a la tabla (por ejemplo descuento) en el medio, el orden de las columnas cambia y tu aplicación puede empezar a mostrar datos en el lugar equivocado, sin ningún error visible.

Seguridad
Podés terminar exponiendo columnas sensibles sin querer (contraseñas, datos personales, información interna) simplemente porque están en la tabla, aunque la aplicación no las necesite ni debería mostrarlas.
Ejemplo de seguridad:
Si la tabla customers tiene una columna password o dni, y usás:
SELECT * FROM customers;
estás exponiendo esos datos sensibles en el resultado, aunque tu reporte solo necesitaba mostrar customer_name y email.

¿Por qué son importantes los alias para un stakeholder no técnico? Explicá con un ejemplo concreto cómo un alias transforma total_amount en algo que cualquier persona del área de finanzas puede interpretar directamente.
El alias traduce el lenguaje técnico de la base de datos al lenguaje del negocio, haciendo que los reportes sean autoexplicativos para quien los va a usar, que normalmente no tiene por qué saber cómo está armada la base de datos.
Ejemplo:
SELECT total_amount AS monto_total_pesos
FROM sales;
Con el alias monto_total_pesos, cualquier persona del área de finanzas que abra el reporte entiende inmediatamente y sin ayuda de qué se trata: es el monto total de la venta, expresado en pesos. 
