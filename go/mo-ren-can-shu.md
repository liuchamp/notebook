在Golang中，[我们经常碰到要设](https://www.cnblogs.com/smartrui/p/10324320.html)置一个函数的默认值，或者说我定义了参数值，但是又不想传递值，这个在python或php一类的语言中很好实现，但Golang中好像这种方法又不行。在Grpc源码，发现了一个方法可以很优雅的实现，叫做 Functional Options Patter.通过定义函数的方式来实现。

例如：

```go
//** 生成6位的sms验证码
func generate6SmsVerCode(timeOut int64) string {

	rnd := rand.New(rand.NewSource(time.Now().UnixNano()))
	vcode := fmt.Sprintf("%06v", rnd.Int31n(1000000))
	if timeOut <= 0 {
		fmt.Println("not in put code", timeOut)
	}
	return vcode
}

```



