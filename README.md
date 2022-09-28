#### Give a star before you see it. Ha ha ha ~ ~

Generates a api file from your mysql database.
仿照sql2pb写的生成通过 mysql database生成 gozero api文件
里面加了一些自己项目的理解，本着生成出来复制总比手敲好的原则还是挺好用的。

### Uses
#### Use from the command line:

`go install github.com/xiafei114/sql2api@latest`
可能不好用，pull下来自己 go install吧

api最好配本人修改的sql2pb生成rpc服务
https://github.com/xiafei114/sql2pb

```
$ sql2api -h

Usage of sql2api:
  -db string
        the database type (default "mysql")
  -go_package string
        the protocol buffer gp_package. defaults to the database schema.
  -host string
        the database host (default "localhost")
  -ignore_tables string
        a comma spaced list of tables to ignore
  -package string
        the protocol buffer package. defaults to the database schema.
  -password string
        the database password (default "root")
  -port int
        the database port (default 3306)
  -schema string
        the database schema
  -service_name string
        the protobuf service name , defaults to the database schema.
  -table string
        the table schema，multiple tables ',' split.  (default "*")
  -user string
        the database user (default "root")

```

```
$ sql2api -go_package ./pb -host localhost -package pb -password root -port 3306 -schema usercenter -service_name usersrv -user root > usersrv.proto
```



#### Use as an imported library

```sh
$ go get -u github.com/xiafei114/sql2api@latest
```

```go
package main

import (
	"database/sql"
	"fmt"
	"github.com/xiafei114/sql2api/core"
	_ "github.com/go-sql-driver/mysql"
	"log"
)

func main() {

	dbType:= "mysql"
	connStr := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s", "root", "root", "127.0.0.1", 3306, "zero-demo")
	pkg := "my_package"
	goPkg := "./my_package"
	table:= "*"
	serviceName:="usersrv"

	db, err := sql.Open(dbType, connStr)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	s, err := core.GenerateSchema(db, table,nil,serviceName, goPkg, pkg)

	if nil != err {
		log.Fatal(err)
	}

	if nil != s {
		fmt.Println(s)
	}
}
```

#### Thanks for schemabuf
    schemabuf : https://github.com/mcos/schemabuf
    sql2pb : https://github.com/Mikaelemmmm/sql2pb
