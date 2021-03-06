# grsync — golang rsync wrapper

[![GoDoc](https://godoc.org/github.com/charoit/grsync?status.svg)](https://godoc.org/github.com/zloylos/grsync)

Repository contains some helpful tools:
- raw rsync wrapper
- rsync task — wrapper which provide important information about rsync task: progress, remain items, total items and speed

## Task wrapper usage

```golang
package main

import (
    "fmt"
    "grsync"
    "time"
)

func main() {
    task := grsync.NewTask(
        "username@server.com:/source/folder",
        "/home/user/destination",
        grsync.RsyncOptions{},
    )

    go func() {
        for {
            state := task.State()
            fmt.Printf(
                "progress: %.2f / rem. %d / tot. %d / sp. %s \n",
                state.Progress,
                state.Remain,
                state.Total,
                state.Speed,
            )
            time.Sleep(time.Second)
        }
    }()

    if err := task.Run(); err != nil {
        panic(err)
    }

    fmt.Println("well done")
    fmt.Println(task.Log())
}
```
