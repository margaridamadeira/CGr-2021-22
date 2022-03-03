(prepP)=
# Preparar o ambiente de trabalho

O ambiente de trabalho deve permitir o desenvolvimento de programas usando OpenGL em mode `core`. O sistema operativo e linguagem de programação é uma escolha do aluno.

O ambiente de trabalho de referência é Windows 10, usando Python (> 3.8).

Uma máquina do Pólo de Visualização Científica da UAlg está disponível para uso partilhado (um utilizador de cada vez) e será a máquina de referência para execução dos trabalhos finais.

Essa máquina (10.38.5.146) pode ser usada pelos alunos inscritos em Computação Gráfica qundo há aulas práticas (manhãs de terça-feira e quinta-feira) e todos os dias a partir das 18h.

A ligação à máquina é feita na rede Ualg com
+  `Remote Desktop Connection` em Windows
+ [Microsoft Remote Desktop](https://apps.apple.com/vg/app/microsoft-remote-desktop/id1295203466?mt=12) para Mac
+ e aplicação semelhante em Linux; Remmina em Debian 11.2 (bullseye)
usando as suas credenciais.


## Pré-requisitos

Antes de iniciar, verifique que tem os *drivers* da carta gráfica atualizados.
Precisará do [Python](https://www.python.org/downloads/). Descarregue e instale.


## Instalação

Crie uma pasta para CGr

Nessa pasta, abra o terminal de comando (`cmd`)

Crie um ambiente virtual, fazendo
```
mkdir C:\Users\xxxxx\venvs
py -m venv C:\Users\xxxxx\venvs\CGr
```

Ative o ambiente virtual
```
C:\Users\xxxxx\venvs\CGr\Scripts\activate
```

Obtenha o [repositório](https://github.com/margaridamadeira/CGr-P) com o código partilhado, descarregando o zip ou clonando o repositório.

Instale os requisitos listados no ficheiro `requirements.txt` que está na pasta E1 fazendo 
```
py -m pip install -r E1\requirements.txt
```

Se tudo correu bem, ótimo. Se algo não correu bem, peça ajuda ao docente em sala.

## Verificação da instalação

Mude para a pasta `src1 fazendo

```
cd src1
```

e experimente correr o script `testInstall.py` fazendo
```
py testInstall.py
```

Se tudo correu bem, surgiu a janela {ref}`janelaVazia`.

```{figure} ../../images/black-window.png
---
width: 500px
name: janelaVazia
---
A janela da etapa Aplicação.
```

Só falta agora analisar o código em `src1` e identificar cada uma das partes.

Bom trabalho!



