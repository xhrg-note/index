
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