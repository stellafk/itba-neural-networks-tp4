# CNN en Skin Dataset

**TP4 - Redes Neuronales (ITBA).** Clasificación de lesiones de piel con redes convolucionales (dataset ISIC, 9 clases, ~880 imágenes).

**Duración estimada: 2 horas.** El código viene resuelto en un notebook; el trabajo del TP es ejecutarlo, entender el código convolucional y **analizar** los resultados.

## Contenido del repo
- `CNN_en_Skin_Dataset.ipynb`: notebook de referencia con el código resuelto y las celdas **✏️** a completar.
- `data/Split_smol/`: imágenes, con `train/` y `val/` y una subcarpeta por clase. Es el dataset tal como se distribuye: **incluye imágenes duplicadas** y **no tiene test**; el notebook se encarga de eso.
- `requirements.txt`: dependencias.

## Cómo correrlo
```bash
python -m venv .venv && source .venv/bin/activate    # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook CNN_en_Skin_Dataset.ipynb
```
Ejecutar el notebook desde la carpeta que contiene `data/`. En Colab, subir o clonar el repo y ejecutar desde esa carpeta. Con GPU corre más rápido; en CPU tarda unos 15-20 minutos (medido en una CPU de 20 núcleos).

---

## Contexto
En los TP anteriores trabajaron con texto (20 Newsgroups): TF-IDF + softmax y MLP. Acá la entrada son **imágenes dermatoscópicas** (ISIC). Una MLP trata cada píxel como una variable independiente y necesita una cantidad enorme de parámetros. Una **red convolucional** aprovecha la estructura espacial de la imagen con filtros que se comparten a lo largo de toda la imagen.

## Qué hay que hacer
1. **Ejecutar el notebook de punta a punta.** El cómputo está calibrado para ~15-20 minutos en CPU. El pipeline, los modelos, el entrenamiento con early stopping y los experimentos ya están implementados.
2. Completar las celdas **✏️**, que son el trabajo del TP:
   - **EDA:** observaciones sobre desbalance, resoluciones, color por clase y duplicados.
   - **Explicá el código convolucional:** explicar, verificando con las formas y parámetros de la tabla, `padding=kernel // 2`, `MaxPool2d`, el cálculo de parámetros de una convolución y de la capa `Linear`, `bias=not use_bn`, el duplicado de canales por bloque y el campo receptivo.
   - **Análisis 1 – De la MLP a la CNN:** parámetros, accuracy, brecha, tiempos y robustez a traslaciones.
   - **Análisis 2 – Arquitectura convolucional:** profundidad, kernel, filtros y resolución de entrada; campo receptivo; qué diferencias superan el ruido entre semillas.
   - **Análisis 3 – Overfitting y data augmentation:** demostrar el overfitting con números, efecto de cada nivel de augmentation, qué transformaciones tienen sentido en dermatología y por qué se aumenta solo train y después del split.
   - **Análisis 4 – Resultados finales e interpretación:** consistencia val vs. test, CNN vs. MLP, matriz de confusión y métrica relevante, filtros y mapas de activación, sesgos y limitaciones.
3. Completar la **declaración de uso de IA**.

Pueden modificar las constantes de la sección 0 (semillas, épocas, tamaños de imagen, etc.), pero deben **justificar y registrar** cualquier cambio en su análisis.

## Qué se trabaja y qué no
- **Se trabaja:** análisis exploratorio, limpieza de duplicados, split estratificado, convoluciones y pooling, campo receptivo, MLP vs. CNN, resolución de entrada, data augmentation hecha a mano (sin librerías) y evaluación final.
- **No se repite** (ya lo hicieron en los TP1 a TP3): búsqueda de hiperparámetros con Optuna, dropout/L2/BatchNorm como variables de estudio, inicialización y logging con TensorBoard/MLflow.

## Reglas metodológicas
- Las entradas van **normalizadas**: media y desvío por canal calculados **solo con train**.
- El dataset trae **imágenes duplicadas entre `train/` y `val/`** y no trae test: el notebook elimina los duplicados y vuelve a dividir en train/val/test de forma **estratificada** y con semilla. No cambien la semilla del split.
- La **augmentation se aplica solo a train y después del split**. Val y test nunca se aumentan.
- El **test se usa una sola vez**, al final. Todo lo que se decida (arquitectura, augmentation, qué modelo) se decide con validación.
- Una diferencia entre configuraciones solo se considera real si supera el **ruido entre semillas** (las tablas reportan media ± desvío de 3 semillas) y el error estándar de la validación/test (pocas imágenes).
- Las conclusiones tienen que **coincidir con los outputs** de su corrida. Si cambian algo y vuelven a ejecutar, revisen sus textos.
- Los textos deben ser propios y estar respaldados por **sus** números, gráficos y tablas.

## Entrega
- Un **repositorio propio** con el notebook **ejecutado (con outputs)**, un `README` que explique cómo reproducirlo y un `requirements.txt`. No subir el dataset ni modelos entrenados. Entregar el link al repositorio; no se aceptan archivos sueltos.
- Si usaron herramientas de IA, declararlo en el notebook (hay una celda para eso). Se puede pedir defensa oral.

## Criterios de evaluación (resumen)
- Notebook ejecutado completo, con las constantes y cambios documentados.
- Explicación correcta del código convolucional, con números verificados.
- Análisis que **explique el porqué** de lo observado, con números propios y coherentes con los outputs.
- Metodología: normalización con estadísticas de train, split sin duplicados, augmentation después del split, validación para decidir, test una sola vez, ruido entre semillas.
- Honestidad sobre las limitaciones (dataset chico, una sola partición, sesgos del dataset).
