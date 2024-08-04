---
title: sqlc 操作数据库
tags: [Go, sqlc, PostgreSQL, 数据库]
date: 2023-05-08 12:00:00
updated: 2023-05-20 20:00:00
categories: 
  - ['Go']
  - ['sqlc']
  - ['PostgreSQL']
  - ['数据库']
cover: https://unpkg.com/keangj-assets@1.0.6/img/go.png


---

# [sqlc](https://sqlc.dev/) 操作数据库 PostgreSQL

## sqlc 基本使用

[安装](https://docs.sqlc.dev/en/latest/overview/install.html)

创建 `sqlc.yaml`

``` yaml
version: "2" # 版本
sql:
  - engine: "postgresql" # 引擎
    queries: "query.sql"
    schema: "schema.sql" # 数据表的描述
    gen:
      go:
        package: "tutorial"
        out: "tutorial" # 输出位置
```

创建数据表文件

``` sql
# schema.sql
# 创建 users 表，拥有 id name created_at updated_at 字段
# sqlc 会根据 schema.sql 生成对应的结构体
CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL UNIQUE,
  created_at TIMESTAMP NOT NULL DEFAULT now(),
  updated_at TIMESTAMP NOT NULL DEFAULT now()
)
```

创建

``` sql
/* query.sql */
/* 会根据下一行注释在 query.sql.go 生成相应的函数 */
/* CreateUser 为函数名；one 会执行对应的方法 */
-- name: CreateUser :one
INSERT INTO users (
  name
) VALUES (
  $1
)
RETURNING *;
```

命令行执行 `sqlc generate` 就会在 `tutorial` 生成 `db.go`  `models.go` `query.sql.go` 三个文件

`db.go` 是生成的包

 `models.go` 是生成的结构体

`query.sql.go` 是生成的方法



## 用 [golang-migrate](https://github.com/golang-migrate/migrate) 数据迁移

### [安装 **golang-migrate**](https://github.com/golang-migrate/migrate/tree/master/cmd/migrate)

``` sh
# mac 安装方法
brew install golang-migrate
migrate --version # 4.x.x
```

### 为数据库添加表

创建迁移文件

``` sh
# -dir migrations 输出目录为 migrations -seq create_users_table 文件名并添加序列号
migrate create -ext sql -dir migrations -seq create_users_table
```

执行命令行后会在 **migrations** 生成 `000001_create_users_table.up.sql` `000001_create_users_table.down.sql` 两个文件

`000001_create_users_table.up.sql` 为升级文件，在文件中添加数据表语句

``` sql
CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  created_at TIMESTAMP NOT NULL DEFAULT now(),
  updated_at TIMESTAMP NOT NULL DEFAULT now()
)
```

`000001_create_users_table.down.sql` 为降级文件，在文件中添加数据表语句

``` sql
DROP TABLE users;
```



运行迁移文件为数据库添加数据表

``` sh
# "<数据库>://<用户名>:<密码>@<域名>:<端口>/<数据库名>?sslmode=disable"
# 升级
migrate -database "postgres://admin:123456@localhost:5432/sql_dev?sslmode=disable" -source "file://$(pwd)/migrations" up
# 降级
# 数字 1 为降级版本数
migrate -database "postgres://admin:123456@localhost:5432/sql_dev?sslmode=disable" -source "file://$(pwd)/migrations" down 1
```



### 为数据表添加字段

再次创建迁移文件

``` sh
migrate create -ext sql -dir migrations -seq add_phone_for_users
```

生成 `000002_add_phone_for_users.up.sql` `000002_add_phone_for_users.down.sql` 两个文件

在升级文件  `000002_add_phone_for_users.up.sql` 中添加

``` sql
ALTER TABLE users ADD COLUMN phone VARCHAR(50) NOT NULL UNIQUE;
```

在降级文件  `000002_add_phone_for_users.down.sql` 中添加

``` sql
ALTER TABLE users DROP COLUMN phone;
```

修改 sqlc 基本使用时创建的 `schema.sql` 文件，添加 **phone address** 字段

``` sql
CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  phone VARCHAR(50) NOT NULL UNIQUE, -- 添加 phone 字段
  address VARCHAR(255) NOT NULL UNIQUE, -- 添加 address 字段
  created_at TIMESTAMP NOT NULL DEFAULT now(),
  updated_at TIMESTAMP NOT NULL DEFAULT now()
)
```

执行升级文件

``` sh
migrate -database "postgres://admin:123456@localhost:5432/sql_dev?sslmode=disable" -source "file://$(pwd)/migrations" up
```



## 用 sqlc 实现 CRUD

修改 `query.sql` 文件

``` sql
-- name: ListUsers :many
SELECT * FROM users
ORDER BY id
OFFSET $1
LIMIT $2;

-- name: GetUser :one
SELECT * FROM users
WHERE id = $1;

-- name: GetUserByEmail :one
SELECT * FROM users
WHERE email = $1;

-- name: GetUserByPhone :one
SELECT * FROM users
WHERE phone = $1;

-- name: DeleteUser :exec
DELETE FROM users
WHERE id = $1;

-- name: CreateUser :one
INSERT INTO users (
  email
) VALUES (
  $1
)
RETURNING *;

-- name: UpdateUser :exec
UPDATE users SET
  email = $1,
  phone = $2,
  address = $3
WHERE
  id = $4;

```

命令行执行 `sqlc generate` 生成新的文件（每次修改 query.sql 和 schema.sql 文件都要重新生成）

连接数据库

``` go
dsn := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable",
		host, port, user, password, dbname)
db, err := sql.Open("postgres", dsn)
if err != nil {
  log.Fatalln(err)
}
DB = db
err = DB.Ping()
if err != nil {
  log.Fatalln(err)
}
```

CRUD

``` go
	// Create
	t := tutorial.New(DB)
	id := rand.Int()
	u, err := t.CreateUser(DBCtx, fmt.Sprintf("%d@qq.com", id))
	if err != nil {
		log.Fatalln(err)
	}
	log.Println(u)
	// Update
	t.UpdateUser(DBCtx, tutorial.UpdateUserParams{
		ID:      u.ID,
		Email:   u.Email,
		Phone:   u.Phone,
		Address: "北京",
	})
	// Read
	users, err := t.ListUsers(DBCtx, tutorial.ListUsersParams{
		Offset: 0,
		Limit:  10,
	})
	if err != nil {
		log.Fatalln(err)
	}
	log.Println(users)
	u, err = t.GetUser(DBCtx, users[0].ID)
	if err != nil {
		log.Fatalln(err)
	}
	log.Println(u)
	// Delete
	err = t.DeleteUser(DBCtx, u.ID)
	if err != nil {
		log.Fatalln(err)
	}
```

