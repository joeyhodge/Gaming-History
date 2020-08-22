# [GCC] Unnamed struct/union fields within structs/unions

原来 "Unnamed struct/union fields within structs/unions" 不是 C89 的标准，不过 C1x 就要进入标准了。

gcc 解法：-fms-extensions

 * [参考1][1]
 * [参考2][2]
  
[1]:http://stackoverflow.com/questions/1972003/how-to-use-anonymous-structs-unions-in-c
[2]:http://gcc.gnu.org/onlinedocs/gcc/Unnamed-Fields.html#Unnamed-Fields
