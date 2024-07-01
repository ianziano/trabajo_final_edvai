# Proyecto-EDVAI

## Introducción


En este proyecto se realizó un análisis del Ecommerce de la marca The Look que se dedica a la venta de indumentaria masculina y femenina. La información fue obtenida desde las bases públicas de Big Query y el foco del análisis esta puesto en las ventas realizadas, principalmente de, la facturación y los objetivos para el año en curso y, también, en el ecommerce y el tráfico de usuarios para poder acompañar los objetivos y las desiciones propuestos por esta área con información actualizada.

La base de datos utilizada desde el repositorio público de big query contiene las siguientes tablas:



![image](https://github.com/ianziano/Proyecto-EDVAI/assets/169062674/475f5273-8ab9-43d9-8858-ea4cd00fa61b)



Para la realización de los desarrollos solicitamos a las áreas de negocio que nos compartan sus objetivos para definir que información vamos a utilizar de todas estas tablas y de que manera vamos a definir las visualizaiones. 

## Objetivos


Estos objetivos se dividen para el área de Ventas y de Ecommerce.

### Ventas:

1- Alcanzar un GMV total de 7 millones para el año 2024 duplicando asi la venta del año anterior.

2- Alcanzar 75.000 órdenes para el año 2024. 

3- Aumentar el valor de ticket promedio en un 10% así alcanzando la totalidad de órdenes poder alcanzar el objetivo de GMV.

4- Identificar los países y categorías que tienen mas facturación.

5- Reducir la cancelación de órdenes a un 10% del total.


### Efectividad de usuarios y ecommerce:

1- Alcanzar un 20% de conversión en la web con campañas de promoción y bonificaciones en envíos.

2- Disminuir la tasa de carritos abandonados mediante email marketing y bonificaciones en grandes compras.

3- Incrementar el tráfico de visitas al ecommerce con respecto al año anterior.

4- Identificar el orígen de los usuarios para poder eficientizar el presupuesto en campañas. 

5- Alcanzar los 80.000 usuarios con compra.


Estos objetivos vienen acompañados de los KPIs que se desarrollaron con los equipos de negocio para poder realizar un seguimiento y cálculo de las métricas para el alcance de estos. A continuación copio el link al plan de metricas desarrollado que sustenta el desarrollo realizado en Power Bi. https://docs.google.com/spreadsheets/d/1qfVXB0ISH6tU_0HF9s4BsUqqQBrF574-B1im8usHRAw/edit?usp=sharing

## Desarrollos

En primer lugar, realicé distintas consultas para conocer y navegar sobre el data set original desde la base pública de Big Query. Con esta información, he realizado consultas en la consola de Big Query donde seleccioné las tablas y campos necesarios para luego importar a Power Bi únicamente los datos necesarios para la posterior realización de medidas y visualizaciones. En la siguiente imágen se observa el data set en la capa Bronze y en la capa Silver donde se encuentran los datos que vamos a utilizar para el desarrollo del tablero:

![image](https://github.com/ianziano/trabajo_final_edvai/assets/169062674/11a49eee-e991-4b98-bb09-fdf76b378fa9)

Comparto algunas de las queries para la creación de la capa silver:

create table `edvai-2024-421315.Ecommerce_proyecto_final.Dim_products` as
SELECT p.id, p.cost, p.category, p.name, p.brand, p.retail_price, p.department, p.distribution_center_id
FROM `bigquery-public-data.thelook_ecommerce.products` p

CREATE TABLE `edvai-2024-421315.Ecommerce_proyecto_final.Dim_users` as
SELECT u.id, u.city, u.country, u.created_at, u.gender, u.latitude, u.longitude
FROM `bigquery-public-data.thelook_ecommerce.users` u

Con estas modificaciones realizadas importé directamente desde Big Query a Power Query, donde modifiqué el tipo de variable de los campos de Fecha y Hora como variable únicamente de Fecha.

Luego creé una tabla calendario para poder tener las jerarquías de fecha en caso de que el cliente lo necesite y realicé las relaciones entre las tablas. El Diagrama Entidad Relación para este desarrolló es el siguiente:

![image](https://github.com/ianziano/trabajo_final_edvai/assets/169062674/a36eb81a-693a-41cf-bea1-2a006a84b35a)


Para el desarrollo de las visualizaciones, las medidas y los KPIs utilicé fórmulas DAX y las desiciones de las medidas a crear fueron tomadas teniendo en cuenta el plan de métricas dearrollado junto con los equipos de negocio de Ventas y Marketing.

Entre las medidas realizadas se pueden destacar las siguientes:

- GMV total y por año.
- GMV del año anterior. Diferencia absoluta y porcentual. Desarrollada mediante "médidas rápidas" de Pbi
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






### Resultados


A continuación se agrega un link para poder visualizar el tablero desarrollado para este proyecto y un link para poder descargar el archivo .pbix:

https://app.powerbi.com/reportEmbed?reportId=59c6eb9c-d471-4930-a9cc-fc43fc694d08&autoAuth=true&ctid=ca0f58d7-fbd0-461f-860d-6a78108097e0

https://1drv.ms/u/s!AlLwmtPHqjj8gf8N5a7C3BBdlqdCwA?e=4HAPo3

Por último agrego capturas del informe:

![image](https://github.com/ianziano/trabajo_final_edvai/assets/169062674/d99202a4-0d66-4c09-bab1-7edf150588a8)


![image](https://github.com/ianziano/trabajo_final_edvai/assets/169062674/090b5bd5-5c8e-4e2d-8525-4e2dba9f5cb7)


![image](https://github.com/ianziano/trabajo_final_edvai/assets/169062674/c2225c75-7ba9-4992-ad6a-36761578dd34)





