# Segmentação de Radiografias Dentárias usando SAM

Este projeto implementa um sistema automatizado de segmentação de radiografias dentárias utilizando o modelo SAM (Segment Anything Model). O sistema oferece segmentação precisa e visualização de dentes em radiografias panorâmicas.

## Funcionalidades

- Segmentação automática de dentes em radiografias
- Pré-processamento aprimorado para melhor detecção de bordas
- Identificação e numeração individual dos dentes
- Visualização colorida dos dentes segmentados
- Suporte para processamento em lote
- Exibição automática dos resultados
- Suporte para múltiplos formatos de imagem (JPG, PNG, TIFF)

## Pré-requisitos

Os seguintes pacotes são necessários:

```bash
segment-anything
torch
torchvision
opencv-python
matplotlib
requests
tqdm
```

## Instalação

1. Instale os pacotes necessários:
```bash
pip install segment-anything torch torchvision opencv-python matplotlib requests tqdm
```

2. Clone este repositório:
```bash
git clone [url-do-seu-repositório]
cd segmentacao-dental
```

## Estrutura de Diretórios

```
├── content/
│   ├── img/           # Diretório de entrada para as radiografias
│   └── resultados/    # Diretório de saída para imagens segmentadas
├── segmentacao_dental.py
└── README.md
```

## Como Usar

1. Prepare seu ambiente:
```python
# Crie os diretórios necessários
mkdir -p /content/img /content/resultados
```

2. Coloque suas radiografias na pasta `/content/img`.

3. Execute a segmentação:
```python
python segmentacao_dental.py
```

Se estiver usando Google Colab:
```python
from google.colab import files
uploaded = files.upload()
for filename in uploaded.keys():
    with open(f'/content/img/{filename}', 'wb') as f:
        f.write(uploaded[filename])
```

## Parâmetros de Configuração

Principais parâmetros que podem ser ajustados no código:

### Parâmetros de Segmentação
```python
mask_generator = SamAutomaticMaskGenerator(
    points_per_side=42,            # Densidade dos pontos de amostragem
    pred_iou_thresh=0.78,          # Limiar IoU para predição de máscaras
    stability_score_thresh=0.82,    # Limiar para estabilidade da máscara
    crop_n_layers=2,               # Número de camadas para recorte
    min_mask_region_area=80,       # Área mínima para regiões da máscara
    box_nms_thresh=0.5             # Limiar NMS para previsões de caixas
)
```

### Parâmetros de Detecção de Dentes
```python
# Restrições de tamanho
área_relativa: 0.0002 < x < 0.02
# Restrições de forma
razão_aspecto: 0.3 < x < 4.5
# Dimensões mínimas
largura > 12 pixels
altura > 15 pixels
```

## Saída

O sistema gera:
1. Imagens segmentadas com sobreposições coloridas para cada dente
2. Identificação numerada dos dentes individuais
3. Imagens de saída em alta resolução (300 DPI)
4. Saída no console mostrando progresso e contagem de dentes

## Visualização

Os resultados são automaticamente exibidos e salvos com:
- Cor única para cada dente
- Numeração dos dentes
- Visualização clara dos contornos
- Sobreposições semi-transparentes
- Alto contraste com o fundo

## Tratamento de Erros

O sistema inclui tratamento robusto de erros:
- Verifica validade das imagens de entrada
- Reporta erros de processamento para imagens individuais
- Continua processando as imagens restantes em caso de falha
- Fornece rastreamento detalhado de erros para depuração

## Otimização de Desempenho

O código inclui várias otimizações:
- Suporte a GPU quando disponível
- Processamento em lote para múltiplas imagens
- Manipulação eficiente de memória
- Pipeline de pré-processamento otimizado

## Limitações

- Requer radiografias claras e de alta qualidade
- Melhor desempenho em radiografias panorâmicas
- Pode requerer ajuste de parâmetros para diferentes tipos de radiografia
- GPU recomendada para processamento mais rápido

## Como Contribuir

Sinta-se à vontade para:
- Submeter issues
- Fazer fork do repositório
- Criar pull requests com melhorias
- Sugerir novas funcionalidades

## Ajustando Parâmetros

Para ajustar a segmentação para suas necessidades específicas, você pode modificar:

1. Pré-processamento:
```python
# Em preprocess_image()
clahe = cv2.createCLAHE(clipLimit=5.0, tileGridSize=(8,8))  # Ajuste o clipLimit
enhanced = cv2.addWeighted(denoised, 1.8, gaussian, -0.8, 0)  # Ajuste os pesos
```

2. Filtragem de máscaras:
```python
# Em filter_tooth_masks()
relative_area: ajuste os limites (0.0002, 0.02)
aspect_ratio: ajuste os limites (0.3, 4.5)
```

## Suporte

Para dúvidas ou problemas:
1. Abra uma issue no GitHub
2. Entre em contato via [seu-email]
3. Consulte a documentação do SAM

## Licença

[Sua licença escolhida]

## Agradecimentos

- Baseado no Segment Anything Model (SAM) da Meta
- Otimizado para aplicações em radiografia odontológica
