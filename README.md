# 🧠 Segmentação de Tumores Cerebrais - BraTS2020

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-1.13+-ee4c2c.svg)](https://pytorch.org/)
[![U-Net](https://img.shields.io/badge/Model-3D%20U--Net-green.svg)](https://arxiv.org/abs/1606.06650)
[![Status](https://img.shields.io/badge/Status-Concluído-brightgreen.svg)](.)

## 📋 Visão Geral
Este projeto implementa um **pipeline completo de segmentação automática de tumores cerebrais** utilizando uma arquitetura **3D U-Net** otimizada, treinada sobre o dataset **BraTS2020** (*Brain Tumor Segmentation Challenge*).  
O objetivo é identificar e segmentar sub-regiões tumorais em imagens de ressonância magnética (MRI), auxiliando no **diagnóstico rápido e padronizado**.

---

## 🧪 Contexto e Desafio

### Contexto Médico
- **Gliomas**: tumores cerebrais primários mais comuns e agressivos.
- **Segmentação manual**: demorada, subjetiva e variável entre especialistas.
- **Necessidade clínica**: segmentação precisa é crucial para planejamento cirúrgico e radioterápico.

### Tarefa de Segmentação
O modelo deve identificar **4 classes** em volumes 3D de MRI:
1. **Fundo** (tecido saudável)
2. **NCR/NET** – Necrose / Núcleo não realçante
3. **Edema** – Região peritumoral
4. **Tumor Realçante** – Parte ativa do tumor

---

## 🗂 Dataset BraTS2020

- **Modalidades**: T1, T1ce, T2, FLAIR
- **Formato**: NIfTI (.nii)
- **Resolução original**: 240×240×155 voxels
- **Pacientes**: 369 (250 treino / 74 validação / 45 teste)
- **Anotações**: Máscaras manuais por especialistas
- **Pré-processamento aplicado**:
  - Descarte da modalidade T1 (baixo contraste)
  - Normalização Min-Max para [0,1]
  - Redimensionamento para 128×128×64
  - Remapeamento de labels (0,1,2,4 → 0,1,2,3)

---

## 🏗 Arquitetura do Modelo

- **Base**: 3D U-Net com *skip connections*
- **Encoder**:
  - Convoluções 3D (3×3×3)
  - Group Normalization
  - LeakyReLU (slope=0.01)
- **Decoder**:
  - Upsampling + concatenação com *features* do encoder
  - LeakyReLU (slope=0.2)
- **Parâmetros treináveis**: ~5.65M
- **Loss Function**: Cross-Entropy ponderada + Dice Loss
- **Otimização**:
  - Otimizador: Adam (lr=1e-3)
  - Redução dinâmica de *learning rate*
  - Early Stopping (paciente=5)
  - Batch size = 16, 20 épocas

---

## 📊 Resultados

### Avaliação Quantitativa (Validação)
| Região            | Dice Score |
|-------------------|------------|
| Whole Tumor (WT)  | **0.811**  |
| Tumor Core (TC)   | 0.644      |
| Enhancing Tumor (ET) | 0.563   |

### Casos Representativos
- **BraTS20_Training_090** – boa segmentação em todas as sub-regiões.
- **BraTS20_Training_278** – modelo corretamente detecta ausência de tumor realçante (ET), mostrando alta especificidade.

---

## 📂 Estrutura do Projeto
```
📁 BrainTumor_UNet_Segmentation/
├── 📓 Project.ipynb # Notebook principal
├── 📄 README.md # Este arquivo
├── 📄 requirements.txt # Dependências
├── 📄 LICENSE # Licença MIT
├── 📁 BraTS2020_TrainingData/ # Dataset de treinamento
└── 📁 BraTS2020_ValidationData/ # Dataset de validação
```


---

## ⚙ Tecnologias Utilizadas
- Python 3.8+
- PyTorch
- nibabel
- scikit-learn
- matplotlib
- NumPy / SciPy

---

## 🚀 Próximos Passos
- Explorar arquiteturas avançadas (com mecanismos de atenção)
- Aplicar *data augmentation* mais sofisticado
- Implementar *ensemble* de modelos para maior robustez
- Avaliar em dados clínicos reais

---

## 📜 Licença
Este projeto está licenciado sob a MIT License - veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

**Desenvolvido para pesquisa em segmentação de tumores cerebrais utilizando deep learning.**
