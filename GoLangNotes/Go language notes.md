### Go language notes

#### Stacked ```defer```s run in reverse order.
+ what about sperate ```defer```s in the function?

#### GOOS and GOARCH for cross-compilation
+ get a list of all the GOOS and GOARCH combinations supported by your Go version by running the command ```go tool dist list```
+ to build for specific platform:
	+ ```GOOS=linux GOARCH=arm go build```

#### Encoding JSON with Struct Tags
+ [reference](https://www.digitalocean.com/community/tutorials/how-to-use-struct-tags-in-go)
+ without tags, only exposed struct fields will be encoded with their original field names:
	+ sample code snippets:
	```
	package main
	import (
	    "encoding/json"
	    "fmt"
	    "log"
	    "os"
	    "time"
	)
	type User struct {
	    Name          string
	    Password      string
	    PreferredFish []string
	    CreatedAt     time.Time
	}
	func main() {
	    u := &User{
	        Name:      "Sammy the Shark",
	        Password:  "fisharegreat",
	        CreatedAt: time.Now(),
	    }
	    out, err := json.MarshalIndent(u, "", "  ")
	    if err != nil {
	        log.Println(err)
	        os.Exit(1)
	    }
	    fmt.Println(string(out))
	}
	```
	+ output:
	```
	{
	  "Name": "Sammy the Shark",
	  "Password": "fisharegreat",
	  "CreatedAt": "2019-09-23T15:50:01.203059-04:00"
	}
	```
	+ sample:
	```
	package main
	import (
	    "encoding/json"
	    "fmt"
	    "log"
	    "os"
	    "time"
	)
	type User struct {
	    name          string
	    password      string
	    preferredFish []string
	    createdAt     time.Time
	}
	func main() {
	    u := &User{
	        name:      "Sammy the Shark",
	        password:  "fisharegreat",
	        createdAt: time.Now(),
	    }
	    out, err := json.MarshalIndent(u, "", "  ")
	    if err != nil {
	        log.Println(err)
	        os.Exit(1)
	    }
	    fmt.Println(string(out))
	}
	```
	+ output:
	```
	{}
	```
+ using struct tags to control encoding
	+ sample
	```
	package main

	import (
	    "encoding/json"
	    "fmt"
	    "log"
	    "os"
	    "time"
	)

	type User struct {
	    Name          string    `json:"name"`
	    Password      string    `json:"password"`
	    PreferredFish []string  `json:"preferredFish"`
	    CreatedAt     time.Time `json:"createdAt"`
	}

	func main() {
	    u := &User{
	        Name:      "Sammy the Shark",
	        Password:  "fisharegreat",
	        CreatedAt: time.Now(),
	    }

	    out, err := json.MarshalIndent(u, "", "  ")
	    if err != nil {
	        log.Println(err)
	        os.Exit(1)
	    }

	    fmt.Println(string(out))
	}
	```
	+ output:
	```
	{
	  "name": "Sammy the Shark",
	  "password": "fisharegreat",
	  "preferredFish": null,
	  "createdAt": "2019-09-23T18:16:17.57739-04:00"
	}
	```
	+ ```omitempty``` tag to remove empty JSON fields:
	```
	// change the struct definition in the above example to this:
	type User struct {
	    Name          string    `json:"name"`
	    Password      string    `json:"password"`
	    PreferredFish []string  `json:"preferredFish,omitempty"`
	    CreatedAt     time.Time `json:"createdAt"`
	}
	```
	+ output:
	```
	{
	  "name": "Sammy the Shark",
	  "password": "fisharegreat",
	  // preferredFish field omitted
	  "createdAt": "2019-09-23T18:16:17.57739-04:00"
	}
	```
	+ ignoring private fields with ``` "-" ``` tag
	```
	// change the struct definition in the above example to this:
	type User struct {
	    Name          string    `json:"name"`
	    Password      string    `json:"-"`
	    PreferredFish []string  `json:"preferredFish,omitempty"`
	    CreatedAt     time.Time `json:"createdAt"`
	}
	```
	+ output:
	```
	{
	  "name": "Sammy the Shark",
	  "createdAt": "2019-09-23T16:08:21.124481-04:00"
	}
	```