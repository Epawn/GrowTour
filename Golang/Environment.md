# go语言环境踩坑之路

## golang启动之go-sqlite3相关

引入了go-sqlite3的go项目启动报错：
error1:

    C:\Development\Workspace\go\src\site>go run main.go
    # github.com/mattn/go-sqlite3
    exec: "gcc": executable file not found in %PATH%

reason：没有装go-sqlite3驱动

solution：装驱动啊，废话，`go get github.com/mattn/go-sqlite3`

然而，装驱动的时候，又有错：

error2：

    C:\Development\Workspace\go\src\site>go get github.com/mattn/go-sqlite3
    
    出现：exec: "gcc": executable file not found in %PATH%
    
    或者：go get github.com/mattn/go-sqlite3: module github.com/mattn/go-sqlite3: Get https://proxy.golang.org/github.com/mattn/go-sqlite3/@v/list: dial tcp 172.217.27.145:443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.

reason: 被墙了呗，需要依赖golang.org/x/net/context，由于墙的问题 ，无法自动获取。

solution:

* mkdir -p $GOPATH/src/golang.org/x/
* cd $GOPATH/src/golang.org/x/
* git clone https://github.com/golang/net.git net
* go install net

error3:安装gcc
solution:

* 下载：https://sourceforge.net/projects/mingw-w64/files/latest/download
* 安装参考：https://blog.csdn.net/qq_38080117/article/details/78022390
  
吐槽一下：安装要下东西，真的好慢哦

error4: 启动数据库连接报错

    C:\Development\Workspace\go\src\site>go run main.go
    2019/10/11 15:32:02 unable to open database file
    exit status 1

reason: 这是个linux服务器跑的项目，连接数据库的路径风格不对

    // CONF 应用配置
    var CONF = Conf{
      Debug:   true,
      Port:    ":8080",
      DBUrl:   "/tmp/app.db",//这是linux的路径写法
      LogFile: "",
      Salt:    "12345678",
    }

solution: 把DBUrl改为比如：`c:/app.db`

我就遇到这些，完~
