---
description: Sobre nós
keywords:
- sobre
title: Sobre
---


O projeto "*The Puzzle in a Mug*" é um projeto relativamente antigo (uns 10 anos). Já cheguei a colocar ele em prática algumas vezes, mas alguns acontecimentos da vida atrasou seu desenvolvimento e mudou seu rumo.

Hoje o projeto é sobre desenvolvimento de aplicações para analise de dados, com intuito de *aprender* e *entregar* algum tipo de conhecimento interessante para a comunidade. No futuro ele pode virar outra coisa, afinal de contas, a bebida na caneca pode acabar ou perder a graça em algum momento.



{{< figure class="center" width="200" src="https://raw.githubusercontent.com/puzzle-in-a-mug/.github/main/logo.png" alt="Uma caneca vermelha com uma peça de jogo de quebra cabeça desenhada. Dentro da caneca o logo do Python usando um gorro vermelho." >}}


## Pacotes Python

Hoje o projeto conta com dois projetos em andamento já disponíveis no [pypi](https://pypi.org/). Porém, estão em fase alpha de desenvolvimento, e devem ser utilizados com cautela.

### paramcheckup 

O pacote ```paramcheckup``` é apenas uma coleção de funções que verificam se um valor é de um tipo espeficiado. Ele esta sendo desenvolvido para ser utilizado em ```class``` para verificar se os inputs do usuário tem o tipo desejado. Isto resolve uma questão do Python de que os tipos dos inputs das ```class``` e das ```function``` serem apenas hints. Fazer esta verificação logo no inicio do código previne gasto computacional. 

Quando encontra algo não esperado a função retorna um aviso dizendo qual parâmetro esta errado, por que esta errado e em qual linha do código do usuário em que ocorreu o erro. Também é possível mostrar o erro completo gerado pelo compilador Python.

Para instalar o pacote ```paramcheckup``` utilize:

```python
pip install paramcheckup 
```

&nbsp;

{{< figure class="center" width="200"  src="https://raw.githubusercontent.com/puzzle-in-a-mug/paramcheckup/main/docs/_static/logo.png" alt="Logo do pacote paramcheckup" >}}

&nbsp;

A documentação esta disponível em [paramcheckup.readthedocs](https://paramcheckup.readthedocs.io/en/latest/index.html).

### normtest

O pacote ```normtest``` contém ```function``` e ```class``` para aplicar testes de Normalidade em dados experimentais. Cada teste tem seu respectivo módulo e uma classe, que podem ser acessados indivualmente.

Porém, as ```class``` é que devem ser utilizadas para obter resultado de forma simples e rápida: elas retornam os principais resultados, como a estatística do teste, valor crítico, p-valor (quando disponível) e a conclusão do teste (e.g., dados Normais ou dados não Normais). Também permitem acesso a visualizações gráficas, como gráficos com a distribuição esperada do teste. 

Atualmente existem três testes implementados, que são testes que fazem uso do coeficiente de correlação:

* Filliben;
* Looney-Gulledge;
* Ryan-Joiner;



Para instalar o pacote ```normtest``` utilize:

```python
pip install normtest 
```

&nbsp;

{{< figure class="center" width="200" src="https://raw.githubusercontent.com/puzzle-in-a-mug/normtest/main/docs/_static/favicon-180x180.png" alt="Logo do pacote normtest" >}}

&nbsp;

A documentação esta disponível em [normtest.readthedocs](https://normtest.readthedocs.io/en/latest/index.html).


### Outros pacotes

Outros pacotes estão em desenvolvimento e vão ser diponibilizados no futuro ~~não tão~~ próximo:

* ```pygnosis```: avalia a regressão dos dados;
* ```bibmaker```: cria arquivos ```.bib```;
* ```pysorption```: aplica regressão em dados de adsorção;

Existem diversas outras ideias, mas não tenho tempo ~~financiamento~~ no momento.

## Sobre o autor

Quem sou eu? Meu nome é Anderson, tenho alguns anos de idade que conferem tons de cinza na minha barba. Gosto de ler, jogar, estudar, e outros verbos de ação relacionados. Sou meio chato as vezes.


## O que significa *The PuZZle in a Mug*?

That's the puzzle!!

&nbsp;

&nbsp;


