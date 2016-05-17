
## Tensor
The ___Tensor___ class is probably the most important class in ___Torch___. Almost every package depends on this class. It is __the__ class for handling numeric data. 

As with pretty much anything in ___Torch___, tensors are serializable and deserializable. What that means is that you can convert a tensor to a string (and save it as a file to disk), and load it back.

### Multi-dimensional matrix
A ___Tensor___ is a potentially multi-dimensional matrix. The number of dimensions is unlimited that can be created using ___LongStorage___ with more dimensions.

Example: 


```
--- creation of a 4D-tensor 4x5x6x2
z = torch.Tensor(4,5,6,2)
--- for more dimensions, (here a 6D tensor) one can do:
s = torch.LongStorage(6)
s[1] = 4; s[2] = 5; s[3] = 6; s[4] = 2; s[5] = 7; s[6] = 3;
x = torch.Tensor(s)
```

The number of dimensions of a Tensor can be queried by ___nDimension()___ or ___dim()___. Size of the i-th dimension is returned by ___size(i)___. A ___LongStorage___ containing all the dimensions can be returned by ___size()___.


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
    




### Internal data representation
The actual data of a ___Tensor___ is contained into a ___Storage___. It can be accessed using ___storage()___. While the memory of a ___Tensor___ has to be contained in this unique ___Storage___, it might not be __contiguous__: the first position used in the ___Storage___ is given by ___storageOffset()___ (starting at 1). And the jump needed to go from one element to another element in the i-th dimension is given by ___stride(i)___. In other words, given a 3D tensor


```
x = torch.Tensor(7,7,7)
```

accessing the element (3,4,5) can be done by


```
x[3][4][5]
```




    0	




or equivalently under the hood (but slowly!)


```
x:storage()[x:storageOffset()
           +(3-1)*x:stride(1)
           +(4-1)*x:stride(2)
           +(5-1)*x:stride(3)
           ]
```




    0	




One could say that a ___Tensor___ is a particular way of viewing a ___Storage___: a ___Storage___ only represents a chunk of memory, while the ___Tensor___ interprets this chunk of memory as having dimensions:


```
x = torch.Tensor(4,5)
s = x:storage()
for i=1,s:size() do -- fill up the Storage
    s[i] = i
end
print(x)
```




      1   2   3   4   5
      6   7   8   9  10
     11  12  13  14  15
     16  17  18  19  20
    [torch.DoubleTensor of dimension 4x5]
    




Note also that in Torch elements in the same row [elements along the last dimension] are contiguous in memory for a Tensor:


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
x:stride() -- element in the last dimension are contiguous!
```

This is exactly like in __C__ (and not __Fortran__).

### Tensors of different types
Actually, several types of Tensor exist:
* ByteTensor -- contains unsigned chars
* CharTensor -- contains signed chars
* ShortTensor -- contains shorts
* IntTensor -- contains ints
* FloatTensor -- contains floats
* DoubleTensor -- contains doubles

Most numeric operations are implemented only for ___FloatTensor___ and ___DoubleTensor___. Other ___Tensor___ types are useful if you want to save memory space.

### Default Tensor type
For convenience, an alias ___torch.Tensor___ is provided, which allows the user to write type-independent scripts, which can then ran after choosing the desired ___Tensor___ type with a call like


```
torch.setdefaulttensortype('torch.FloatTensor')
```




    




See ___torch.setdefaulttensortype___ for more details. By default, the alias "points" to ___torch.DoubleTensor___.


```

```




    





```

```
