# Proyecto productividad de pozos no convencionales Argentina
#Proyecto final de EnergIA Digital - Data Science (Fundación YPF) Factores, predicción y recomendaciones
Dataset

Los datos son de la Secretaría de Energía de la Nación y se publican en el portal de datos abiertos Datos Energía (datos.energia.gob.ar): dataset "Producción de petróleo y gas por pozo (Capítulo IV)", archivo "Producción de Pozos de Gas y Petróleo No Convencional". Tiene 426.145 registros mensuales de 5.126 pozos y 55 empresas, de enero de 2006 a agosto de 2026. Unidades: petróleo y agua en m3, gas en miles de m3. El archivo original pesa 147 MB y no entra en GitHub; en este repositorio se incluye una versión comprimida (.csv.gz) con las mismas filas y columnas.
Resumen del proyecto

Analizamos la producción de los pozos no convencionales (shale y tight) de Argentina, donde Vaca Muerta concentra casi toda la actividad. Exploramos y limpiamos los datos, construimos modelos supervisados para predecir la productividad de un pozo y un modelo no supervisado para agrupar pozos con comportamientos parecidos.
Objetivo

Entender qué factores explican que un pozo petrolífero no convencional sea de alta o baja producción y construir un modelo que prediga su productividad inicial, para orientar recomendaciones sobre qué tipo de pozos y zonas rinden más. La variable a predecir es alta_prod: vale 1 si el petróleo de los primeros 12 meses supera la mediana de todos los pozos petrolíferos.
Qué descubrimos en el análisis exploratorio

La producción de petróleo pasó de 15.402 en 2006 a 29,4 millones en 2025, con un salto desde 2014. Está muy concentrada: Neuquén aporta el 97,6% del petróleo, Vaca Muerta el 96,5% y YPF el 58%. El shale explica el 96,6% del petróleo. La profundidad se correlaciona con la producción de los primeros 12 meses (0,65) y los pozos más recientes arrancan con más producción. Los datos tenían 3 valores negativos, columnas casi vacías, nulos en tipo de pozo y de recurso, y profundidades en 0 o imposibles; todo se limpió y está documentado en el notebook.
Modelos ajustados

Supervisado (clasificación y regresión). Con 1.988 pozos petrolíferos con al menos 12 meses de historia, comparamos un clasificador base (Dummy), regresión logística y Random Forest con validación cruzada estratificada de 5 pliegues, y optimizamos el Random Forest con búsqueda aleatoria de hiperparámetros (usando AUC como métrica), dividiendo en 80% de entrenamiento y 20% de test. El Random Forest optimizado obtuvo en test un AUC de 0,93, un accuracy de 0,86 y un recall de 0,92 para la clase de alta productividad, con un leve overfitting (AUC de 0,98 en entrenamiento). Las variables más importantes son la profundidad, el año de inicio y la ubicación. En regresión, el Random Forest explica el 65% de la variación del petróleo a 12 meses (R2 = 0,65).

No supervisado (clustering). Con K-Means sobre 2.511 pozos (profundidad, año de inicio y producción de los primeros 12 meses, con logaritmo y escalado) elegimos 3 grupos con un criterio explícito: mayor silhouette (0,45 al quitar las coordenadas), sin grupos menores al 3% y desempate por simplicidad. Comparamos con clustering jerárquico y mezcla gaussiana, y verificamos la estabilidad con 10 semillas. Los grupos son: pozos petroleros modernos y profundos (43%), pozos más antiguos con perfil de gas (47%) y pozos sin producción en el primer año (10%). Al compararlos con el tipo de pozo, que no se usó para agrupar, el índice Rand ajustado es 0,345.
Contenido del repositorio
exploracion_pozos_no_convencionales.ipynb: análisis exploratorio (2ª pre-entrega).
Proyecto_productividad_de_pozos_no_convencionales_Argentina.ipynb: modelos de clasificación y regresión (3ª pre-entrega).
clustering_pozos_no_convencionales.ipynb: modelo de clustering (4ª pre-entrega).
pozos_no_convencionales.csv.gz: dataset utilizado (426.145 filas y 40 columnas).
Limitaciones

Son pocos pozos para modelar y casi todos de Vaca Muerta. El conjunto de test es chico (398 pozos), por lo que las diferencias de pocos puntos entre modelos no son concluyentes. No hay datos de diseño del pozo (longitud lateral, etapas de fractura), que probablemente expliquen parte de la productividad. Algunos pozos tienen profundidades o coordenadas atípicas, probablemente errores de carga.

Integrantes

Valeria Villarpando y Rogelio Sandoval
