# 📶 Predicción de Churn – Interconnect

La operadora de telecomunicaciones **Interconnect** desea anticipar qué clientes cancelarán su servicio (churn) para tomar acciones preventivas. Este proyecto implementa un flujo de trabajo completo de Machine Learning para clasificar usuarios según probabilidad de abandono.

## 🎯 Objetivos

- Unir y depurar los registros de clientes, contratos y servicios (Internet, teléfono).  
- Generar nuevas variables que aporten señal (p. ej. uso de Internet, líneas múltiples).  
- Balancear el dataset descompensado (ratio ≈ 3:1 negativo/positivo).  
- Comparar varios modelos de clasificación y seleccionar el óptimo.  
- Optimizar hiperparámetros y reducir características con RFE.  
- Evaluar el rendimiento final en un conjunto de prueba independiente.

## 🔍 Flujo de Trabajo

1. **Análisis Exploratorio de Datos (EDA)**  
   - Revisión de medias de duración de contrato (activos vs. churn).  
   - Inspección de desequilibrio de clases.  
   - Correlación entre variables y con la variable objetivo.

2. **Preparación de Datos**  
   - **Unión de tablas:** clientes, contratos, Internet y teléfono.  
   - **Eliminación de columnas irrelevantes:** IDs, fechas de corte, etc.  
   - **Creación de variables:**  
     - `Internet` (sí/no), `Phone` (líneas múltiples / fibra).  
     - Duración en días del contrato.  
     - Etiqueta binaria `Churn`.  
   - **Codificación:** One-Hot Encoding de variables categóricas.  
   - **Escalado:** Min-Max Scaler de todas las variables numéricas (rango 0–1).  
   - **Balanceo:** SMOTE para corregir el desequilibrio de clases.

3. **División de Conjuntos**  
   - Split en **entrenamiento**, **validación** y **prueba**.

4. **Entrenamiento y Ajuste de Modelos**  
   - Modelos probados:  
     - **XGBoost**  
     - **Random Forest**  
   - Hiperparámetros optimizados vía búsqueda manual (GridSearch exploratorio).  
   - Selección de características con **Recursive Feature Elimination (RFE)** sobre Random Forest.

5. **Evaluación**  
   - Métrica principal: **AUC-ROC** (objetivo ≥ 0.88 en validación).  
   - Métricas secundarias: **Precisión**, **Recall**, **F1-score**.  
   - Comparación frente a baseline (modelo dummy).

## 📊 Resultados Principales

- En validación se alcanzó **AUC-ROC ≈ 0.88** con ambos modelos (XGBoost y Random Forest).  
- Tras RFE, Random Forest mantuvo rendimiento similar con menos variables.  
- En conjunto de prueba, el modelo final (Random Forest) obtuvo:  
  - **Exactitud (Accuracy):** 0.77  
  - **Precisión (Precision):** 0.73  
  - **Recall:** 0.77  
  - **F1-score:** 0.74  
## 🏁 Conclusiones y Recomendaciones

- El modelo **Random Forest** seleccionado cumple con el objetivo de AUC-ROC ≥ 0.88 y muestra un buen equilibrio entre precisión y recall.  
- La reducción de variables mediante RFE no deteriora el rendimiento, facilitando implementaciones más sencillas.  
- El balanceo de clases con SMOTE resultó clave para evitar sesgos en la clasificación.
- **Despliegue en Producción:** Integrar el modelo en el sistema de CRM para generar alertas tempranas de churn.  
- **Monitoreo Proactivo de Antigüedad**: Implementar alertas automáticas para clientes con `tenure_days` bajos, ya que suelen presentar mayor riesgo de churn.
- **Ofertas Personalizadas en Cargos**: Analizar planes con altos `MonthlyCharges` y diseñar promociones para reducir la percepción de costo.
- **Revisión de Facturación Total**: Detectar clientes con `TotalCharges` muy elevados y ofrecer esquemas de pago fraccionado o descuentos por fidelidad.
- **Engagement de Servicios**: Incentivar el uso de Internet (`InternetService_Yes`) con paquetes de datos adicionales o mejoras de velocidad.
- **Optimización de Planes de Líneas Múltiples**: Revisar planes de `MultipleLines_Yes` para asegurar que ofrecen valor agregado y mejorar la retención.

## 🚀 Cómo Clonar y Preparar el Entorno

```bash
git clone https://github.com/dsc530/tasa_cancelacion.git
