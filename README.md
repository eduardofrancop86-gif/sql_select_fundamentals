# sql_select_fundamentals
sql-select-fundamentals/ ├── consultas_basicas.sql └── README.md

Buenas Prácticas y Estándares de SQL
¿Por qué evitar SELECT * en producción?
El uso de SELECT * en consultas o pipelines en producción introduce ineficiencias críticas a nivel de infraestructura, estabilidad y gobernanza:
Rendimiento y costo de procesamiento: Fuerza al motor a leer y transferir datos innecesarios. En data warehouses con almacenamiento columnar (como BigQuery o Snowflake), SELECT * escanea la tabla completa, disparando los costos de consulta y saturando el ancho de banda y la memoria.
Mantenibilidad y fragilidad de pipelines: Si el esquema de la tabla cambia (se agrega, elimina o reordena una columna), las vistas, modelos o procesos ETL downstream pueden romperse o acoplar datos en campos incorrectos de forma silenciosa.
Seguridad y privacidad (Gobernanza): Transfiere columnas con datos sensibles o PII (información de identificación personal) a capas de aplicación o logs que no necesitan acceso a esa información.

Impacto de los Alias (AS) en la interfaz con Stakeholders
Los alias funcionan como una capa de traducción entre el modelo de datos y el lenguaje de negocio. Permiten que los datasets consumidos por áreas no técnicas generen claridad inmediata sin requerir documentación técnica constante.

Ejemplo de transformación (Área de Finanzas)
Consulta sin alias:
SQL
SELECT total_amount FROM facturas;
Impacto: Para el equipo financiero, total_amount es ambiguo. No aclara si es un monto bruto o neto, si incluye IVA o en qué divisa está expresado.

Consulta con alias:
SQL
SELECT 
    total_amount AS "Monto Total Facturado con IVA ($ MXN)" 
FROM facturas;
Impacto: Transforma el nombre de la variable en una métrica autoexplicativa. Finanzas interpreta el alcance contable, la moneda y el impuesto aplicado de forma directa en cualquier reporte o dashboard.
