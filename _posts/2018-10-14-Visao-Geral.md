---
layout: post
title:  "Visão Geral"
date:   2018-10-14 20:04:32 -0300
comments:  true
---
#### Olá leitor!

Neste post será mostrado a visão geral como é feito desde a imagem até a plotagem pelo braço robótico. Não será explicado em detalhes como foi feito, mas sim dará a ideia necessaria para reprodizuir o projeto.

## Fluxograma

A figura abaixo mostra fluxograma do processo. Para ser mais didático a visão geral será dividido em duas partes:
1. Processamento da imagem;
2. Controle mecânico.

<img src="/images/visao-geral/visao-geral-completa.svg" alt="Visão Geral" width="650"/>

### Processamento da imagem

<img src="/images/visao-geral/visao-geral1-2.svg" alt="Processamento da imagem" width="650"/>

- **Leitura da imagem em escala cinza:** por questão de praticidade foi escolhido trabalhar com imagens resultantes monocromáticas, por isso temos essa etapa no fluxograma;

- **Detecção de bordas:** é utilizado o Canny para detectar as bordas da imagem;

- **Aplicação do BSAS:** nesta etapa, se utiiza os pontos de borda do Canny como base de dado do BSAS. O que é retirado desse processamento é os centros dos *clusters* achados pelo algorítimo. Também é utilizado um "rho", distância mínima entre os *clusters*, pequeno para dar um pouco mais de nítides nas bordas da imagem;

- **Aplicação do K-medias:** para dar um efeito de coloarção o histograma da foto é quantizado em *K* tons, utilizado o algoritimo K-médias de cinza e depois a imagem é reconstruida com este novo histograma.

- **Aplicação do BSAS em cada centro do k-médias:** nesta etapa, se utiliza como base de dados para o BSAS os *K* grupos retirado do K-médias. E o "rho" é definido através de uma função de primeiro grau, rho = A * Centro + B. Onde A e B são constantes definidas pelo usuário, e o Centro, é o ton de cinza que está sendo analisado. Resumindo, separo as *K* amostras e vou pegando todos os pixels que pertence a cada ton desse, vou aplicando o BSAS e quanto mais escuro for o tom, menor vai ser o espaçamento entre os pontos.

- **Junção dos 2 processamentos:** após os 2 processamentos no Canny e no K-médias é juntado todos os pixels em uma imagem só.

### Controle mecânico

<img src="/images/visao-geral/visao-geral2-2.svg" alt="Controle mecânico" width="650"/>

Antes de começar essa parte, deve-se mapear qual será os limites da imagem. Tipo, se queremos que a imagem seja desenhado num quadrado de 10cm por 10cm. Devemos dividir os 10cm pelo número de píxeis.

- **Leitura de pixel:** O programa vasculha a imagem pixel a pixel pŕocurando os pontos marcados, que no caso seria pontos pretos, 0 na escala cinza.

- **Cinemática inversa:** Utilizando as coordenadas previamente mapeadas dos píxeis, pegamos a posição do píxel atual e aplicamos a cinemática inversa. Com isso, obtemos os ângulos necessário de cada junta para atingir tal posição.

- **Conversão dos angulos em PWM:** Foi criado uma equação para traduzir o ângulo dado em graus para a largura de pulso do PWM, [mais informações](http://home.roboticlab.eu/pt/examples/motor/servo). Essa equação foi criada pegando 3 ângulos diferentes e montando uma parábola para conversão, por exemplo pegando os ângulos 0, 90 e 180 graus e vendo qual largura de PWM é cada um deles. Após a conversão é mandada o sinal de PWM para os servo motores.

- **Pingar o píxel no papel:** Ao chegar na posição designada mandamos ele descer no eixo Z até chegar no papel e subir de volta pra posição anterior.

- **Acabou os píxeis da imagem?:** Nessa etapa ele verifica se este é o ultimo píxel da imagem. Caso não for, ele retorna a Leitura do próximo píxel e refaz todos esses passo. Caso seja o último píxel ele termina por aqui.

Recapitulando, neste post foi descrito a visão geral do projeto. Desda parte de software, do processamento da imagem, até o controle mecânico do braço.

Inté!

