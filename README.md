<div align="center">

# 🌿 AI and Biology in the Cerrado 🍃

**Teaching a computer to recognize plant health**

**English** · [Português](README.pt.md) · [Español](README.es.md)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/giovannafg/ia-no-cerrado/blob/main/notebooks/ia_no_cerrado_folhas.ipynb)
[![License: MIT](https://img.shields.io/badge/code-MIT-6e8a23.svg)](LICENSE)
[![Slides: PT · EN · ES](https://img.shields.io/badge/slides-PT%20%C2%B7%20EN%20%C2%B7%20ES-ffde59.svg)](#-slides)
[![RIABIA-pro](https://img.shields.io/badge/RIABIA--pro-CYTED-4e621c.svg)](#-about-the-project)

</div>

Open materials for **AI and Biology in the Cerrado** (*IA e Biologia no Cerrado*), a two-hour workshop for schools. It is a project of the Ibero-American Network for Foresight in Artificial Intelligence for Biosciences and Biotechnology (**RIABIA-pro**, CYTED).

> 🇧🇷 **The workshop is taught in Portuguese.** We will run it in public schools in the Cerrado region of Brazil, which is why it uses Cerrado plants. The English and Spanish versions are support materials: any country in the network can adapt the workshop by swapping in plants from a local biome.

## 🌱 The question

> Does a model trained on **lab** photos still get it right on **field** images?

## 🍂 The workshop

The workshop is aimed at basic education students (middle school, adaptable to high school) and lasts 2 hours:

| Time | Part | What happens |
|---|---|---|
| 0–10 min | Opening | Where is AI already in your life? |
| 10–25 min | AI basics | What AI is and how it learns |
| 25–45 min | Machine learning | Unplugged activity: leaf detectives |
| 45–55 min | AI, biotechnology and the Cerrado | Why this matters for our region |
| 55–70 min | Demonstration | Lab × field: where the model fails |
| 70–105 min | Hands-on | Train a model in [Teachable Machine](https://teachablemachine.withgoogle.com/) |
| 105–120 min | Reflection | Bias, ethics and who does science |

1. **Leaf detectives (no computer).** In groups, students sort real leaves into healthy and symptomatic, then write yes/no questions that explain their choices. Together, those questions form a decision tree.
2. **Demonstration of this notebook.** It shows the gap between lab photos and field images, and uses Grad-CAM to show where the model looks.
3. **Hands-on in Teachable Machine.** Each group trains its own classifier with photos of leaves, directly in the browser, and then tries to "fool" it by changing the background or showing another species.

### 🌎 Adapting it to another country

The structure works with any plant. To run it elsewhere:

- **Leaves:** use species from a local biome in *Leaf detectives* and in the Teachable Machine session. This is the main change.
- **Cerrado slides** ("Why the Cerrado?" and "What AI can do for the Cerrado"): replace them with the local biome.
- **Experiment slides:** they can stay as they are, because they use public international datasets (PlantVillage and PlantDoc).

## 📑 Slides

| Language | PDF |
|---|---|
| 🇧🇷 Português (original) | [slides_pt.pdf](materiais/slides_pt.pdf) |
| 🇬🇧 English | [slides_en.pdf](materiais/slides_en.pdf) |
| 🇪🇸 Español | [slides_es.pdf](materiais/slides_es.pdf) |

## 🔬 The experiment

| | Training | Test |
|---|---|---|
| **Model A** | PlantVillage (lab) | PlantVillage (held-out test) and PlantDoc `test` |
| **Model B** | PlantVillage + PlantDoc `train` | PlantVillage (held-out test) and PlantDoc `test` |

- The 38 original classes (species + disease) are grouped into two: **healthy** and **symptomatic**.
- The model is MobileNetV2 pre-trained on ImageNet (transfer learning), with fine-tuning of the last layers.
- Grad-CAM shows which regions of the image weighed most in the decision.
- Random seed: 42.

## 📊 Results

| Balanced accuracy | Lab test (n = 8,146) | Field test (n = 236) |
|---|---|---|
| **Model A** | 98.6% | 82.4% |
| **Model B** | 98.7% | 88.4% |

![Model comparison](figuras/06_comparacao_modelos.png)

**Findings:**

1. Model A loses about 16 points outside the lab. This reproduces, with open data, an effect already reported by the PlantDoc authors.
2. The error is concentrated on healthy leaves. In the field, Model A labels 31% of them (28 of 90) as "symptomatic". Model B reduces this error to 9% (8 of 90).
3. Model B has a cost. In the field, symptomatic leaves labeled as healthy rise from 4% (6 of 146) to 14% (21 of 146). The model becomes more balanced, and balanced accuracy rises from 82.4% to 88.4%. But it misses more symptomatic leaves.
4. In the Grad-CAM examples, the model looks mainly at the leaf and its lesions.

![Model A confusion matrices](figuras/03_matrizes_modelo_A.png)

*Model A confusion matrices on the lab test (left) and the field test (right).*

![Grad-CAM on lab photos](figuras/07_gradcam_laboratorio.png)

*Model A Grad-CAM on lab photos: warm colors mark the regions that weighed most in the decision.*

![Grad-CAM on field images](figuras/08_gradcam_campo.png)

*Model A Grad-CAM on field images (PlantDoc).*

The other figures (lab × field samples, training curves and Model B matrices) are in [`figuras/`](figuras/). Full metrics are in [`resultados/`](resultados/).

## ⚠️ Limitations

- **The field test is small.** It has only 236 images (90 healthy and 146 symptomatic). In balanced accuracy, each healthy leaf is worth about 0.56 points and each symptomatic leaf about 0.34 points. The margin of uncertainty is about ±4 points. The lab → field drop is robust. Model B's improvement (about 6 points) should be seen as preliminary.
- **PlantDoc is not strictly "field".** The dataset was built from images collected on the web. Some are stock photos, with a white background and a watermark.
- **These are not Cerrado plants.** The models were trained on crop plants, not on native Cerrado species.

## 💻 How to run

**In Google Colab (recommended):**

1. Open the notebook with the button at the top of this page.
2. Turn on the GPU in *Runtime › Change runtime type › GPU*.
3. Run the cells in order. The data is downloaded automatically.

**On your computer:**

```bash
pip install -r requirements.txt
jupyter notebook notebooks/ia_no_cerrado_folhas.ipynb
```

**Main settings** (first section of the notebook):

- `MAX_POR_CLASSE_PV`: how many photos to use from each original PlantVillage class. `None` uses all of them. For a quick demo, use `300`.
- `EPOCAS`: maximum number of training epochs. Training stops early if it stops improving. For a quick demo, use `5`.

The results above were obtained with `MAX_POR_CLASSE_PV = None` and `EPOCAS = 8`.

Section 10 (testing with your own photos) is optional and can be skipped.

The notebook is written in Portuguese.

## 📂 Repository structure

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

The trained models (`.keras`) and the `dados/` and `minhas_fotos/` folders are created by the notebook and are not versioned.

## 🗂️ Data and licenses

| Dataset | Content | License | Reference |
|---|---|---|---|
| [PlantVillage](https://github.com/spMohanty/PlantVillage-Dataset) | ~54k photos of leaves on a uniform background, 14 species | CC BY-SA 3.0 | Hughes, D. P.; Salathé, M. *An open access repository of images on plant health to enable the development of mobile disease diagnostics.* arXiv:1511.08060, 2015. |
| [PlantDoc](https://github.com/pratikkayal/PlantDoc-Dataset) | ~2.6k photos of leaves in real conditions, 13 species | CC BY 4.0 | Singh, D.; Jain, N.; Jain, P.; Kayal, P.; Kumawat, S.; Batra, N. *PlantDoc: A Dataset for Visual Plant Disease Detection.* CoDS-COMAD 2020. |

The images are **not** redistributed in this repository: the notebook downloads each dataset from its original source.

The PlantVillage authors ask that models trained on their data also be shared under CC BY-SA 3.0. For this reason, the models created by the notebook follow that license.

## 🌻 About the project

This workshop is a RIABIA-pro project. Its focus is to show students where AI models fail outside controlled conditions, and to turn dataset bias into something they can see. We are preparing a short abstract and a poster about the workshop to submit to a scientific event. The workshop has not yet been run in schools.

## 🌾 Next steps

- Run the workshop in public schools in the Cerrado region.
- Validate the models on Brazilian plant image datasets (Digipathos, Embrapa).
- Repeat the experiment with several random seeds.

## 📜 Code license

The code in this repository is under the MIT license (see [`LICENSE`](LICENSE)).

## 🤝 Credits

Workshop team: Ana Sarah, Giovanna and Mariana.

Giovanna Felipe, Faculty of Science and Technology in Engineering (FCTE), University of Brasília.

RIABIA-pro / CYTED.
