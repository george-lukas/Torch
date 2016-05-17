
Come�ando com o Torch
==========================

Torch-7 fornece ambiente semelhante ao Matlab para algoritmos de aprendizado de m�quina quase como uma obra-prima.
� f�cil de usar e fornece uma implementa��o muito eficiente de CPUs e GPUs e usa Lua, uma linguagem simples, semelhante ao javascript.

## Instalando o Torch
Se voc� chegou aqui, voc� deve j� deve ter o Torch instalado (e o iTorch). Se n�o, v� para o [Getting Started](torch.ch/docs/getting-started.html#_) p�gina para instalar ___Torch___ em sua m�quinao. A instala��o � um processo de tr�s simples comandos no Linux e OSX.

## Execu��o de C�digo em um Terminal
__Para este tutorial, vamos contar com a interface notebook iTorch para a execu��o de c�digo e visualiza��o.__

No entanto, se voc� deseja executar o c�digo a partir de linha de comando, o Torch fornece um int�rprete (como python ou MATLAB).
Voc� pode iniciar o interpretador em um terminal e executar comandos diretamente:
```bash
$ th

  ______             __   |  Torch7
 /_  __/__  ________/ /   |  Scientific computing for Lua.
  / / / _ \/ __/ __/ _ \  |
 /_/  \___/_/  \__/_//_/  |  https://github.com/torch
                          |  http://torch.ch

th> torch.Tensor{1,2,3}
 1
 2
 3
[torch.DoubleTensor of dimension 3]

th>
```

Para sair da sess�o interativa, digite ^d ? a chave de controle junto com a tecla d, ou digite `os.exit()`. 

Uma vez que o usu�rio inseriu uma express�o completa, como `1 + 2`, e pressiona [Enter], a sess�o interativa avalia a express�o e mostra o seu valor.

Para avaliar express�es escritas em um arquivo de origem `file.lua`, digite: 
```bash
th> dofile "file.lua".
```
Para executar c�digo em um arquivo n�o-interativamente, voc� pode d�-lo como o primeiro argumento para o comando th:
```bash
$ th file.lua
```

Existem v�rias maneiras de executar c�digo Lua e fornecem op��es, semelhantes aos dispon�veis para programas em `perl` ou `ruby`:
```bash
$ th -h
Usage: th [options] [script.lua [arguments]]

Options:
  -l name            load library name
  -e statement       execute statement
  -h,--help          print this help
  -a,--async         preload async (libuv) and start async repl (BETA)
  -g,--globals       monitor global variables (print a warning on creation/access)
  -gg,--gglobals     monitor global variables (throw an error on creation/access)
  -x,--gfx           start gfx server and load gfx env
  -i,--interactive   enter the REPL after executing a script
```

## iTorch notebooks
O pacote [iTorch](https://github.com/facebook/iTorch) permite f�cil renderiza��o de imagens, 
usando um navegador web padr�o como um cliente gr�ficos.

� usado a UI [IPython](ipython.org/install.html) para permitir a execu��o de c�digo e visualiza��o em um navegador.

Antes de iniciar este tutorial, certifique-se de ter instalado [IPython](ipython.org/install.html) e o [iTorch](https://github.com/facebook/iTorch).

Voc� pode iniciar um notebook iTorch em um terminal da seguinte maneira:
```bash
$ itorch notebook
```

Agora, voc� tem uma interface iTorch no seu browser no http://localhost:8888 

Iniciar um novo notebook para executar seu c�digo. Voc� pode digitar o seu c�digo nas c�lulas de c�digo, e pressione [Shift] + [Enter] juntas e digite seu c�digo.

## Pacotes
Por padr�o, o int�rprete apenas pr�-carrega ___torch___. pacotes adicionais, tais como ___image___ e ___nn___ deve ser requeridas manualmente:


```
require 'nn'
require 'image'
```

## Rendering / Displaying
iTorch permite que voc� exiba imagens, v�deos e �udio.


```
i = image.lena()
itorch.image(i)
```


![png](1_get_started_files/1_get_started_6_0.png)


Nestes tutoriais, vamos estar interessado em visualizar os estados internos e filtros de convolu��o:


```
-- visualizando pesos
n = nn.SpatialConvolution(1,64,16,16)
itorch.image(n.weight)
```


![png](1_get_started_files/1_get_started_8_0.png)



```
-- visualizando estados
n = nn.SpatialConvolution(1,16,12,12)
res = n:forward(image.rgb2y(image.lena()))
-- res here is a 16x501x501 state. We view it now as 16 separate images of size 1x501x501 using the :view function
res = res:view(res:size(1), 1, res:size(2), res:size(3))
itorch.image(res)
```


![png](1_get_started_files/1_get_started_9_0.png)


## Obtendo ajuda
A documenta��o do Torch � conduzido pela comunidade e � sempre um trabalho em andamento.

No entanto, voc� j� pode obter ajuda para a maioria das fun��es previstas nos pacotes oficiais (torch/nn/gnuplot/image etc.).
A documenta��o est� dispon�vel nos reposit�rios git em cada pacote: [torch](https://github.com/torch/torch7/blob/master/README.md), [nn](https://github.com/torch/nn/blob/master/README.md), [image](https://github.com/torch/image/blob/master/README.md) etc.

#### Auto-complete
Uma maneira r�pida de aprender fun��es e explorar os pacotes � usar o TAB-completion, ou seja, come�e a digitar alguma coisa, e depois aperte [TAB] duas vezes:
```bash
th> torch.
Display all 175 possibilities? (y or n)
torch.abs(                  torch.floor(                torch.prod(
torch.acos(                 torch.ge(                   torch.pushudata(
torch.add(                  torch.gels(                 torch.rand(
torch.addcdiv(              torch.Generator.            torch.randn(
```

Isso funciona da mesma maneira, tanto no REPL de linha de comando e itorch.

#### Ajuda on-line
Para cada fun��o que est� documentado, voc� pode obter a sua documenta��o em linha usando o simbolo __?__ prefixado antes do nome da fun��o.

Por exemplo:


```
?torch.randn
```




    
    +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    [1;35m[res] torch.randn([res,] m [,n...]) [0m
    
    
    [0;32m y=torch.randn(n) [0mreturns a one-dimensional tensor of size n filled 
    with random numbers from a normal distribution with mean zero and variance 
    one.
    
    [0;32m y=torch.randn(m,n) [0mreturns a mxn tensor of random numbers from a normal 
    distribution with mean zero and variance one.
    
    For more than 4 dimensions, you can use a storage as argument: [0;32m y=torch.rand(torch.LongStorage{m,n,k,l,o}) 
    [0m
    +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++	
    




## Indo al�m
#####Introdu��o ao Lua
Agora que voc� tem todas as pe�as b�sicas, n�s convidamos voc� a dar uma olhada em um pequeno tutorial de 10 minutos sobre Lua aqui:
__http://learnxinyminutes.com/docs/lua/__

#####O que � o n�cleo do Torch?

O n�cleo Torch � uma biblioteca num�rica geral, combinado com v�rios pacotes de alta qualidade que s�o necess�rias para fazer as tarefas di�rias no torch.

O n�cleo da tocha consiste nos seguintes pacotes:
  * [torch](https://github.com/torch/torch7) : tensors, class factory, serialization, BLAS ;
  * [nn](https://github.com/torch/nn) : neural network Modules and Criterions;
  * [optim](https://github.com/torch/optim) : SGD, LBFGS and other optimization functions ;
  * [gnuplot](https://github.com/torch/gnuplot) : ploting and data visualization ;
  * [paths](https://github.com/torch/paths) : fazer diret�rios, concatenar caminhos de arquivos, e outros utilit�rios do sistema de arquivos ;
  * [image](https://github.com/torch/image) : salvar, carregar, cortar, escalar, warp, traduzir imagens e tais ;
  * [trepl](https://github.com/torch/trepl) : o interpretador torch LuaJIT  ;
  * [cwrap](https://github.com/torch/cwrap) : usado para envolver fun��es C/CUDA em Lua ;

Cada reposit�rio de pacotes geralmente inclui sua pr�pria documenta��o atrav�s de um README.md que pode conter links para uma docs/diret�rio.

Al�m dos pacotes do n�cleo, a distribui��o tamb�m inclui
pacotes que permite a mais rotinas de computa��o intensiva para ser executado
perfeitamente usando gratuitamente o __NVIDIA Graphical Processing Units (GPUs)__ e a
linguagem de programa��o __NVIDIA CUDA__ . Estes pacotes velozes incluem :
 
  * [cutorch](https://github.com/torch/cutorch) : tensors and BLAS ;
  * [cunn](https://github.com/torch/cunn) : Modules and Criterions ;

Para a maior parte, convertendo tensores, m�dulos ou Caracter�sticas de CUDA
� t�o f�cil quanto :


```
require 'cutorch';
a = torch.randn(3,4)        -- create a torch.DoubleTensor()
b = a:cuda()                -- convert to a torch.CudaTensor()
c = torch.CudaTensor(3,4):zero()
d = torch.add(b,c)          -- d = b + c (note that this allocates memory for d)
d:add(b,c)                  -- like the previous line, but reuses existing result memory d
a:copy(d)                   -- copy torch.CudaTensor to back to CPU memory, i.e. torch.DoubleTensor
```

##### Third-party e pacotes da comunidade
Dezenas de pacotes de terceiros t�m crescido em torno dos pacotes do n�cleo do Torch.
Estes est�o listados em Torch [Cheatsheet](https://github.com/torch/torch7/wiki/Cheatsheet).

Voc� pode instalar um pacote de comunidade geralmente usando o gerenciador de pacotes a partir do terminal ___luarocks__ . Por exemplo:
```bash
$ luarocks install penlight
```

## Forums ##

Para suporte, os usu�rios podem pesquisar o Torch
[Google Groups](https://groups.google.com/forum/embed/?place=forum%2Ftorch7#!forum/torch7) and ask questions.
A comunidade � geralmente muito r�pido nas respostas.