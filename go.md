在Golang中，我们经常碰到要设置一个函数的默认值，或者说我定义了参数值，但是又不想传递值，这个在python或php一类的语言中很好实现，但Golang中好像这种方法又不行。今天在看Grpc源码时，发现了一个方法可以很优雅的实现，叫做 Functional Options Patter.通过定义函数的方式来实现

  


