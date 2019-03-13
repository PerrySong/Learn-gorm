# How to run

```
$ git clone https://github.com/PerrySong/Learn-gorm.git
$ cd Learn-gorm
$ dep ensure
$ go run main.go
```

# Example

```
package main
  
  import (
  	"github.com/jinzhu/gorm"
  	_ "github.com/jinzhu/gorm/dialects/postgres"
  	"time"
  )
  
  type Owner struct {
  	gorm.Model
  	FirstName string
  	LastName string
  }
  
  type Book struct {
  	gorm.Model
  	Name string
  	Publish time.Time
  	OwnerID uint `sql:"index"`
  	Authors []Author `gorm:"many2many:books_authors"`
  }
  
  type Author struct {
  	gorm.Model
  	FirstName string
  	LastName string
  }
  
  func main() {
  	db, err := gorm.Open("postgres", "user=postgres password=postgres dbname=learngorm sslmode=disable")
  	db.SingularTable(true)
  	if err != nil {
  		panic(err.Error())
  	}
  	//err = db.DB().Ping()
  	//if err != nil {
  	//	fmt.Println(err)
  	//}
  	defer db.Close()
  	db.DropTable(&Owner{})
  	db.CreateTable(&Owner{})
  
  	owner := Owner{
  		FirstName: "Perry",
  		LastName: "Song",
  	}
  
  	db.Debug().Create(&owner)
  
  	owner.FirstName = "Candice"
  
  	db.Debug().Save(owner)
  	db.Debug().Delete(&owner)
  }
  ````