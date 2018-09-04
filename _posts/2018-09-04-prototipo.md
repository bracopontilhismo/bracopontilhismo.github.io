---
layout: post
title:  "Resumo do protótipo"
date:   2018-09-04 01:20:15 -0300
---
#### Olá leitor!

Neste post estarei relatando o que foi feito no protótipo do projeto, carinhosamente apilidado de Astolfinho.

<!--more-->

## Introdução

Este protótipo foi desenvolvido por mim, com uma ajudinha de [Weny Maysa](https://wenyaraujo.github.io/). O objetivo do projeto é utilizar um braço robótico para desempenhar a tarefa de desenhar uma imagem dada pelo usuário com a técnica de pontilhismo, como já mencionado no [post anterior](/2018/09/02/introducao-braco.html).

## Metodologia

No protótipo foi utilizado:
- [Arduino Uno Rev3](https://store.arduino.cc/usa/arduino-uno-rev3) (placa e [software](https://www.arduino.cc/en/Main/Software))
- [MATLAB](https://www.mathworks.com/products/matlab.html) (software)
- 4 Servo motores TowerPro SG90
- Braço Robótico MDF

A estrutura do braço robótico é feita em MDF que é relativamente leve e facilita para que os servo motores possa aplicar o menor torque possivel para mover cada junta. Pois, os servor motores utilizados geram torque relativamente baixo.

Após de utilizar a Notação Denavit Hartenberg para a atribuição de referencias de elo (figura 1). Foi retirado a cinemática direta e com isso após de varios cálculos e simplificações algébricas conseguimos a cinemática inversa para calcular os angulos dos servos dado a posição XYZ no espaço (figura 2).

Figura 1 - Referenciais de elo

<img src="/images/prototipo/referencial.png" alt="referencial" width="600"/>
<br><br><br><br>

Figura 2 - Matriz de cinemática inversa

<img src="/images/prototipo/cinematica-inversa.png" alt="cinemática inversa" width="600"/>

O MATLAB foi utilizado para o processamento da imagem. A ideia que utilizamos foi detectar as bordas da imagem utilizando Canny. Após a detecção das bordas, para criar o efeito de pontilhismo foi utilizado uma tecnica de agrupamento de dados chamado de BSAS, "Basic Sequential Algorithmic Scheme", com isso reduzimos varios pixels num certo raio de um pixel central como um só. Resultando o seguinte efeito:

Figura 3 - Bordas detectadas
<img src="/images/prototipo/canny.png" alt="canny" width="600"/>

Figura 4 - aplicação do BSAS
<img src="/images/prototipo/bsas.png" alt="bsas" width="600"/>

Utilizamos os pontos finais do BSAS e a matriz de cinemática inversa para plotar o os pontos no papel. Resultando no que já foi mostrado no [post anterior](/2018/09/02/introducao-braco.html).

Figura 5 - Imagem original

<img src="/images/prototipo/prototipo-original.jpg" alt="original" width="200"/>

Figura 6 - Resultado

<img src="/images/prototipo/prototipo-resultado.png" alt="resultado" width="200"/>
