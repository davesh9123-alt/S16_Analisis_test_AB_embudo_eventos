# Proyecto: Nuevo embudo de pago - Analisis de pruebas A/B

Tenemos una tarea analítica para una tienda en línea internacional. Nuestros predecesores no consiguieron completarla: lanzaron una prueba A/B y luego abandonaron (para iniciar una granja de sandías en Brasil). Solo nos dejaron las especificaciones técnicas y los resultados de las pruebas.

**Descripción técnica**

- Nombre de la prueba: `recommender_system_test`
- Grupos: А (control), B (nuevo embudo de pago)
- Fecha de lanzamiento: 2020-12-07
- Fecha en la que dejaron de aceptar nuevos usuarios: 2020-12-21
- Fecha de finalización: 2021-01-01
- Audiencia: 15% de los nuevos usuarios de la región de la UE
- Número previsto de participantes de la prueba: 6 000

**Propósito de la prueba**: probar cambios relacionados con la introducción de un sistema de recomendaciones mejorado
**Resultado esperado**: dentro de los 14 días posteriores a la inscripción, los usuarios mostrarán una mejor conversión en vistas de la página del producto (el evento `product_page`), instancias de agregar artículos al carrito de compras (`product_cart`) y compras (`purchase`). En cada etapa del embudo `product_page → product_cart → purchase`, habrá **al menos un 10% de aumento**.

## Habilidades Tecnológicas Utilizadas

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=for-the-badge&logo=scipy&logoColor=%white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75.svg?style=for-the-badge&logo=Plotly&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Markdown](https://img.shields.io/badge/markdown-%23000000.svg?style=for-the-badge&logo=markdown&logoColor=white)
![Postgresql](https://img.shields.io/badge/PostgreSQL-4169E1.svg?style=for-the-badge&logo=PostgreSQL&logoColor=white)
![Power Bi](https://img.shields.io/badge/power_bi-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Microsoft Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-%2334A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)
![Claude](https://img.shields.io/badge/Claude-D97757?style=for-the-badge&logo=claude&logoColor=white)
![GitHub Copilot](https://img.shields.io/badge/github_copilot-8957E5?style=for-the-badge&logo=github-copilot&logoColor=white)
![Google Gemini](https://img.shields.io/badge/google%20gemini-8E75B2?style=for-the-badge&logo=google%20gemini&logoColor=white)


## Etapas

- Estudiar la conversion en diferentes etapas del embudo
- Distribucion de eventos por usuario
- Distribucion de eventos por dia
- Evaluacion de resultados de la prueba A/B


## Estudiar la conversion en diferentes etapas del embudo

Las conversiones de ambos grupos son muy similares, sin embargo la conversion de grupo A (el grupo de control) es ligaramente mejor con un 34.07% de conversion en la etapa de compra, mientras que el grupo B se queda un poco por debajo con 32.37% de conversion. Estos resultados no son muy favorables para el nuevo embudo de pago.

<img width="1247" height="699" alt="embudo_gruposAB_output" src="https://github.com/user-attachments/assets/3c7e99f6-1876-4300-b8dc-04dfe25debae" />

<img width="1238" height="699" alt="Conversion_gruposAB_output" src="https://github.com/user-attachments/assets/d1b6f52b-b9cb-4b1f-ba86-54b2509bbc95" />


## Distribucion de eventos por usuario

Las medias del grupo A y B son relativamente similares (`medias: A=7.46 y B=7.11`). Sin embargo la grafica de distribucion de eventos por usuario muestra diferencias notables pese a que la forma o proporcion de las distribuciones es similar. Finalmente la prueba nos arroga un resultado algo inesperado. El valor P es significativamente inferior a `alpha=0.05` lo que quiere decir que los grupos no estan equilibrados. Debemos seguir explorando los datos para encontrar el motivo de desequilibrio.

<img width="1790" height="490" alt="eventos_usuario_output" src="https://github.com/user-attachments/assets/a4b657b2-f46b-4859-999d-734bf8563e52" />


## Distribucion de eventos por dia

El numero de eventos que suceden por dia nos da infomacion sobre la actividad de los usuarios durante el periodo estudiado. El registro comienza el 2020-12-07 y termina el 2020-12-30. El periodo de actividad mas alto fue del 7 de diciembre al 21 de diciembre, esto es probablemente debido a que durante estas fechas el experimento estuvo registrando nuevos usuarios. El 21 de diciembre el registro de nuevos usuarios finalizo. A partir de esa fecha vemos una caida gradual en el numero de eventos, lo que indica que la actividad de los usuarios fue dismunuyendo.

<img width="1247" height="545" alt="eventos_por_dia_output" src="https://github.com/user-attachments/assets/b281cbaa-bbec-415e-8a3e-f8d86fbe74b4" />


## Evaluacion de resultados de la prueba A/B

Anteriormente durante el analisis exploratorio descubrimos que las muestras de los grupos A y B no estan distribuidos equilibradamente, lo cual simplemente pudo ser un error de distribucion. Sin embargo despues detectamos que hay usuarios que estan en ambos grupos, lo cual crea un problema muy grave ya que contamina la validez de la prueba. Entonces, para tratar de solucionar ese problema pensamos que la mejor solucion es eliminar los registros de esos usuarios duplicados, con el fin de tener datos experimentales lo mas limpios posible.

## Conversion con datos limpios

<img width="1238" height="699" alt="conversion_datos_limpios_output" src="https://github.com/user-attachments/assets/1537b8ab-a05c-4bbe-a721-e9800e21194d" />

## Conclusiones

El proposito de la prueba era probar los cambios relacionados con la introduccion de un sistema de recomendaciones mejorado. Se esperaban resultados de al menos un 10 % en cada etapa del embudo. Sin embargo las pruebas estadisticas no mostraron mejoras significativas. Tambien es probable que los datos de las pruebas se hayan comprometido su validez pues encontramos usuarios que experimentaron ambos versiones, es decir, la nueva version del sistema de recomendacion y la version anterior. Aunque los usuarios de la prueba superaban los 6,000 participantes en ambos grupos y los usuarios que estaban en ambos grupos solo era 441, quiza porcion fue suficiente para alterar los resultados en el periodo de tiempo que se tomaron las pruebas. De cualquier forma lo que podemos asegurar es que la mejorar esperada en las tasas de conversion no fue alcanzada.

**Conclusiones**:

- Las evaluaciones iniciales mostraron un desequilibrio en la distribucion de las muestras. El grupo A tuvo 7873 participantes, mientras que el grupo B 6206 participantes.
- Las pruebas iniciales no mostraron una diferencia significativa favorable para el grupo B (el grupo de prueba), y de hecho el grupo A (grupo de control) mostro una ligera ventaja en la conversion de compra (un 1.7%).
- Descubrimos que habia usuarios en ambos grupos (441 usuarios) lo cual pudo haber sesgado o comprometido la validez de la prueba.
- Eliminamos los usuarios duplicados de ambos grupos para tener datos limpios en ambos grupos.
- Volvimos a hacer comparaciones y pruebas con los datos "limpios", pero encontramos resultados similares.
- Los resultados no mostraron una mejora significativa a favor de grupo B.
- En la etapa de compra, de hecho el grupo A tuvo un liegro mejor desempeño que el grupo B, un 1.74%, muy cerca de 2% mejor.
- Las tasas de conversion predichas de un aumento de 10% no fueron alcanzadas.
- En general no hubo una diferencia significativa notable, pero esto pudo deberse a los errores en la distribucion de participantes en la prueba.
