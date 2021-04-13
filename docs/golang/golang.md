goland配置

* GOROOT，这个指的是golang的sdk的安装目录
* GOPATH，这个可以不用设置，因为现在已经不用了。但是默认应该会有一个userHome的go目录
* Go Modules，勾选enable，然后再环境变量设置增加GO111MODULE=auto


golang编译

```
//linux
GOOS=linux GOARCH=amd64 go build

//mac 
GOOS=darwin GOARCH=amd64 go build

//windows
GOOS=windows GOARCH=amd64 go build
```

---

golang反射笔记

```

安装grpcurl
go get github.com/fullstorydev/grpcurl
go install github.com/fullstorydev/grpcurl/cmd/grpcurl

查看服务列表
grpcurl -plaintext localhost:1535 list

查看接口的详细描述
grpcurl -plaintext localhost:1535 describe serviceName
```
