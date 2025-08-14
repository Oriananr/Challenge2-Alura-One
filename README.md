# Challenge2-Alura-One
Desafio 2 de la Beca ALURA LATAM
**Challenge Telecom X: Análisis de evasión de clientes. Parte I**

Programa de formación Oracle ONE / 8 (Oracle Next Education) - Especialización en Data Science, patrocinado por Oracle y Alura Latam.



Fecha:13-08-2025

Nombre de la alumna: Oriana Ortiz.

# 1.Introducción.

Dentro del programa Oracle ONE (Oracle Next Education) - Alura Latam, la realización de una serie de Challenges se contempla como una parte fundamental de la formación de los becarios. Esta metodología de aprendizaje, desarrollada por Apple, se basa en exponer a los alumnos a situaciones reales y complejas.
El presente Challenge, corresponde al segundo de la serie y fue desarrollado en este notebook, el cual presenta un análisis exploratorio y de limpieza de datos para comprender los factores que influyen en el abandono de clientes (Churn) en la empresa de telecomunicaciones Telecom X. 

El objetivo principal es identificar patrones y características de los clientes que deciden dejar la empresa, con el fin de desarrollar estrategias de retención más efectivas.

A través de este análisis, se abordarán las siguientes etapas:
   - Extracción de datos: carga del dataset de clientes.
   - Transformación de datos: limpieza, manejo de valores ausentes, duplicados, inconsistencias, y creación de nuevas variables.
   - Análisis descriptivo y visualización: exploración de la distribución de las variables y relaciones entre ellas, con un enfoque en la variable Churn.
   - Identificación de segmentos clave: análisis de grupos de clientes con alta propensión al Churn.
     Preparación para Modelado: Conclusiones sobre la necesidad de técnicas más avanzadas (Machine Learning) para predecir el Churn.
     
Este trabajo sienta las bases para futuros análisis predictivos y la implementación de modelos que permitan anticipar el comportamiento de los clientes y reducir la tasa de abandono.


# 2. Metodología.

**2.1. Descripción de los Datos y Fuentes.**

El análisis se basa en el conjunto de datos TelecomX_Data.json, una fuente pública obtenida del repositorio de GitHub: 

   https://github.com/alura-cursos/challenge2-data-science-LATAM/blob/main/TelecomX_Data.jason
   
Este conjunto de datos contiene información anonimizada de 7,267 clientes de una empresa de telecomunicaciones y su comportamiento de consumo. Los datos iniciales estaban estructurados en un formato JSON anidado, lo que requirió un pre-procesamiento exhaustivo.

**2.2. Metadata y descripción de variables utilizadas.** 

El análisis utilizó las siguientes variables, categorizadas por su tipo de dato:
   Variable dependiente:
   - Churn (Categórica binaria): variable objetivo que indica si un cliente abandona ('Yes') o no ('No') la empresa.

   Variables independientes (Explicativas):
   
   - customer_gender (Categórica): Género del cliente ('Female', 'Male').
   - customer_SeniorCitizen (Categórica): Indica si el cliente es de la tercera edad ('Yes', 'No').
   - customer_Partner (Categórica): Indica si el cliente tiene pareja ('Yes', 'No').
   - customer_Dependents (Categórica): Indica si el cliente tiene dependientes ('Yes', 'No').
   - customer_tenure (Numérica): Antigüedad del cliente en meses.
   - phone_PhoneService (Categórica): Indica si el cliente tiene servicio telefónico ('Yes', 'No').
   - phone_MultipleLines (Categórica): Indica si el cliente tiene múltiples líneas telefónicas ('Yes', 'No', 'No phone service').
   - internet_InternetService (Categórica): Tipo de servicio de internet ('DSL', 'Fiber optic', 'No').
   - internet_OnlineSecurity (Categórica): Estado de la suscripción de seguridad en línea ('Yes', 'No', 'No internet service').
   - internet_OnlineBackup (Categórica): Estado de la suscripción de respaldo en línea ('Yes', 'No', 'No internet service').
   - internet_DeviceProtection (Categórica): Estado de la suscripción de protección del dispositivo ('Yes', 'No', 'No internet service').
   - internet_TechSupport (Categórica): Estado de la suscripción de soporte técnico ('Yes', 'No', 'No internet service').
   - internet_StreamingTV (Categórica): Estado de la suscripción de televisión por streaming ('Yes', 'No', 'No internet service'). 
   - internet_StreamingMovies (Categórica): Estado de la suscripción de películas por streaming ('Yes', 'No', 'No internet service').
   - account_Contract (Categórica): Tipo de contrato del cliente ('Month-to-month', 'One year', 'Two year').
   - account_PaperlessBilling (Categórica): Indica si el cliente utiliza facturación sin papel ('Yes', 'No').
   - account_PaymentMethod (Categórica): Método de pago utilizado.
   - account_Charges_Monthly (Numérica): Cargo mensual total.
   - account_Charges_Total (Numérica): Cargo total acumulado.
   - Cuentas_Diarias (Numérica): Cargo diario calculado a partir del cargo mensual.
    
El DataFrame original estaba compuesto por 7267 filas y 22 columnas. De las 22 columnas 20, corresponden a variables explicativas.

**2.3 Pre-procesamiento y Normalización.**

La extracción de los datos se realizó mediante la carga el dataset TelecomX_Data.json desde una URL en un DataFrame.

Para preparar los datos para el análisis, se realizaron los siguientes pasos:

   - Aplanamiento de la estructura JSON: las columnas anidadas en formato de diccionario (customer, phone, internet, account) fueron normalizadas utilizando la función pd.json_normalize() de la librería Pandas. Este proceso aplanó la estructura de los diccionarios, convirtiendo cada clave en una nueva columna, por ejemplo, 'customer' se aplanó en 'customer_gender', 'customer_tenure', etc.
   - Limpieza de nombres de columnas: se renombraron las columnas que contenían caracteres especiales como puntos (.), que podrían generar errores en el acceso a los datos. Por ejemplo, 'account_Charges.Monthly' fue renombrada como 'account_Charges_Monthly'.
     
**2.4. Proceso de Análisis y Limpieza de Datos.**

Para asegurar la consistencia y operatividad de los datos, se realizaron las siguientes transformaciones y limpiezas:

   - Revisión de valores en el DataFrame: Se buscó valores nulos, duplicados y en blanco. Los registros vacíos encontrados en las variables 'Churn' y  'account_Charges_Total' fueron extraidos y se guardó esta información en dos archivos de tipo CSV, ya que podrían resultar útiles en un siguiente proceso.
     
   - Detección de outliers en las variables numéricas.
       
   - Corrección de inconsistencias lógicas: Se revisaron incoherencias en las variables categóricas. Por ejemplo, se validó que los clientes sin servicio de internet ('internet_InternetService' = 'No') tuvieran 'No internet service' en todas las columnas de servicios adicionales, asegurando la integridad del conjunto de datos.
     
   - Se creó la columna Cuentas_Diarias a partir de account_Charges_Monthly para una visualización más granular del gasto del cliente.

Posteriormente, se realizó un análisis exploratorio y descriptivo para entender la distribución y las relaciones de las variables:

   - Se examinaron estadísticas básicas y la frecuencia de las variables numéricas y categóricas.
     
   - Mediante el uso de diversos tipos de gráficos, se identificó un segmento de alto riesgo de abandono (Churn), localizando este segmento en la facturación mensual entre $70 y $100 USD.
   
   - Se analizó la composición de los servicios de teléfono e internet para los clientes dentro de este rango, diferenciando entre clientes que se quedaron (No Churn) y los que se fueron (Yes Churn). También se determinó la cantidad y proporción de clientes Churn en este segmento y su relación con el servicio de Fibra Óptica. Además, se caracterizaron los servicios adicionales de internet contratados por los clientes Churn.
     
   - Se exploran las relaciones entre las variables numéricas y el Churn utilizando un gráfico pairplot.
     
   - Se exploraron las relaciones entre las variables numéricas y el Churn a través de un mapa de calor de correlación.
     
**2.5. Software utilizado**

Se empleó el lenguaje de programación Python (Versión 3.11.13) en un entorno de desarrollo integrado (IDE, Google Colaboratory), que proporcionó las herramientas necesarias para la codificación, ejecución y depuración de scripts. 

Las siguientes librerías de Python fueron utilizadas en cada etapa del proceso:
   - Pandas: Se utilizó para la manipulación y análisis de datos. Fue clave para la carga del archivo JSON, la normalización de las estructuras anidadas y la limpieza del dataset.
   - NumPy: Sirvió de base para las operaciones numéricas y matemáticas de alto rendimiento, especialmente en el cálculo de métricas y la preparación de los datos para la modelización.
   - Matplotlib: Se empleó para la creación de gráficos estáticos, como el boxplot y los subplots, permitiendo la visualización de la distribución de las variables.
   - Seaborn: Se utilizó para generar visualizaciones estadísticas más sofisticadas, como el mapa de calor de correlación y los gráficos de dispersión (pairplot), facilitando la exploración de las relaciones entre las variables.
     
El desarrollo de este trabajo fue asistido por IA Gemini Flash, versión 1.5 Pro. Esta colaboración permitió una implementación ágil y eficiente de las tareas de análisis de datos, facilitando la exploración de hipótesis y la toma de decisiones informadas sobre la dirección del proyecto.




# 3. Resultados.

El análisis de los datos se llevó a cabo en dos etapas: la preparación de datos y el análisis exploratorio, cuyos resultados se detallan a continuación.

**3.1. Preparación de Datos y Calidad**

La fase inicial de preparación y limpieza del conjunto de datos TelecomX_Data.json permitió obtener un DataFrame consolidado y de alta calidad para el análisis. Se destacan los siguientes hallazgos en la calidad de los datos:
   - Valores faltantes y duplicados: No se encontraron registros con valores nulos o duplicados en el conjunto de datos, lo que facilitó el proceso de análisis sin la necesidad de imputación o eliminación masiva de datos.
   - Inconsistencias lógicas: Se identificaron y corrigieron inconsistencias lógicas en variables categóricas, principalmente en la relación entre los servicios de telefonía e internet. Este proceso aseguró la coherencia de los datos, garantizando que, por ejemplo, los clientes sin servicio de internet no tuvieran contratados servicios adicionales de streaming o seguridad.
   - Valores atípicos: Un análisis inicial de los boxplots de las variables numéricas clave (account_Charges_Monthly, account_Charges_Total, customer_tenure) no reveló la presencia de valores atípicos significativos que pudieran distorsionar los resultados, lo que permitió proceder sin la necesidad de tratamientos específicos de outliers.
     
La depuración del DataFrame resultó en un conjunto de datos limpio y robusto, adecuado para los análisis descriptivos y preparatorio para una eventual aplicación de modelos de Machine Learning.

**3.2. Hallazgos del Análisis Descriptivo**

El análisis exploratorio de los datos reveló una serie de patrones clave relacionados con el abandono de clientes (Churn):

   - Segmento de alto riesgo: Se identificó que un segmento específico de clientes representa la mayoría de los casos de Churn. Aproximadamente el 52.43% del total de clientes que abandonaron el servicio tenían una facturación mensual entre $70 y $100 USD. Este grupo de alto riesgo se caracteriza por tener una tasa de Churn desproporcionadamente alta en comparación con otros segmentos.

   - Correlación con el servicio de Fibra Óptica: Dentro del segmento de alto riesgo ($70 - $100 USD), se encontró una fuerte relación entre la contratación de fibra óptica y la propensión al Churn, con un 96% de los clientes que abandonaron teniendo este servicio. Esto sugiere que, a pesar de ser un servicio de alta velocidad, su coste o experiencia de uso en este rango de precios podría estar directamente relacionado con la decisión de los clientes de irse.

   - Patrones en servicios adicionales: La exploración de los servicios adicionales de internet para este segmento reveló que, si bien muchos clientes de alto riesgo de Churn carecían de suscripciones de seguridad en línea, respaldo o soporte técnico, sí existía una proporción notable que mantenía servicios de streaming (TV y películas).

Estos hallazgos demuestran la utilidad del análisis descriptivo para identificar segmentos problemáticos y relaciones directas. Sin embargo, para desentrañar la influencia de las 21 variables explicativas en el Churn restante, se justifica la necesidad de emplear técnicas de Machine Learning, tal como se plantea en las conclusiones.

# 4. Conclusiones del Análisis Descriptivo

A través de un análisis descriptivo detallado, fue posible identificar patrones significativos en el comportamiento de los clientes y su relación con el Churn. Los hallazgos más relevantes son los siguientes:

   - Identificación de un segmento de riesgo clave: Un hallazgo destacado es que aproximadamente el  52.43% de los clientes que abandonan la empresa (Churn) se concentra en un rango de facturación mensual entre $70 y $100 USD. Este comportamiento se observa de manera específica en los clientes que han contratado el servicio de fibra óptica. Este segmento representa una clara área de enfoque para futuras acciones de retención.

   - Insuficiencia del análisis descriptivo: Aunque se han identificado estas relaciones, el análisis descriptivo presenta limitaciones para explorar la complejidad completa del problema. Con un total de 21 variables explicativas, la interacción de múltiples factores que influyen en el Churn de los clientes restantes es demasiado compleja para ser abordada únicamente con herramientas de visualización y análisis de una o dos variables.

   - Necesidad de un enfoque más avanzado: La incapacidad de nuestro análisis actual para explicar el comportamiento del 50% restante de los clientes que abandonan la empresa, subraya la necesidad de un enfoque más robusto. Para desentrañar las relaciones multifactoriales y descubrir patrones ocultos, es fundamental avanzar hacia herramientas de Machine Learning.

En resumen, el análisis descriptivo ha proporcionado una comprensión básica y ha validado un segmento de clientes en riesgo, pero es insuficiente para una comprensión completa del problema. Por lo tanto, se concluye el análisis descriptivo y se propone continuar la investigación con modelos de Machine Learning, que permitirán identificar las variables explicativas más importantes y construir un modelo predictivo robusto para la retención de clientes.
