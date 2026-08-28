## Atividade 1 — Quantização de Imagem

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white)
![Pillow](https://img.shields.io/badge/Pillow-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/lianeheidemann/atividades-processamento-de-imagens/blob/main/atividade/atividade-1/atividade-1-quantizacao.ipynb)

[⬅ Voltar para o repositório principal](../../README.md)

### Sobre o exercício

O objetivo é entender o efeito da **quantização de intensidade** em uma imagem em tons de cinza: a imagem original (8 bits, 256 níveis) é reduzida para 4, 2 e 1 bit(s), e o resultado de cada redução é comparado visualmente e explicado.

**Pergunta guia:** o que acontece com os detalhes e com as transições de intensidade da imagem à medida que reduzimos o número de bits?

### Sumário

- [Como executar](#como-executar)
- [Resultado](#resultado)
- [Análise](#análise)
- [Como a quantização foi implementada](#como-a-quantização-foi-implementada)
- [Relação entre bits e qualidade da imagem](#relação-entre-bits-e-qualidade-da-imagem)

### Como executar

O notebook [`atividade-1-quantizacao.ipynb`](atividade-1-quantizacao.ipynb) foi feito para rodar no **Google Colab**:

1. Abra o notebook no Colab.
2. Execute a célula de código — ela vai pedir o upload de uma imagem através de `google.colab.files.upload()`.
3. Envie uma imagem (por exemplo, uma das disponíveis em [`assets/`](assets)).
4. A imagem é convertida para tons de cinza e exibida lado a lado com as versões quantizadas em 8, 4, 2 e 1 bit(s).

### Resultado

Imagem original utilizada como entrada:

<img width="30%" src="assets/olho.jpg" alt="Imagem original">

Comparação entre a imagem original e as versões quantizadas em 8, 4, 2 e 1 bit(s):

<img src="assets/quantizacao.png" alt="Comparação da quantização em 8, 4, 2 e 1 bit(s)">

### Análise

À medida que reduzimos o número de bits, diminuímos a quantidade de tons de cinza disponíveis:

- **8 bits:** 256 níveis — praticamente igual à imagem original, pois ela normalmente já possui 8 bits.
- **4 bits:** 16 níveis — ainda preserva muitos detalhes; por isso parece muito semelhante à original e à versão de 8 bits.
- **2 bits:** 4 níveis — perde vários detalhes e apresenta regiões com tons mais uniformes, com transições mais bruscas.
- **1 bit:** apenas preto e branco — há grande perda de detalhes e nenhuma transição suave de intensidade.

As três imagens — original, 4 bits e 8 bits — parecem parecidas porque 16 níveis de cinza ainda são suficientes para representar visualmente boa parte dos detalhes desse desenho, especialmente por ele já possuir áreas bem definidas e pouco contraste gradual. Porém, observando com atenção, a versão de 4 bits apresenta pequenas faixas ou "degraus" nas regiões sombreadas. Esse efeito é chamado de banding ou posterização.

> **Posterização:** redução visível da quantidade de tons. A imagem passa a apresentar regiões separadas por níveis bem definidos, como ocorre claramente nas versões de 1 e 2 bits.
>
> **Banding:** aparecimento de faixas ou "degraus" em áreas que deveriam ter transições suaves, como um degradê. Pode aparecer na versão de 4 bits, principalmente nas regiões sombreadas.

Em resumo: quanto menos bits, maior a perda de detalhes e mais abruptas ficam as transições entre claro e escuro. Quanto mais bits, mais suaves e naturais são essas transições.

### Como a quantização foi implementada

A imagem é inicialmente convertida para tons de cinza de 8 bits, portanto cada pixel pode assumir um valor de intensidade entre 0 e 255, totalizando 256 possíveis tons de cinza.

A quantização reduz essa quantidade de valores. Para isso, o código calcula a quantidade de níveis disponíveis a partir do número de bits:

```python
niveis = 2 ** bits
```

Assim:

| Bits | Níveis de intensidade |
| :--: | :--------------------: |
| 8    | 256                     |
| 4    | 16                      |
| 2    | 4                       |
| 1    | 2                       |

Em seguida, é calculado o tamanho de cada intervalo de intensidade:

```python
passo = 256 / niveis
```

A linha:

```python
q = np.floor(im / passo) * passo
```

faz a quantização propriamente dita. Ela agrupa valores de intensidade que pertencem ao mesmo intervalo, fazendo com que diferentes pixels passem a ser representados por um mesmo valor.

Por exemplo, para 2 bits existem 4 níveis, então os 256 valores possíveis são divididos em 4 intervalos de 64 valores:

| Intervalo | Nível | Valor original → quantizado |
| :-------: | :---: | :--------------------------: |
| 0–63      | 1     | 40 → 0                       |
| 64–127    | 2     | 70 → 64, 100 → 64             |
| 128–191   | 3     | 150 → 128                    |
| 192–255   | 4     | 220 → 192                    |

Dessa forma, valores diferentes da imagem original são substituídos por uma quantidade menor de valores representativos.

Por fim, os valores são reescalados para a faixa de 0 a 255 para facilitar a visualização da imagem quantizada:

```python
q = q * (255 / (256 - passo))
```

A implementação utiliza operações matemáticas sobre os valores dos pixels com NumPy, sem utilizar uma função específica de quantização pronta.

### Relação entre bits e qualidade da imagem

Quanto menor o número de bits, menor é a quantidade de níveis de intensidade disponíveis. Consequentemente, mais valores diferentes precisam ser representados pelo mesmo nível, causando perda de informação — o que produz posterização e, nas regiões de transição suave, o efeito de banding, como descrito no início deste documento.
