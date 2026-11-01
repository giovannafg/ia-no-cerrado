<div align="center">

# 🌿 IA e Biologia no Cerrado 🍃

**Ensinando o computador a reconhecer a saúde das plantas**

[English](README.md) · **Português** · [Español](README.es.md)

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/giovannafg/ia-no-cerrado/blob/main/notebooks/ia_no_cerrado_folhas.ipynb)
[![Licença: MIT](https://img.shields.io/badge/c%C3%B3digo-MIT-6e8a23.svg)](LICENSE)
[![Slides: PT · EN · ES](https://img.shields.io/badge/slides-PT%20%C2%B7%20EN%20%C2%B7%20ES-ffde59.svg)](#-slides)
[![RIABIA-pro](https://img.shields.io/badge/RIABIA--pro-CYTED-4e621c.svg)](#-sobre-o-projeto)

</div>

Material aberto da oficina **IA e Biologia no Cerrado**, de duas horas, para escolas. É um projeto da Rede Iberoamericana de Prospectiva em Inteligência Artificial para Biociências e Biotecnologia (**RIABIA-pro**, CYTED).

> 🇧🇷 **A oficina é dada em português.** Vamos aplicá-la em escolas públicas da região do Cerrado, por isso ela usa plantas do Cerrado. As versões em inglês e espanhol são material de apoio: qualquer país da rede pode adaptar a oficina trocando as folhas por plantas de um bioma local.

## 🌱 A pergunta

> Um modelo treinado com fotos de **laboratório** continua acertando com imagens de **campo**?

## 🍂 A oficina

A oficina é voltada à educação básica (ensino fundamental, adaptável ao ensino médio) e dura 2 horas:

| Tempo | Momento | O que acontece |
|---|---|---|
| 0–10 min | Abertura | Onde a IA já está na sua vida? |
| 10–25 min | Fundamentos de IA | O que é IA e como ela aprende |
| 25–45 min | Machine learning | Atividade sem computador: detetives de folhas |
| 45–55 min | IA, biotecnologia e Cerrado | Por que isso importa para a nossa região |
| 55–70 min | Demonstração | Laboratório × campo: onde o modelo erra |
| 70–105 min | Mão na massa | Treinar um modelo no [Teachable Machine](https://teachablemachine.withgoogle.com/) |
| 105–120 min | Reflexão | Viés, ética e quem faz ciência |

1. **Detetives de folhas (sem computador).** Em grupos, os alunos separam folhas reais em saudáveis e com sintomas e escrevem perguntas de sim ou não que expliquem a separação. Juntas, essas perguntas formam uma árvore de decisão.
2. **Demonstração deste notebook.** Mostra a diferença entre fotos de laboratório e imagens de campo, e usa o Grad-CAM para mostrar onde o modelo olha.
3. **Prática no Teachable Machine.** Cada grupo treina o próprio classificador com fotos de folhas, direto no navegador, e depois tenta "enganar" o modelo trocando o fundo ou mostrando outra espécie.

### 🌎 Para adaptar a outro país

A estrutura funciona com qualquer planta. Para aplicar em outro lugar:

- **Folhas:** use espécies de um bioma local nos *Detetives de folhas* e na prática no Teachable Machine. É a principal mudança.
- **Slides do Cerrado** ("Por que o Cerrado?" e "O que a IA pode fazer pelo Cerrado"): troque pelo bioma local.
- **Slides do experimento:** podem ficar iguais, porque usam datasets públicos internacionais (PlantVillage e PlantDoc).

## 📑 Slides

| Idioma | PDF |
|---|---|
| 🇧🇷 Português (original) | [slides_pt.pdf](materiais/slides_pt.pdf) |
| 🇬🇧 English | [slides_en.pdf](materiais/slides_en.pdf) |
| 🇪🇸 Español | [slides_es.pdf](materiais/slides_es.pdf) |

## 🔬 O experimento

| | Treino | Teste |
|---|---|---|
| **Modelo A** | PlantVillage (laboratório) | PlantVillage (teste separado) e PlantDoc `test` |
| **Modelo B** | PlantVillage + PlantDoc `train` | PlantVillage (teste separado) e PlantDoc `test` |

- As 38 classes originais (espécie + doença) são agrupadas em duas: **saudável** e **com sintomas**.
- O modelo é a MobileNetV2 pré-treinada no ImageNet (aprendizado por transferência), com ajuste fino das últimas camadas.
- O Grad-CAM mostra quais regiões da imagem mais pesaram na decisão.
- Semente aleatória: 42.

## 📊 Resultados

| Acurácia balanceada | Teste laboratório (n = 8.146) | Teste campo (n = 236) |
|---|---|---|
| **Modelo A** | 98,6% | 82,4% |
| **Modelo B** | 98,7% | 88,4% |

![Comparação dos modelos](figuras/06_comparacao_modelos.png)

**Achados:**

1. O Modelo A perde cerca de 16 pontos fora do laboratório. Isso reproduz, com dados abertos, um efeito já relatado pelos autores do PlantDoc.
2. O erro se concentra nas folhas saudáveis. No campo, o Modelo A classifica 31% delas (28 de 90) como "com sintomas". O Modelo B reduz esse erro para 9% (8 de 90).
3. O Modelo B tem um custo. No campo, as folhas com sintomas classificadas como saudáveis sobem de 4% (6 de 146) para 14% (21 de 146). O modelo fica mais equilibrado, e a acurácia balanceada sobe de 82,4% para 88,4%. Mas ele passa a deixar escapar mais folhas com sintomas.
4. Nos exemplos de Grad-CAM, o modelo olha principalmente para a folha e as lesões.

![Matrizes de confusão do Modelo A](figuras/03_matrizes_modelo_A.png)

*Matrizes de confusão do Modelo A no teste de laboratório (esquerda) e de campo (direita).*

![Grad-CAM em fotos de laboratório](figuras/07_gradcam_laboratorio.png)

*Grad-CAM do Modelo A em fotos de laboratório: as cores quentes marcam as regiões que mais pesaram na decisão.*

![Grad-CAM em fotos de campo](figuras/08_gradcam_campo.png)

*Grad-CAM do Modelo A em imagens de campo (PlantDoc).*

As demais figuras (amostras laboratório × campo, curvas de treino e matrizes do Modelo B) estão em [`figuras/`](figuras/). As métricas completas estão em [`resultados/`](resultados/).

## ⚠️ Limitações

- **O teste de campo é pequeno.** São só 236 imagens (90 saudáveis e 146 com sintomas). Na acurácia balanceada, cada folha saudável vale cerca de 0,56 ponto e cada folha com sintomas cerca de 0,34 ponto. A margem de incerteza é de cerca de ±4 pontos. A queda laboratório → campo é robusta. A melhora do Modelo B (cerca de 6 pontos) deve ser vista como preliminar.
- **O PlantDoc não é estritamente "campo".** O dataset foi montado com imagens coletadas da web. Parte delas são fotos de banco de imagens, com fundo branco e marca d'água.
- **Não são plantas do Cerrado.** Os modelos foram treinados com plantas cultivadas, não com espécies nativas do Cerrado.

## 💻 Como rodar

**No Google Colab (recomendado):**

1. Abra o notebook pelo botão no topo desta página.
2. Ative a GPU em *Ambiente de execução › Alterar o tipo de ambiente de execução › GPU*.
3. Rode as células em ordem. Os dados são baixados automaticamente.

**No computador:**

```bash
pip install -r requirements.txt
jupyter notebook notebooks/ia_no_cerrado_folhas.ipynb
```

**Configurações principais** (primeira seção do notebook):

- `MAX_POR_CLASSE_PV`: quantas fotos usar de cada classe original do PlantVillage. `None` usa todas. Para uma demonstração rápida, use `300`.
- `EPOCAS`: número máximo de épocas de treino. O treino para antes se parar de melhorar. Para uma demonstração rápida, use `5`.

Os resultados acima foram obtidos com `MAX_POR_CLASSE_PV = None` e `EPOCAS = 8`.

A seção 10 (teste com as suas fotos) é opcional e pode ser pulada.

## 📂 Estrutura do repositório

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

Os modelos treinados (`.keras`) e as pastas `dados/` e `minhas_fotos/` são gerados pelo notebook e não são versionados.

## 🗂️ Dados e licenças

| Dataset | Conteúdo | Licença | Referência |
|---|---|---|---|
| [PlantVillage](https://github.com/spMohanty/PlantVillage-Dataset) | ~54 mil fotos de folhas sobre fundo uniforme, 14 espécies | CC BY-SA 3.0 | Hughes, D. P.; Salathé, M. *An open access repository of images on plant health to enable the development of mobile disease diagnostics.* arXiv:1511.08060, 2015. |
| [PlantDoc](https://github.com/pratikkayal/PlantDoc-Dataset) | ~2,6 mil fotos de folhas em condições reais, 13 espécies | CC BY 4.0 | Singh, D.; Jain, N.; Jain, P.; Kayal, P.; Kumawat, S.; Batra, N. *PlantDoc: A Dataset for Visual Plant Disease Detection.* CoDS-COMAD 2020. |

As imagens **não** são redistribuídas neste repositório: o notebook baixa cada dataset da fonte original.

Os autores do PlantVillage pedem que modelos treinados com os dados também sejam compartilhados sob CC BY-SA 3.0. Por isso, os modelos gerados pelo notebook seguem essa licença.

## 🌻 Sobre o projeto

Esta oficina é um projeto da RIABIA-pro. O foco é mostrar aos alunos onde os modelos de IA erram fora das condições controladas e tornar visível o viés nos dados. Estamos preparando um resumo e um pôster sobre a oficina para submeter a um evento científico. A oficina ainda não foi aplicada nas escolas.

## 🌾 Próximos passos

- Aplicar a oficina em escolas públicas da região do Cerrado.
- Validar os modelos com bancos de imagens de plantas brasileiras (Digipathos, Embrapa).
- Repetir o experimento com várias sementes.

## 📜 Licença do código

O código deste repositório está sob a licença MIT (veja [`LICENSE`](LICENSE)).

## 🤝 Créditos

Equipe da oficina: Ana Sarah, Giovanna e Mariana.

Giovanna Felipe, Faculdade de Ciências e Tecnologias em Engenharia (FCTE), Universidade de Brasília.

RIABIA-pro / CYTED.
