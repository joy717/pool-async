[![Go Report Card](https://goreportcard.com/badge/github.com/joy717/poolasync)](https://goreportcard.com/report/github.com/joy717/poolasync)

# pool-async
pool-async is a tool for goroutines with a pool.

# Features
* limit goroutine numbers for avoid to use out of resources like memory, fd etc.
* can return first error when using `DoWithError()` then `Wait()` will return the first non-nil error
* you can get all errors if you want. just call `GetErrors()`

# Quick start
```
package main

import "github.com/joy717/poolasync"
import "fmt"

func main() {
  pa := poolasync.NewDefaultPoolAsync()
  pa.DoWitError(func() error {
    fmt.Println("goroutine 1")
    return nil
  }).DoWitError(func() error {
    fmt.Println("goroutine 2")
    return fmt.Errorf("goroutine 2 err")
  })
  
  // NOTICE: should call pa.Wait() always to complete the jobs.
  // because we use channel, Wait() is the reciver function.
  if err := pa.Wait(); err != nil {
    fmt.Println("the 1st non-nil err: ", err)
  }
}
```
If you like this tool, star it and share it. Thx.

# Note
You can choose Golang offical repo errgroup.Group too. this repo add limit since 2022 now.
https://cs.opensource.google/go/x/sync/+/0976fa681c295de5355f7a4d968b56cb9da8a76b

`Poolasync` can get all errors, the `errgroup` can only get the first error.
