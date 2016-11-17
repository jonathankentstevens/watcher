# watcher
Simple file or directory watcher

# implementation
    
    go get github.com/jonathankentstevens/gostruct
    
# usage 

```go
package main

import (
	"log"
	"github.com/jonathankentstevens/watcher"
)

func main() {
	w := watcher.NewWatcher()
	defer w.Close()

	done := make(chan bool)
	go func() {
		for {
			select {
			case event := <-w.Events:
				log.Println("File changed", event)
			case err := <-w.Errors:
				log.Println("Error", err)
			}
		}
	}()

	err := w.Add("/path/file1.txt")
	if err != nil {
		log.Fatalln(err)
	}
	err = w.Add("/path/file2.txt")
	if err != nil {
		log.Fatalln(err)
	}
	<-done
}
```

# output

```
2016/11/16 21:00:08 File changed {/path/file1.txt MODIFIED}
2016/11/16 21:00:12 File changed {/path/file1.txt MODIFIED}
```