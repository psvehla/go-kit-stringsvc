# go-kit-stringsvc

stringsvc from the go-kit examples

## Simple example

```bash
go build

./go-kit-stringsvc &
[1] 96217

curl -XPOST -d'{"s":"hello, world"}' localhost:8080/uppercase
{"v":"HELLO, WORLD"}

curl -XPOST -d'{"s":"hello, world"}' localhost:8080/count
{"v":12}
```

## Proxying example

```bash
./go-kit-stringsvc -listen=:8001 &
[1] 25202
listen=:8001 caller=proxying.go:25 proxy_to=none
listen=:8001 caller=main.go:73 msg=HTTP addr=:8001

./go-kit-stringsvc -listen=:8002 &
[2] 25229
listen=:8002 caller=proxying.go:25 proxy_to=none
listen=:8002 caller=main.go:73 msg=HTTP addr=:8002

./go-kit-stringsvc -listen=:8003 &
[3] 25282
listen=:8003 caller=proxying.go:25 proxy_to=none
listen=:8003 caller=main.go:73 msg=HTTP addr=:8003

./go-kit-stringsvc -listen=:8080 -proxy=localhost:8001,localhost:8002,localhost:8003 &
[4] 25644
listen=:8080 caller=proxying.go:42 proxy_to="[localhost:8001 localhost:8002 localhost:8003]"
listen=:8080 caller=main.go:73 msg=HTTP addr=:8080

for s in foo bar baz ; do curl -d"{\"s\":\"$s\"}" localhost:8080/uppercase ; done
listen=:8001 caller=logging.go:22 method=uppercase input=foo output=FOO err=null took=516ns
listen=:8080 caller=logging.go:22 method=uppercase input=foo output=FOO err=null took=13.467318ms
{"v":"FOO"}
listen=:8002 caller=logging.go:22 method=uppercase input=bar output=BAR err=null took=435ns
listen=:8080 caller=logging.go:22 method=uppercase input=bar output=BAR err=null took=3.513567ms
{"v":"BAR"}
listen=:8003 caller=logging.go:22 method=uppercase input=baz output=BAZ err=null took=472ns
listen=:8080 caller=logging.go:22 method=uppercase input=baz output=BAZ err=null took=1.344947ms
{"v":"BAZ"}
```
