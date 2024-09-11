# Golang

## Mirrors

[https://golang.org/dl/](https://golang.org/dl/)

[https://golang.google.cn/dl/go1.21.5.linux-amd64.tar.gz](https://golang.google.cn/dl/go1.21.5.linux-amd64.tar.gz)

- `wget https://golang.google.cn/dl/go1.21.5.linux-amd64.tar.gz`
- `tar -xvf ./go1.21.5.linux-amd64.tar.gz -C /usr/local`
- `vim ./.zshrc`

```Bash
# Go
export GOVERSION=xxxx
export GOROOT=/usr/local/go
export GOPATH=xxxxx
export GOBIN=$GOROOT/bin
export PATH=$GOBIN:$GOPATH/bin:$PATH
export GOPROXY=https://goproxy.cn,direct
export GO111MOUDLE="on"
```