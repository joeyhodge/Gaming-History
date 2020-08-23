# Benchmark - go vs python vs pypy

看了[这篇benchmark][1]，打算自己也测试下。

我测试中，C版本在range变大的情况下，果然是 constant time，估计和文中C/C++编译器的优化有关。

我主要对比下 go 和 pypy 的性能。

编译器版本

```
openbsd 6.3 virtualbox

go-1.10
python2.7
PyPy 5.3.1(python2.7)
```

go代码

```go
// go-sum.go
package main

import "fmt"

func main() {
        sum := 0
        for i := 0; i < 50000000; i++ {
                sum += i
        }
        fmt.Println(sum)
}
```

python代码

```python
# py-sum.py                           
result = 0
for a in xrange(50000000):
        result += a
print result
```

运行结果

```
$ time ./go-sum
1249999975000000
    0m00.03s real
    0m00.03s user
    0m00.01s system

$ time python py-sum.py
1249999975000000
    0m04.06s real
    0m04.03s user
    0m00.03s system

$ time pypy py-sum.py
1249999975000000
    0m02.92s real
    0m00.15s user
    0m01.04s system

$ time pypy py-sum.py 
1249999975000000
    0m00.21s real
    0m00.15s user
    0m00.05s system

$ time pypy py-sum.py 
1249999975000000
    0m00.20s real
    0m00.11s user
    0m00.07s system
```

pypy 第一次运行会做 JIT，所以会慢一些。之后再运行，速度就快起来了。

go 和 pypy 速度，大约在 1:5。

[1]:http://karlheinzniebuhr.github.io/en/2015/09/28/C-vs-Go-vs-pypy-vs-Python/