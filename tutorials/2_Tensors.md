
## Tensor
A classe ___Tensor___ classe é provavelmente o mais importante na classe ___Torch___. Quase todos os pacotes depende dessa classe. É A ( __The__ ) classe para a manipulação de dados numéricos 

Tal como acontece com praticamente qualquer coisa em ___Torch___, tensores são serializáveis e desejáveis. o que isso significa é que você pode converter um tensor para uma cadeia (que você pode guardá-lo em um arquivo no disco), e carregá-lo de volta.
### Matriz multi-dimensional
Um ___Tensor___ é potencialmente uma matriz multi-dimensional. O número de dimensões é ilimitado, que pode ser criada usando ___LongStorage___ com mais dimensões.

Exemplo: 


```
--- Criação de um 4D-tensor 4x5x6x2
z = torch.Tensor(4,5,6,2)
--- para mais dimensões, (aqui um 6D tensor) que pode ser definido como:
s = torch.LongStorage(6)
s[1] = 4; s[2] = 5; s[3] = 6; s[4] = 2; s[5] = 7; s[6] = 3;
x = torch.Tensor(s)
```

O número de dimensões de um Tensor pode ser consultado pelo ___nDimension()___ ou ___dim()___. O tamanho da i-ézima dimensão é retornada por ___size(i)___. ___LongStorage___ contém todas as dimensões que podem ser retornadas por ___size()___.


```
print(x:nDimension())
```




    6	





```
print(x:size())
```




    
     4
     5
     6
     2
     7
     3
    [torch.LongStorage of size 6]
    




### Representação de dados interna
Os dados reais de um ___Tensor___ está contido numa ___Storage___. Ele pode ser acessado usando ___storage()___. Enquanto a memória de um ___Tensor___ tem de ser contida neste única ___Storage___, ela pode não ser __contígua__: a primeira posição utilizado no ___Storage___ é dado por ___storageOffset()___ (starting at 1). E o salto necessário para ir de um elemento para outro elemento na dimensão i-th é dada por ___stride(i)___. Por outras palavras, dado um 3D Tensor


```
x = torch.Tensor(7,7,7)
```

acessando os elementos (3,4,5) pode ser feito atravéz de:


```
x[3][4][5]
```




    0	




ou de modo equivalente "under the hood" (Porém mais lento!)


```
x:storage()[x:storageOffset()
           +(3-1)*x:stride(1)
           +(4-1)*x:stride(2)
           +(5-1)*x:stride(3)
           ]
```




    0	




Pode-se dizer que uma ___Tensor___ é um modo particular de ver um ___Storage___: um ___Storage___ representa apenas um pedaço de memória, quando o ___Tensor___ interpreta este pedaço de memória como tendo dimensões:


```
x = torch.Tensor(4,5)
s = x:storage()
for i=1,s:size() do -- preenchendo o armazenamento
    s[i] = i
end
print(x)
```




      1   2   3   4   5
      6   7   8   9  10
     11  12  13  14  15
     16  17  18  19  20
    [torch.DoubleTensor of dimension 4x5]
    




Note-se também que, em elementos do Torch na mesma linha [elementos ao longo da última dimensão] são contíguas na memória por um Tensor:


```
x = torch.Tensor(4,5)
i = 0
x:apply(function()
     i = i + 1
     return i
    end)
print(x)
```




      1   2   3   4   5
      6   7   8   9  10
     11  12  13  14  15
     16  17  18  19  20
    [torch.DoubleTensor of dimension 4x5]
    





```
x:stride() --Elemento na última dimensão são contíguos!
```

Isso é exatamente como em __C__ (e não __Fortran__).

### Tensors de tipos de diferentes
Na verdade, existem vários tipos de Tensor:
* ByteTensor -- contém unsigned chars
* CharTensor -- contém signed chars
* ShortTensor -- contém shorts
* IntTensor -- contém ints
* FloatTensor -- contém floats
* DoubleTensor -- contém doubles

A maioria das operações numéricas são implementadas apenas para ___FloatTensor___ e ___DoubleTensor___. Outros tipos de ___Tensor___ são úteis se você quiser economizar espaço de memória.

### Tipo padrão de Tensor
Por conveniência, um apelido é dado para ___torch.Tensor___, que permite ao usuário escrever scripts independente do tipo, que pode, em seguida, que pode ser rodado depois de escolher tipo ___Tensor___ desejado numa uma chamada como


```
torch.setdefaulttensortype('torch.FloatTensor')
```




    




Veja ___torch.setdefaulttensortype___ para mais detalhes. Por padrão, é dado o apelido  "points" para ___torch.DoubleTensor___.


```

```




    





```

```
