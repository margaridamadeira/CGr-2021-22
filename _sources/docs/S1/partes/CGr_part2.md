---
theme : "beige"
transition: "slide"
highlightTheme: "monokai"
logoImg: "images/logo.png"
slideNumber: true
title: "CGr-2020-21 T2"
---

<!-- .slide: style="text-align: left;" -->
# Enquadramento

<aside class="notes">
Uma perspectiva geral sobre a criação de gráficos 3D
</aside>

---

## Áreas de Computação Gráfica
<!-- 
```{figure} /images/topicos.png
---
height: 100px
name: Tópicos
---
Grandes áreas de CGr.
``` -->

- Modelação (*modelling*)
- Representação (*rendering*)
- Animação (*animation*)


---
<!-- .slide: style="text-align: left;" -->
### Modelação (*modelling*)

Processamento de Vértices
- Converte representações dos objectos de um sistema de coordenadas para outro
Mundo >> Camâra >> Dispositivo

---
<!-- .slide: style="text-align: left;" -->
### Representação (*rendering*)

[Teste as sua sensibilidade](https://area.autodesk.com/fakeorfoto/)

---
<!-- .slide: style="text-align: left;" -->
### Modelação (*modelling*)


- Animação (*animation*)


---

<!-- .slide: style="text-align: left;" -->
## O processo

Gera (ou mostra) uma imagem 2D de uma cena 3D

A cena 3D contém:
- uma câmera virtual: posição e orientação
- fonte(s) de luz - localização, tipo, cor
- objetos 3D: cada um tem posição, orientação e escala
    - textura ou material de cada objeto

---



Processamento de Vértices
Converte representações dos objectos
de um sistema de coordenadas para outro
Mundo >> Camâra >> Dispositivo

Determina cores dos vértices

20

Projecção
Combina informação da câmara com a dos objectos
para produzir imagem 2D
Projecção perspectiva: raios de projecção convergentes
Projecção ortogonal: raios de projecção paralelos

21

Assemblagem de Primitivas
Vértices são associados às suas primitivas
antes de se realizar o recorte
Segmentos de recta, polígonos, curvas e superfícies

22

Recorte

Objectos fora do volume de visualização
são recortados da cena
23

Discretização
Se objecto fica na cena, devem ser associadas cores
aos pixéis correspondentes no frame buffer
Discretizador produz conjunto de fragmentos
para cada objecto

24

Discretização
Fragmentos são “pixéis potenciais” com
atributos cor e profundidade
Atributos dos vértices interpolados
para determinar atributos dos fragmentos
(localização, cor e profundidade)

25

Processamento de Fragmentos
Atributos dos fragmentos usados para determinar a
cor do pixel correspondente no frame buffer
Fragmentos podem ocultar outros
mais distantes da câmara

26

API Gráfica

27

API Gráfica
Componente chave de um sistema gráfico

Colecção de funções para realizar operações básicas
Desenhar Imagens Sintéticas
Representar Objectos 3D
Animar Cenas
...
28

Como criar imagens sintéticas?

Através da API especificam-se
Objectos, Materiais, Câmaras e Fontes de Luz

29

Especificar Objectos
APIs suportam
conjunto limitado de primitivas
Objectos definidos
pela sua localização no espaço ou
pelos seus vértices
30

Especificar um Triângulo
(à antiga)
Tipo de objecto
Localização dos vértices
glBegin(GL_POLYGON)
glVertex3f(0.0, 0.0, 0.0);
glVertex3f(0.0, 1.0, 0.0);
glVertex3f(0.0, 0.0, 1.0);
glEnd( );

31

Especificar um Triângulo
(usando GPU)
1.

Colocar informação geométrica num array
vec3 points[3];
points[0] = vec3(0.0, 0.0, 0.0);
points[1] = vec3(0.0, 1.0, 0.0);
points[2] = vec3(0.0, 0.0, 1.0);

2.

Enviar array para o GPU

3.

Ordenar ao GPU para renderizar um triângulo

32

Especificar Câmaras
Posição do centro de projecção
Orientação da câmara
Campo de visualização
Dimensão da janela

33

Especificar Fontes de Luz

34

Especificar Materiais
Reflexão Ambiente Difusa e Especular

35

Pipeline Gráfico
Primitivas
Câmara
Fontes de Luz

Frame

36

Frame
Animação:
representação sequencial de imagens estáticas
(tal como no cinematógrafo de Lumiére em 1890)

37

Animação

38

Animação em CG
Gerar sequência de frames
Desempenho dado por fps
Quanto mais, melhor… min. 25fps

39

Computação Gráfica Interactiva
Mais que gerar frames em tempo real…
dar ao utilizador o controlo
Resultado produzido depende do input do utilizador

?
