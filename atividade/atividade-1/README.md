### Exercício: Quantização de Imagem

<img width="30%" src="assets/olho.jpg">
<img src="assets/quantizacao.png">

À medida que reduzimos o número de bits, diminuímos a quantidade de tons de cinza disponíveis:

- 8 bits: 256 níveis — praticamente igual à imagem original, pois ela normalmente já possui 8 bits.
- 4 bits: 16 níveis — ainda preserva muitos detalhes; por isso parece muito semelhante à original e à versão de 8 bits.
- 2 bits: 4 níveis — perde vários detalhes e apresenta regiões com tons mais uniformes, com transições mais bruscas.
- 1 bit: apenas preto e branco — há grande perda de detalhes e nenhuma transição suave de intensidade.

As três imagens — original, 4 bits e 8 bits — parecem parecidas porque 16 níveis de cinza ainda são suficientes para representar visualmente boa parte dos detalhes desse desenho, especialmente por ele já possuir áreas bem definidas e pouco contraste gradual. Porém, observando com atenção, a versão de 4 bits apresenta pequenas faixas ou “degraus” nas regiões sombreadas. Esse efeito é chamado de banding ou posterização.

> Posterização: redução visível da quantidade de tons. A imagem passa a apresentar regiões separadas por níveis bem definidos, como ocorre claramente nas versões de 1 e 2 bits.<br>
> Banding: aparecimento de faixas ou “degraus” em áreas que deveriam ter transições suaves, como um degradê. Pode aparecer na versão de 4 bits, principalmente nas regiões sombreadas.

Em resumo: quanto menos bits, maior a perda de detalhes e mais abruptas ficam as transições entre claro e escuro. Quanto mais bits, mais suaves e naturais são essas transições.



