# Proyecto-EDVAI

## Introducción

En este proyecto se realizó un análisis del Ecommerce de la marca The Look que se dedica a la venta de indumentaria masculina y femenina. La información fue obtenida desde las bases públicas de Big Query y el foco del análisis esta puesto en las ventas realizadas, principalmente de, la facturación y los objetivos para el año en curso y, también, en el ecommerce y el tráfico de usuarios para poder acompañar los objetivos y las desiciones propuestos por esta área con información actualizada.

La base de datos utilizada desde big query contiene las siguientes tablas:



![image](https://github.com/ianziano/Proyecto-EDVAI/assets/169062674/475f5273-8ab9-43d9-8858-ea4cd00fa61b)


Para la realización de los desarrollos solicitamos a las áreas de negocio que nos compartan sus objetivos actuales para tener referencias de que información y como la mostraríamos. Estos objetivos se dividen para el área de Ventas y del Ecommerce que son los siguientes:

### Ventas:

1- Alcanzar un GMV total de 7 millones para el año 2024 duplicando asi la venta del año anterior.

2- Alcanzar 75.000 órdenes para el año 2024. 

3- Aumentar el valor de ticket promedio en un 10% así alcanzando la totalidad de órdenes poder alcanzar el objetivo de GMV.

4- Identificar los países y categorías que tienen mas facturación


### Efectividad de usuarios y ecommerce:

1- Alcanzar un 20% de conversión en la web con campañas de promoción y bonificaciones en envíos.

2- Disminuir la tasa de carritos abandonados mediante email marketing y bonificaciones en grandes compras.

3- Incrementar el tráfico de visitas al ecommerce con respecto al año anterior

4- Identificar el orígen de los usuarios para poder eficientizar el presupuesto en campañas. 


## Desarrollos

Con esta información, he realizado consultas en la consola de Big Query donde seleccioné las tablas y campos necesarios para luego importar a Power Bi unicamente lo necesario para la posterior realización de medidas y visualizaciones.

Con estas modificaciones realizadas importé directamente desde Big Query a Power Query, donde modifiqué el tipo de variable de los campos de Fecha y Hora como variable únicamente de Fecha.

Luego creé una tabla calendario para poder tener las jerarquías de fecha en caso de que el cliente lo necesite y realicé las relaciones entre las tablas. El Diagrama Entidad Relación para este desarrolló es el siguiente:

![image](https://github.com/ianziano/Proyecto-EDVAI/assets/169062674/0c3a4c48-88ae-4ee2-9474-41d5be5f224d)

Para el desarrollo de las visualizaciones, las medidas y los KPIs utilicé fórmulas DAX y las desiciones de las medidas a crear fueron tomadas teniendo en cuenta el plan de métricas dearrollado junto con los equipos de negocio de Ventas y Marketing. A continuación comparto el link con el plan de métricas y los detalles de las mismas: https://docs.google.com/spreadsheets/d/1qfVXB0ISH6tU_0HF9s4BsUqqQBrF574-B1im8usHRAw/edit?usp=sharing

Entre las medidas realizadas se pueden destacar las siguientes:

- GMV total y por año.
- GMV del año anterior. Diferencia absoluta y porcentual. 
- Profit por cada orden de compra. Para esta medida se agregó una columna en la tabla Dim_Products calculando la diferencia entre el precio de venta y el costo de cada producto:

  Profit = CALCULATE(SUM(Dim_products[Profit]),Fact_orders[order_id])

- Tasa de conversión web:
  
  DIVIDE (
  CALCULATE(
    [Trafico de usuarios],
    Dim_user_events[event_type] = "purchase"),
  DISTINCTCOUNT(Dim_user_events[id]))
  
- Tasa de abandono de carritos:
  
  DIVIDE(
  CALCULATE(
    [Trafico de usuarios],
    Dim_user_events[event_type] = "cart"),
  (CALCULATE(
    [Trafico de usuarios],
    Dim_user_events[event_type] = "purchase")
  + CALCULATE(
    [Trafico de usuarios],
    Dim_user_events[event_type] = "cart"))

- Ticket promedio: CALCULATE(SUM(Fact_orders[sale_price])/[Cantidad de Ordenes],Fact_orders[order_id]) 


##### Flujo de datos

![image](https://github.com/ianziano/Proyecto-EDVAI/assets/169062674/42463367-7a73-4e38-815f-b60eca8b2a82)








### Link a Power Bi Service

https://app.powerbi.com/reportEmbed?reportId=59c6eb9c-d471-4930-a9cc-fc43fc694d08&autoAuth=true&ctid=ca0f58d7-fbd0-461f-860d-6a78108097e0

https://app.powerbi.com/groups/me/reports/59c6eb9c-d471-4930-a9cc-fc43fc694d08?ctid=ca0f58d7-fbd0-461f-860d-6a78108097e0&pbi_source=linkShare&bookmarkGuid=b1ca7958-9264-4aab-9488-161c4388263a

https://app.powerbi.com/view?r=eyJrIjoiNWM1YzI2NTItMTU2Yy00YThkLWE1N2YtNmEwNTMxNTM2NmM1IiwidCI6ImNhMGY1OGQ3LWZiZDAtNDYxZi04NjBkLTZhNzgxMDgwOTdlMCIsImMiOjR9


<iframe title="Ignacio Anziano - Proyecto final EDVAI - Look Ecommerce" width="600" height="373.5" src="https://app.powerbi.com/view?r=eyJrIjoiNWM1YzI2NTItMTU2Yy00YThkLWE1N2YtNmEwNTMxNTM2NmM1IiwidCI6ImNhMGY1OGQ3LWZiZDAtNDYxZi04NjBkLTZhNzgxMDgwOTdlMCIsImMiOjR9" frameborder="0" allowFullScreen="true"></iframe>


