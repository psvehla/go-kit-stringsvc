# go-kit-stringsvc

stringsvc from the go-kit examples

```bash
go build

./go-kit-stringsvc &
[1] 96217

curl -XPOST -d'{"s":"hello, world"}' localhost:8080/uppercase
{"v":"HELLO, WORLD"}

curl -XPOST -d'{"s":"hello, world"}' localhost:8080/count
{"v":12}
```
