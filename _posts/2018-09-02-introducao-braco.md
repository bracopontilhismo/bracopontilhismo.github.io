---
layout: post
title:  "Introdução do projeto"
date:   2018-09-02 17:54:42 -0300
---
#### Olá leitor!

<p style='text-align: justify;'>
Este blog será dedicado para documentar um projeto que estou desenvolvendo durante o curso de Engenharia Elétrica na UFRN. A ideia principal é controlar um braço robótico, através de servo motores, para desenhar qualquer imagem utilizando a técnica de <a href="https://pt.wikipedia.org/wiki/Pontilhismo" target="_blank">pontilhismo</a>.
</p>

Imagem 1 - Exemplo da tecninca de pontilhismo
<img src="/images/introducao-braco/Exemplo-pontilhismo.svg" alt="Exemplo-pontilhismo" width="600"/>

Fonte: LUCCAS BONATO, Pássaro

<p style='text-align: justify;'>
É cada vez mais recorrente o uso de robôs que exercem atividades que geralmente são feitas por humanos. Este projeto tem fim acadêmico, porém, diversas aplicações podem ser geradas a partir desse trabalho, visto a gama de assuntos envolvidos na sua concepção.

Um protótipo foi feito durante o curso de instrumentação eletrônica no pŕimeiro semestre de 2018. Como o blog será sobre a versão melhorada, o protótipo não será abordado tão detalhadamente.
</p>

## Protótipo


Imagem 2 - protótipo

<img src="/images/introducao-braco/prototipo.svg" alt="protótipo" width="400"/>

<p style='text-align: justify;'>
O protótipo foi construido apenas com <a href="https://store.arduino.cc/usa/arduino-uno-rev3" target="_blank">Arduino Uno Rev3</a>, 4 servo motores TowerPro SG90 e sua estrutura em MDF. A estrutura em MDF é leve e facilita o controle dela pelos servo motores, pois eles têm baixo torque.
</p>



<p style='text-align: justify;'>
A parte de processamento da imagem foi toda feita em <a href="https://www.mathworks.com/products/matlab.html" target="_blank">MATLAB</a>. Pega-se a imagem, retira-se os pontos de borda da imagem utilizando <a href="https://pt.wikipedia.org/wiki/Detector_de_bordas_de_Canny" target="_blank">Canny</a> com o limiar(threshold) escolhido pelo usuário. A partir do resultado anterior é utilizado <a href="http://cs.joensuu.fi/pages/oili/PR/?a=Some__Material&b=Sequential__Clustering/" target="_blank">BSAS</a> para dar o efeito de espaçamento dos pontos.
</p>

#### BSAS

<p style='text-align: justify;'>
<i>Basic Sequential Algorithmic Scheme</i> é um algoritimo utilizado principalmente em agrupamento de dados. O algorítimo pode ser resumidamente descrito assim:
</p>
1. <p style='text-align: justify;'>Escolhe-se um ponto aleatório dos dados para ser o lider do agrupamento;</p>
2. <p style='text-align: justify;'>Agrupa-se, numa mesma classe, todos os dados que estão próximos dele a uma distância 'ρ' escolhida préviamente;</p>
3. <p style='text-align: justify;'>Repete-se o passo dois para um novo lider, aleatóriamente escolhido, que não pertence a nenhuma classe até não haver mais nenhum dado sem classe.</p>

Exemplo:

Imagem 3 - Base de dados aleatóriamente gerada
<img src="/images/introducao-braco/dados.svg" alt="dados" width="500"/>

Imagem 4 - Dados agrupados com ρ=0.7
<img src="/images/introducao-braco/BSASagrupados.svg" alt="agrupados" width="500"/>

#### Processamento da imagem

<p style='text-align: justify;'>
O que foi utilizado no processamento foi a ideia do BSAS em separar dados de acordo com uma distância. Modificou-se o algoritimo para só memorizar a posição dos lideres, invés de agrupar. Com isso dando uma certa característica de pontilhismo.
</p>

Imagem 5 - Imagem original<br>
<img src="/images/introducao-braco/original.svg" alt="original" width="500"/>

Imagem 6 - Imagem processada<br>
<img src="/images/introducao-braco/bsas.svg" alt="bsas" width="500"/>

#### Hardware

<p style='text-align: justify;'>
Há um problema na parte do hardware pois os servo motores só controlam ângulo dos elos. Então, calcula-se a cinemática inversa do braço e seus elos através da notação <a href="https://pt.wikipedia.org/wiki/Par%C3%A2metros_de_Denavit-Hartenberg" target="_blank">Denavit Hartenberg</a> e simplificações algébricas. Com isso se tem controle sobre as coordenadas em XYZ invés de ângulos.
</p>

<p style='text-align: justify;'>
E, por fim, como o pixel é adimensional tem que predefinir o que seria a distância entre um pixel e outro. Utilizou-se a propria bibliotéca do Arduino para controlar cada servo. Para criar o pingo, ou ponto, simplesmente move-se o braço para uma posição acima do papel sem toca-lo. Manda o braço descer, e depois voltar para a posição a cima dele. Repete-se esse movimento toda vez que na imagem processada tiver um pixel branco.
</p>

Imagem 6 - Resultado<br>
<img src="/images/introducao-braco/resultado.svg" alt="resultado" width="500"/>

<p style='text-align: justify;'>
O resultado foi aceitável para o que foi idealizado. Dá para reconhecer a imagem que foi processada. Porém, teve alguns obstáculos e não foram todos resolvidos. Como por exemplo, deslisamento da caneta quando vai criar o ponto. Pretendo resolver isso com sensor de distância nesta nova versão. Mas isso será assunto dos próximos posts.
</p>

Inté!
