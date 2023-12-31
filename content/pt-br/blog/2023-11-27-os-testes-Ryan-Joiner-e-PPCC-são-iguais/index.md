---
title: "Os testes Ryan-Joiner e PPCC são iguais?"
authors: [Anderson Canteli]
banner: img/banners/banner-1.jfif
date: 2023-11-27T22:28:00-03:00
categories: ["data analisys"]
tags: ["normality", "normtest", "Python", "Ryan-Joiner", "Looney-Gulledge", "PPCC"]
mathjax: true
bibliography: [RyanJoiner1976.bib, LooneyGulledge1985.bib, Filliben1975.bib, ShapiroWilk1965.bib, Blom1958.bib]

---

<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>

<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />

<!-- Ajustando configurações iniciais -->
<!-- Setando valores iniciais nlolo r -->
<!-- Importações no Pyton -->
<!-- Setando valores iniciais em Python -->
<!-- Calculos em Python -->

## Motivação

Este texto foi escrito durante estudo e implementação do teste de PPCC no pacote [`normtest`](https://normtest.readthedocs.io/en/latest/), onde percebi a semelhança entre os testes, especialmente em relação aos valores críticos. Para verificar o quão parecido são os valores, resolvi encarar o problema tomando um cafezinho.

<details>
<summary class="pointer">
<h3>
Spoiler
</h3>
</summary>
<hr>
<blockquote>
<h4>
Os testes de Ryan-Joiner e PPCC sáo iguais?
</h4>

Sim e não. Eles são uns 99,98% iguais.

</blockquote >
</details>

## Introdução

Quando você começa a estudar testes de Normalidade, invariavelmente irá se deparar com um tipo de teste baseado no *coeficiente de correlação* (vulgo `\(r_{pearson}\)`). Estes testes prometem ser simples e ao mesmo tempo poderosos, entregando resultados similares ao teste de Shapiro and Wilk (1965). Este tipo de teste (provavelmente) foi introduzido por Filliben (1975), com variações propostas por Ryan and Joiner (1976) e mais tarde por Looney and Gulledge (1985) (o teste PPCC).

A ideia deste tipo de teste é realmente simples: comparar os dados experimentais com o que seria esperado para a distribuição Normal. Geralmente, plotamos o gráfico dos dados ordenados *versus* os valores Normais. Se o gráfico apresentar comportamento linear, quer dizer que existe correlação entre os dados experimentais e a distribuição Normal, e, portanto, os dados podem ser considerados com distribuição Normal. Como o comportamento esperado é linear, o coeficiente de correlação surge naturalmente como uma medida de Normalidade (e.g., a estatística do teste).

A diferença entre estes testes está na forma que se obtém os valores da distribuição Normal, o que não é uma tarefa simples. Porém, existem algumas equações que fornecem resultados adequados (Blom (1958)). Filliben (1975) propôs o uso das medianas uniformes, que são estimadas através da seguinte relação:

<blockquote>

`$$\begin{split}m_{i} = \begin{cases}1-0.5^{1/n} & i = 1\\ \frac{i-0.3175}{n+0.365} & i = 2, 3,  \ldots , n-1 \\ 0.5^{1/n}& i=n \end{cases}\end{split}$$`

onde `\(n\)` é o tamanho da amostra e `\(i\)` é a i-ésima observação.

</blockquote>

As ordens uniformes devem ser transformadas para a escala Normal `\(z_i\)`:

<blockquote>

`$$z_{i} = \phi^{-1} \left(m_{i} \right)$$`

onde `\(\phi^{-1}\)` é a inversa da distribuição Normal padrão.

</blockquote>

Os valores de `\(z_i\)` são plotados com os dados experimentais ordenados, obtendo um gráfico com o perfil linear, como o apresentado na Figura <a href="#fig:fillibenexample">1</a>.

<div class="figure">

<img src="{{< blogdown/postref >}}index_files/figure-html/fillibenexample-1.png" alt="Exemplo do gráfico de correlação esperado para uma distribuição Normal." width="480" />
<p class="caption">
<span id="fig:fillibenexample"></span>Figure 1: Exemplo do gráfico de correlação esperado para uma distribuição Normal.
</p>

</div>

<br>

Se a correlação for forte o suficiente (e.g., se o comportamento dos dados for parecido com uma reta), os dados podem ser considerados com distribuição Normal. A estatística do teste é o coeficiente de correlação, que deve ser comparado com os valores críticos para a conclusão do teste. O artigo original trás os valores críticos para diversos níveis de significância em função do tamanho amostral (Filliben (1975)).

A hipótese nula do teste ($H_0$) é de que os dados seguem a distribuição Normal, e vamos procurar por evidências do contrário ($H_1$). Ou seja:

<blockquote>

- `\(H_0\)`: Os dados foram amostrados da distribuição Normal;
- `\(H_1\)`: Os dados foram amostrados de uma distribuição diferente da Normal;

</blockquote>

A conclusão é feita comparando o valor crítico ($r_{crítico}$) com a estatística do teste ($R_p$). Caso a estatística seja maior ou igual ao valor crítico, nós falhamos em rejeitar a hipótese de normalidade dos dados. Caso contrário, nós rejeitamos a hipótese nula. Assim:

<blockquote>

- Se `\(R_p \geq R_{crítico}\)` os dados são, pelo menos aproximadamente, Normais;
- Se `\(R_p < R_{crítico}\)` os dados não são Normais;

</blockquote>

<br>

## Relação entre os testes de Ryan-Joiner e PPCC

Os testes de Ryan-Joiner e PPCC (*Probability Plot Correlation Coefficient*, que será chamado como teste de Looney-Gulledge a partir de agora neste texto) utilizam a mesma lógica vista acima, comparando o coeficiente de correlação com valores críticos. Porém, utilizam a estatística de ordem Normal ao invés das medianas uniformes, que são estimadas de forma um pouco diferente. As equações utilizadas apresentam o perfil:

<blockquote>

`$$p_{i} = \frac{i - \alpha_{cte}}{n - 2 \times \alpha_{cte} + 1}$$`

onde `\(n\)` é o número de observações na amostra e `\(\alpha_{cte}\)` é uma constante pré determinada.

</blockquote>

Todas as referências que encontrei indicam o uso de `\(3/8\)` como valor correto para esta constante. Blom (1958) estudou três constantes ( `\(0\)`, `\(3/8\)` e `\(1/2\)`), chegando a conclusão de que `\(3/8\)` apresenta resultados mais precisos, especialmente para dados com poucas observações.

Ambos os trabalhos de Ryan and Joiner (1976) e Looney and Gulledge (1985) utilizam `\(\alpha_{cte}=3/8\)`. Assim, ambos testes de Normalidade calculam a estatística com base nos mesmos dados, e, portanto, a estatística dos dois testes é *idêntica*. Portanto, a única diferença entre os testes se deve aos valores críticos.

<br>

### Comparando os valores críticos

Ryan and Joiner (1976) estimaram valores críticos para `\(1\%\)`, `\(5\%\)` e `\(10\%\)` de significância, e reportaram equações para cada valor de `\(\alpha\)`:

<blockquote>

`$$\begin{align}\begin{aligned}R_{critico}^{\alpha=10\%} = 1.0071 - \frac{0.1371}{\sqrt{n}} - \frac{0.3682}{n} + \frac{0.7780}{n^{2}}\\R_{critico}^{\alpha=5\%} = 1.0063 - \frac{0.1288}{\sqrt{n}} - \frac{0.6118}{n} + \frac{1.3505}{n^{2}}\\R_{critico}^{\alpha=1\%} = 0.9963 - \frac{0.0211}{\sqrt{n}} - \frac{1.4106}{n} + \frac{3.1791}{n^{2}}\end{aligned}\end{align}$$`

onde `\(n\)` é o tamanho da amostra.

</blockquote>

\* As equações acima foram obtidas a partir de dados simulados e depois de terem sido suavizados.

Já Looney and Gulledge (1985) estimaram valores críticos para uma maior variedade de níveis de significância, cobrindo todo o intervalo de forma espaçada (os mesmos valores estimados por Filliben (1975)). O artigo original trás os valores críticos para os diversos níveis de significância em função do tamanho amostral (entre 3 e 100).

A Tabela <a href="#tab:tcriticos">1</a> apresenta os valores críticos para `\(5\%\)` de significância variando o tamanho amostral entre `\(3\)` e `\(10\)` (os valores críticos de Ryan and Joiner (1976) foram arredondados na terceira casa decimal para ter precisão similar aos dados de Looney and Gulledge (1985)). Como é possível observar, os valores críticos são muito parecidos, sendo iguais para alguns valores de `\(n\)`.

<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
<span id="tab:tcriticos"></span>Table 1: Comparativo entre os valores críticos dos testes de Ryan-Joiner e Looney-Gulledge
</caption>
<thead>
<tr>
<th style="text-align:center;">
Tamanho amostral
</th>
<th style="text-align:center;">
Ryan Joiner
</th>
<th style="text-align:center;">
Looney Gulledge
</th>
<th style="text-align:center;">
Diferença
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
0.879
</td>
<td style="text-align:center;">
0.878
</td>
<td style="text-align:center;">
-0.001
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
0.868
</td>
<td style="text-align:center;">
0.873
</td>
<td style="text-align:center;">
0.005
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
0.880
</td>
<td style="text-align:center;">
0.880
</td>
<td style="text-align:center;">
0.000
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0.888
</td>
<td style="text-align:center;">
0.889
</td>
<td style="text-align:center;">
0.001
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
0.898
</td>
<td style="text-align:center;">
0.898
</td>
<td style="text-align:center;">
0.000
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
0.906
</td>
<td style="text-align:center;">
0.905
</td>
<td style="text-align:center;">
-0.001
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
0.912
</td>
<td style="text-align:center;">
0.912
</td>
<td style="text-align:center;">
0.000
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
0.918
</td>
<td style="text-align:center;">
0.918
</td>
<td style="text-align:center;">
0.000
</td>
</tr>
</tbody>
</table>

Esta semelhança fica mais evidente quando plotamos o gráfico com todos os valores disponíveis, como apresentado na Figura <a href="#fig:criticalplot">2</a>. Com exceção de alguns poucos pontos, os valores críticos são iguais.

<div class="figure">

<img src="{{< blogdown/postref >}}index_files/figure-html/criticalplot-1.png" alt="Comparativo entre os valores críticos do testes de Ryan-Joiner e Looney-Gylledge" width="960" />
<p class="caption">
<span id="fig:criticalplot"></span>Figure 2: Comparativo entre os valores críticos do testes de Ryan-Joiner e Looney-Gylledge
</p>

</div>

<br>

Poderia parar aqui esta análise, pois a Figura <a href="#fig:criticalplot">2</a> deixa bem claro que os valores críticos são muito parecidos. Porém, ainda tenho café na caneca. Para explorar um pouco mais, a Figura <a href="#fig:diffplot">3</a> traz a diferença estimada para os valores críticos:

<div class="figure">

<img src="{{< blogdown/postref >}}index_files/figure-html/diffplot-3.png" alt="Comparativo entre as diferenças dos valores críticos entre os testes de Ryan-Joiner e Looney-Gylledge." width="960" />
<p class="caption">
<span id="fig:diffplot"></span>Figure 3: Comparativo entre as diferenças dos valores críticos entre os testes de Ryan-Joiner e Looney-Gylledge.
</p>

</div>

<br>

Os resultados obtidos indicam uma pequena diferença entre os resultados, especialmente para `\(\alpha=1\%\)` e valores de `\(n\)` pequenos. Nos demais casos, a diferença (que já era pequena) é praticamente nenhuma. Para saber quanto, em média, os valores críticos são diferentes, estimei a diferença quadrática média através da seguinte relação:

<blockquote>

`$$dff^2 = \sum_{i=3}^{n}\dfrac{\left(RJ_{critico} - LG_{critico} \right)^{2}}{n-1}$$`

onde `\(RJ_{critico}\)` é o valor crítico do teste de Ryan-Joiner, `\(LG_{critico}\)` é o valor crítico do teste de Looney-Gulledge e `\(n\)` é o número de valores críticos adotados

</blockquote>

e então a raiz quadrada da diferença quadrática média, obtendo algo parecido com o desvio padrão (Tabela <a href="#tab:tabledesvios">2</a>):

<blockquote>

`$$dff=\sqrt{dff^{2}}$$`

</blockquote>
<table class="table table-striped table-hover table-condensed table-responsive" style="margin-left: auto; margin-right: auto;">
<caption>
<span id="tab:tabledesvios"></span>Table 2: Média dos desvios entre os valores críticos do teste de Ryan-Joiner e Looney-Gulledge.
</caption>
<thead>
<tr>
<th style="text-align:center;">
Alfa
</th>
<th style="text-align:center;">
Diferença quadrática média
</th>
<th style="text-align:center;">
Desvio padrão
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
0.01
</td>
<td style="text-align:center;">
4e-06
</td>
<td style="text-align:center;">
0.0020088
</td>
</tr>
<tr>
<td style="text-align:center;">
0.05
</td>
<td style="text-align:center;">
8e-07
</td>
<td style="text-align:center;">
0.0008786
</td>
</tr>
<tr>
<td style="text-align:center;">
0.10
</td>
<td style="text-align:center;">
3e-07
</td>
<td style="text-align:center;">
0.0005774
</td>
</tr>
</tbody>
</table>

<br>

As diferenças obtidas são muito pequenas, porém é bem claro que para `\(\alpha=1\%\)` ela é um pouco maior. Como a equação utilizada para estimar as diferenças as eleva ao quadrado, o sinal não é considerado. Contudo, para esta análise, o sinal da diferença é importante pois os valores críticos indicam o critério de rejeição de `\(H_0\)`. Assim, caso um teste apresente consistentemente valores maiores do que o outro teste, temos um padrão e este teste seria mais sensível.

Como a diferença está sendo calculada entre o valor crítico do teste de Ryan and Joiner (1976) e Looney and Gulledge (1985) (e.g., `\(RJ_{critico} - LG_{critico}\)`), se o resultado é positivo quer dizer que o valor crítico de Ryan-Joiner é *maior*, e portanto será mais rígido (quanto maior o valor crítico, maior a chance de rejeitar `\(H_0\)`). A Figura <a href="#fig:barplot">4</a> traz o resultado da análise entre quantas diferenças foram positivas, negativas ou nulas.

<div class="figure">

<img src="{{< blogdown/postref >}}index_files/figure-html/barplot-1.png" alt="Comparativo entre as diferenças dos críticos entre os testes de Ryan-Joiner e Looney-Gylledge" width="768" />
<p class="caption">
<span id="fig:barplot"></span>Figure 4: Comparativo entre as diferenças dos críticos entre os testes de Ryan-Joiner e Looney-Gylledge
</p>

</div>

<br>

Mais de `\(65\%\)` das diferenças são nulas para dados com `\(5\%\)` e `\(10\%\)` de significância, indicando a similaridade entre os testes nestes níveis de siginificância. Já para `\(1\%\)` temos que `\(67\%\)` das diferenças são ***positivas***, indicando que neste caso os valores críticos do teste de Ryan and Joiner (1976) são ***maiores*** do que os valores críticos estiamdos por Looney and Gulledge (1985).

## Conclusão

Nesta análise comparamos os testes de Ryan and Joiner (1976) e Looney and Gulledge (1985). Vimos que a estatística de ambos os testes é estimada de forma idêntica. Depois, comparamos os valores críticos destes testes, e foi possível observar que eles são muito parecidos.

Entretanto, uma avalição mais aprofundada indicou uma pequena diferença positiva entre os valores críticos para `\(1\%\)` de significância, especialmente para amostras de tamanho pequeno. Isto mostra que os valores críticos do teste de Ryan and Joiner (1976) são, em sua maioria, maiores do que os respetctivos valores do teste de Looney and Gulledge (1985).

Do ponto de vista prático isto quer dizer que se o pesquisador decidir adotar 99% de confiança para o erro de tipo I, o teste de Ryan and Joiner (1976) será um pouco mais rígido do que o teste de Looney and Gulledge (1985) (irá rejeitar a hipótese nula mais vezes).

Porém, como na prática nós adotamos 95% de confiança, não fará diferença adotar um ou outro teste. Mas claro, você deveria utilizar o teste de Shapiro and Wilk (1965), que é o menos pior dentre os testes de Normalidade.

## Extras

Você encontra um notebook com os códigos utilizados para construir os gráficos e tabelas [neste link](https://github.com/puzzle-in-a-mug/blog/blob/main/notebooks/Are-Ryan-Joiner-and-PPCC-tests-the-same.ipynb).

<script src="https://giscus.app/client.js"
        data-repo="puzzle-in-a-mug/blog"
        data-repo-id="R_kgDOKx46OQ"
        data-category="Announcements"
        data-category-id="DIC_kwDOKx46Oc4Cbf5x"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="light"
        data-lang="pt"
        crossorigin="anonymous"
        async>
</script>

## Referências

<br>

<div id="refs" class="references csl-bib-body hanging-indent">

<div id="ref-Blom1958" class="csl-entry">

Blom, Gunnar. 1958. *Statistical Estimates and Transformed Beta-Variables*. New York: Jonh Wiley & Sons.

</div>

<div id="ref-Filliben1975" class="csl-entry">

Filliben, James J. 1975. “The Probability Plot Correlation Coefficient Test for Normality.” *Technometrics* 17 (1): 111–17. <https://doi.org/10.2307/1268008>.

</div>

<div id="ref-LooneyGulledge1985" class="csl-entry">

Looney, Stephen W., and Thomas R. Gulledge. 1985. “Use of the Correlation Coefficient with Normal Probability Plots.” *The American Statistician* 39 (1): 75–79. <https://doi.org/10.2307/2683917>.

</div>

<div id="ref-RyanJoiner1976" class="csl-entry">

Ryan, Thomas A., and Brian L. Joiner. 1976. “Normal Probability Plots and Tests for Normality.” The Pennsylvania State University, Statistics Department. <https://api.semanticscholar.org/CorpusID:9233652>.

</div>

<div id="ref-ShapiroWilk1965" class="csl-entry">

Shapiro, S. S., and M. B. Wilk. 1965. “An Analysis of Variance Test for Normality (Complete Samples).” *Biometrika* 52 (3/4): 591–611. <http://www.jstor.org/stable/2333709>.

</div>

</div>
