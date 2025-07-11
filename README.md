# Segmentação de Tumores Cerebrais - BraTS2020

## Visão Geral

Este projeto implementa um pipeline completo de segmentação automática de tumores cerebrais utilizando redes neurais U-Net 3D, baseado no dataset BraTS2020 (Brain Tumor Segmentation Challenge). O objetivo é segmentar diferentes regiões tumorais em imagens de ressonância magnética (MRI) do cérebro.

## Problema

### Contexto Médico
- **Gliomas** são os tumores cerebrais primários mais comuns e agressivos
- **Segmentação manual** é um processo demorado e sujeito a variabilidade entre especialistas
- **Diagnóstico precoce** e segmentação precisa são cruciais para o planejamento do tratamento

### Desafio Técnico
- Segmentar **4 regiões distintas** em imagens 3D de MRI:
  - **Fundo** (tecido saudável)
  - **NCR/NET** (Necrose/Não-realçante)
  - **Edema** (região peritumoral)
  - **Tumor Realçante** (região ativa)

## Dataset BraTS2020

### Modalidades de Imagem
- **T1**: Contraste anatômico básico
- **T1ce**: T1 com contraste (realça tumor ativo)
- **T2**: Detecta edema e alterações
- **FLAIR**: Suprime fluido cerebrospinal

### Especificações
- **Formato**: NIfTI (.nii)
- **Resolução Original**: 240×240×155 voxels
- **Pacientes**: ~370 casos de treinamento
- **Anotações**: Segmentação manual por especialistas

## Solução Proposta

### Pipeline de Processamento
1. **Análise Exploratória**: Análise de contraste entre modalidades
2. **Seleção Automática**: Escolha das 3 melhores modalidades por contraste
3. **Pré-processamento**:
   - Normalização Min-Max [0,1]
   - Cropping inteligente baseado na máscara
   - Redimensionamento para 128×128×64
   - Remapeamento de labels (0,1,2,4 → 0,1,2,3)

### Arquitetura do Modelo
- **U-Net 3D** com skip connections
- **Input**: 3 modalidades selecionadas automaticamente
- **Output**: Segmentação de 4 classes
- **Loss Function**: Dice Loss + CrossEntropy

## Inovações do Projeto

### 1. Seleção Automática de Modalidades
- Análise quantitativa de contraste entre regiões tumorais
- Seleção das 3 modalidades com melhor separabilidade
- Redução de dimensionalidade mantendo qualidade

### 2. Pipeline Inteligente
- Cropping automático baseado na máscara de tumor
- Processamento robusto com fallbacks para casos edge
- Visualização completa do processo

### 3. Implementação Otimizada
- Dataset PyTorch customizado
- DataLoader eficiente para volumes 3D
- Estrutura modular e reutilizável

## Estrutura do Projeto

```
📁 BrainTumor_UNet_Segmentation/
├── 📓 Project.ipynb                 # Notebook principal
├── 📄 README.md                     # Este arquivo
├── 📄 requirements.txt              # Dependências
├── 📄 LICENSE                       # Licença MIT
├── 📁 BraTS2020_TrainingData/       # Dataset de treinamento
└── 📁 BraTS2020_ValidationData/     # Dataset de validação
```

## Resultados Esperados

### Métricas de Avaliação
- **Dice Score**: Sobreposição entre predição e ground truth
- **IoU (Jaccard)**: Interseção sobre união
- **Hausdorff Distance**: Distância máxima entre superfícies
- **Sensitivity/Specificity**: Detecção de regiões tumorais

### Aplicações Clínicas
- **Planejamento cirúrgico**: Delimitação precisa do tumor
- **Radioterapia**: Definição de volumes alvo
- **Monitoramento**: Acompanhamento da evolução tumoral
- **Pesquisa**: Análise quantitativa de características tumorais

## Tecnologias Utilizadas

- **Python 3.8+**
- **PyTorch**: Framework de deep learning
- **nibabel**: Manipulação de imagens médicas NIfTI
- **scikit-learn**: Pré-processamento e métricas
- **matplotlib**: Visualização
- **numpy/scipy**: Computação científica

## Contribuições

Este projeto demonstra:
- **Pipeline de ML médico** completo e reproduzível
- **Seleção automática de features** para imagens médicas
- **Boas práticas** em processamento de dados 3D
- **Visualização clara** do processo de segmentação

## Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## Referências

- **BraTS Challenge**: https://www.med.upenn.edu/cbica/brats2020/
- **U-Net Original**: Ronneberger et al. "U-Net: Convolutional Networks for Biomedical Image Segmentation"
- **3D U-Net**: Çiçek et al. "3D U-Net: Learning Dense Volumetric Segmentation from Sparse Annotation"

---

**Desenvolvido para pesquisa em segmentação de tumores cerebrais utilizando deep learning.**
