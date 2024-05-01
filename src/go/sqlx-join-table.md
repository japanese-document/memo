{ "category": "Go",  "order": 8, "date": "2024-05-02 00:30" }
---
# sqlxでjoinしたテーブルのデータを構造体にマッピングする

以下のように[struct tags](https://go.dev/wiki/Well-known-struct-tags)を付与したテーブルの型をjoinに対応した型に埋め込みます。

```go
package main

import (
	"fmt"
	"log"

	"github.com/jmoiron/sqlx"
	_ "github.com/mattn/go-sqlite3"
)

type User struct {
	ID    int    `db:"id"`
	Name  string `db:"name"`
	Email string `db:"email"`
	Age   int    `db:"age"`
}

type Car struct {
	ID     int    `db:"id"`
	UserID int    `db:"user_id"`
	Maker  string `db:"maker"`
	Model  string `db:"model"`
	Year   int    `db:"year"`
}

type CarUser struct {
	User User `db:"u"`
	Car  Car  `db:"c"`
}

func main() {
	db := sqlx.MustOpen("sqlite3", ":memory:")
	defer db.Close()

	createUserTableSQL := `CREATE TABLE IF NOT EXISTS user (
		id INTEGER PRIMARY KEY AUTOINCREMENT,
		name TEXT NOT NULL,
		email TEXT UNIQUE NOT NULL,
		age INTEGER
	);`
	db.MustExec(createUserTableSQL)

	createCarTableSQL := `CREATE TABLE IF NOT EXISTS car (
		id INTEGER PRIMARY KEY AUTOINCREMENT,
		user_id INTEGER NOT NULL,
		maker TEXT NOT NULL,
		model TEXT NOT NULL,
		year INTEGER,
		FOREIGN KEY (user_id) REFERENCES user (id) ON DELETE CASCADE
	);`
	db.MustExec(createCarTableSQL)

	insertUserData := `INSERT INTO user (name, email, age) VALUES (?, ?, ?)`
	users := []User{
		{Name: "Foo", Email: "foo@example.com", Age: 30},
		{Name: "Bar", Email: "bar@example.com", Age: 25},
	}

	for _, user := range users {
		result := db.MustExec(insertUserData, user.Name, user.Email, user.Age)
		userID, _ := result.LastInsertId()

		insertCarData := `INSERT INTO car (user_id, maker, model, year) VALUES (:user_id, :maker, :model, :year)`
		cars := []Car{
			{UserID: int(userID), Maker: "Toyota", Model: "Corolla", Year: 2020},
			{UserID: int(userID), Maker: "Honda", Model: "Civic", Year: 2018},
		}

		_, err := db.NamedExec(insertCarData, cars)
		if err != nil {
			log.Fatal("Failed to execute query: ", err)
		}
	}

	query := `
	SELECT u.id as "u.id", u.name as "u.name", u.email as "u.email", u.age as "u.age", 
	       c.id as "c.id", c.user_id as "c.user_id", c.maker as "c.maker", c.model as "c.model", c.year as "c.year"
	FROM car c
	JOIN user u ON u.id = c.user_id`

	var carUsers []CarUser
	err := db.Select(&carUsers, query)
	if err != nil {
		log.Fatal("Failed to execute query: ", err)
	}

	fmt.Printf("%+v", carUsers)
}
```