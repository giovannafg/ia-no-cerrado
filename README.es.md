<div align="center">

# 🌿 IA y Biología en el Cerrado 🍃

**Enseñando a la computadora a reconocer la salud de las plantas**

[English](README.md) · [Português](README.pt.md) · **Español**

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/giovannafg/ia-no-cerrado/blob/main/notebooks/ia_no_cerrado_folhas.ipynb)
[![Licencia: MIT](https://img.shields.io/badge/c%C3%B3digo-MIT-6e8a23.svg)](LICENSE)
[![Diapositivas: PT · EN · ES](https://img.shields.io/badge/diapositivas-PT%20%C2%B7%20EN%20%C2%B7%20ES-ffde59.svg)](#-diapositivas)
[![RIABIA-pro](https://img.shields.io/badge/RIABIA--pro-CYTED-4e621c.svg)](#-sobre-el-proyecto)

</div>

Materiales abiertos de **IA y Biología en el Cerrado** (*IA e Biologia no Cerrado*), un taller de dos horas para escuelas. Es un proyecto de la Red Iberoamericana de Prospectiva en Inteligencia Artificial para Biociencias y Biotecnología (**RIABIA-pro**, CYTED).

> 🇧🇷 **El taller se imparte en portugués.** Lo aplicaremos en escuelas públicas de la región del Cerrado, en Brasil, y por eso usa plantas del Cerrado. Las versiones en inglés y español son material de apoyo: cualquier país de la red puede adaptar el taller usando plantas de un bioma local.

## 🌱 La pregunta

> ¿Un modelo entrenado con fotos de **laboratorio** sigue acertando con imágenes de **campo**?

## 🍂 El taller

El taller está dirigido a estudiantes de educación básica (secundaria, adaptable a bachillerato) y dura 2 horas:

| Tiempo | Momento | Qué pasa |
|---|---|---|
| 0–10 min | Apertura | ¿Dónde está la IA en tu vida? |
| 10–25 min | Fundamentos de IA | Qué es la IA y cómo aprende |
| 25–45 min | Aprendizaje automático | Actividad sin computadora: detectives de hojas |
| 45–55 min | IA, biotecnología y Cerrado | Por qué esto importa para nuestra región |
| 55–70 min | Demostración | Laboratorio × campo: dónde falla el modelo |
| 70–105 min | Manos a la obra | Entrenar un modelo en [Teachable Machine](https://teachablemachine.withgoogle.com/) |
| 105–120 min | Reflexión | Sesgo, ética y quién hace ciencia |

1. **Detectives de hojas (sin computadora).** En grupos, los estudiantes separan hojas reales en sanas y con síntomas, y escriben preguntas de sí o no que expliquen la separación. Juntas, esas preguntas forman un árbol de decisión.
2. **Demostración de este notebook.** Muestra la diferencia entre fotos de laboratorio e imágenes de campo, y usa Grad-CAM para mostrar dónde mira el modelo.
3. **Práctica en Teachable Machine.** Cada grupo entrena su propio clasificador con fotos de hojas, directamente en el navegador, y luego intenta "engañarlo" cambiando el fondo o mostrando otra especie.

### 🌎 Para adaptarlo a otro país

La estructura funciona con cualquier planta. Para aplicarlo en otro lugar:

- **Hojas:** usen especies de un bioma local en *Detectives de hojas* y en la práctica en Teachable Machine. Es el cambio principal.
- **Diapositivas del Cerrado** ("¿Por qué el Cerrado?" y "Qué puede hacer la IA por el Cerrado"): reemplácenlas por el bioma local.
- **Diapositivas del experimento:** pueden quedar iguales, porque usan conjuntos de datos públicos internacionales (PlantVillage y PlantDoc).

## 📑 Diapositivas

| Idioma | PDF |
|---|---|
| 🇧🇷 Português (original) | [slides_pt.pdf](materiais/slides_pt.pdf) |
| 🇬🇧 English | [slides_en.pdf](materiais/slides_en.pdf) |
| 🇪🇸 Español | [slides_es.pdf](materiais/slides_es.pdf) |

## 🔬 El experimento

| | Entrenamiento | Prueba |
|---|---|---|
| **Modelo A** | PlantVillage (laboratorio) | PlantVillage (prueba separada) y PlantDoc `test` |
| **Modelo B** | PlantVillage + PlantDoc `train` | PlantVillage (prueba separada) y PlantDoc `test` |

- Las 38 clases originales (especie + enfermedad) se agrupan en dos: **sana** y **con síntomas**.
- El modelo es MobileNetV2 preentrenada en ImageNet (aprendizaje por transferencia), con ajuste fino de las últimas capas.
- Grad-CAM muestra qué regiones de la imagen pesaron más en la decisión.
- Semilla aleatoria: 42.

## 📊 Resultados

| Exactitud balanceada | Prueba laboratorio (n = 8146) | Prueba campo (n = 236) |
|---|---|---|
| **Modelo A** | 98,6% | 82,4% |
| **Modelo B** | 98,7% | 88,4% |

![Comparación de los modelos](figuras/06_comparacao_modelos.png)

**Hallazgos:**

1. El Modelo A pierde cerca de 16 puntos fuera del laboratorio. Esto reproduce, con datos abiertos, un efecto ya reportado por los autores de PlantDoc.
2. El error se concentra en las hojas sanas. En el campo, el Modelo A clasifica el 31% de ellas (28 de 90) como "con síntomas". El Modelo B reduce este error al 9% (8 de 90).
3. El Modelo B tiene un costo. En el campo, las hojas con síntomas clasificadas como sanas suben del 4% (6 de 146) al 14% (21 de 146). El modelo queda más equilibrado, y la exactitud balanceada sube de 82,4% a 88,4%. Pero deja escapar más hojas con síntomas.
4. En los ejemplos de Grad-CAM, el modelo mira principalmente la hoja y sus lesiones.

![Matrices de confusión del Modelo A](figuras/03_matrizes_modelo_A.png)

*Matrices de confusión del Modelo A en la prueba de laboratorio (izquierda) y de campo (derecha).*

![Grad-CAM en fotos de laboratorio](figuras/07_gradcam_laboratorio.png)

*Grad-CAM del Modelo A en fotos de laboratorio: los colores cálidos marcan las regiones que más pesaron en la decisión.*

![Grad-CAM en imágenes de campo](figuras/08_gradcam_campo.png)

*Grad-CAM del Modelo A en imágenes de campo (PlantDoc).*

Las demás figuras (muestras laboratorio × campo, curvas de entrenamiento y matrices del Modelo B) están en [`figuras/`](figuras/). Las métricas completas están en [`resultados/`](resultados/).

## ⚠️ Limitaciones

- **La prueba de campo es pequeña.** Tiene solo 236 imágenes (90 sanas y 146 con síntomas). En la exactitud balanceada, cada hoja sana vale cerca de 0,56 puntos y cada hoja con síntomas cerca de 0,34 puntos. El margen de incertidumbre es de cerca de ±4 puntos. La caída laboratorio → campo es robusta. La mejora del Modelo B (cerca de 6 puntos) debe verse como preliminar.
- **PlantDoc no es estrictamente "campo".** El conjunto de datos se armó con imágenes recolectadas de la web. Algunas son fotos de bancos de imágenes, con fondo blanco y marca de agua.
- **No son plantas del Cerrado.** Los modelos se entrenaron con plantas cultivadas, no con especies nativas del Cerrado.

## 💻 Cómo ejecutarlo

**En Google Colab (recomendado):**

1. Abran el notebook con el botón al inicio de esta página.
2. Activen la GPU en *Entorno de ejecución › Cambiar tipo de entorno de ejecución › GPU*.
3. Ejecuten las celdas en orden. Los datos se descargan automáticamente.

**En la computadora:**

```bash
pip install -r requirements.txt
jupyter notebook notebooks/ia_no_cerrado_folhas.ipynb
```

**Configuraciones principales** (primera sección del notebook):

- `MAX_POR_CLASSE_PV`: cuántas fotos usar de cada clase original de PlantVillage. `None` usa todas. Para una demostración rápida, usen `300`.
- `EPOCAS`: número máximo de épocas de entrenamiento. El entrenamiento se detiene antes si deja de mejorar. Para una demostración rápida, usen `5`.

Los resultados de arriba se obtuvieron con `MAX_POR_CLASSE_PV = None` y `EPOCAS = 8`.

La sección 10 (prueba con sus propias fotos) es opcional y se puede omitir.

El notebook está escrito en portugués.

## 📂 Estructura del repositorio

```
ia-no-cerrado/
├── README.md            (English)
├── README.pt.md         (Português)
├── README.es.md         (Español)
├── LICENSE
├── requirements.txt
├── materiais/
│   ├── slides_pt.pdf
│   ├── slides_en.pdf
│   └── slides_es.pdf
├── notebooks/
│   └── ia_no_cerrado_folhas.ipynb
├── figuras/
│   └── 01–08 *.png
└── resultados/
    ├── resultados.csv
    ├── resumo.json
    ├── historico_modelo_A.csv
    └── historico_modelo_B.csv
```

Los modelos entrenados (`.keras`) y las carpetas `dados/` y `minhas_fotos/` los genera el notebook y no se versionan.

## 🗂️ Datos y licencias

| Conjunto de datos | Contenido | Licencia | Referencia |
|---|---|---|---|
| [PlantVillage](https://github.com/spMohanty/PlantVillage-Dataset) | ~54 mil fotos de hojas sobre fondo uniforme, 14 especies | CC BY-SA 3.0 | Hughes, D. P.; Salathé, M. *An open access repository of images on plant health to enable the development of mobile disease diagnostics.* arXiv:1511.08060, 2015. |
| [PlantDoc](https://github.com/pratikkayal/PlantDoc-Dataset) | ~2,6 mil fotos de hojas en condiciones reales, 13 especies | CC BY 4.0 | Singh, D.; Jain, N.; Jain, P.; Kayal, P.; Kumawat, S.; Batra, N. *PlantDoc: A Dataset for Visual Plant Disease Detection.* CoDS-COMAD 2020. |

Las imágenes **no** se redistribuyen en este repositorio: el notebook descarga cada conjunto de datos de su fuente original.

Los autores de PlantVillage piden que los modelos entrenados con sus datos también se compartan bajo CC BY-SA 3.0. Por eso, los modelos generados por el notebook siguen esa licencia.

## 🌻 Sobre el proyecto

Este taller es un proyecto de la RIABIA-pro. Su foco es mostrar a los estudiantes dónde fallan los modelos de IA fuera de condiciones controladas, y hacer visible el sesgo en los datos. Estamos preparando un resumen y un póster sobre el taller para presentarlos a un evento científico. El taller todavía no se ha aplicado en las escuelas.

## 🌾 Próximos pasos

- Aplicar el taller en escuelas públicas de la región del Cerrado.
- Validar los modelos con bancos de imágenes de plantas brasileñas (Digipathos, Embrapa).
- Repetir el experimento con varias semillas aleatorias.

## 📜 Licencia del código

El código de este repositorio está bajo la licencia MIT (ver [`LICENSE`](LICENSE)).

## 🤝 Créditos

Equipo del taller: Ana Sarah, Giovanna y Mariana.

Giovanna Felipe, Facultad de Ciencias y Tecnologías en Ingeniería (FCTE), Universidad de Brasilia.

RIABIA-pro / CYTED.
