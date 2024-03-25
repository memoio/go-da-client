# go-da-client

go-da-client defines specific Data Availablility interface in MemoDA for modular blockchains.

## MemoDA Interface

| Method        | Params                                    | Return            |
| ------------- | ----------------------------------------- | ----------------- |
| `Start`       |                                           |                   |
| `Get`         | `ids [][]byte`                            | `data [][]byte`   |
| `Submit`      | `data [][]byte, options ...interface{}`   | `ids [][]byte`    |

## Example

```golang
package main

import (
    "context"
    "flag"
    "fmt"

    memo "github.com/memoio/go-da-client"
    "github.com/ethereum/go-ethereum/common"
)

var (
    defaultData = "0x095ea7b30000000000000000000000000776b0a89b77163a33bb5f062f3a781f920adcf100000000000000000000000000000000000000000000000000005af3107a4000"
    rpcaddr     = flag.String("http", "localhost:12345", "MemoDA rpc")
    testdata    = flag.String("data", defaultData, "test data for submission and retrieval")
)

func main() {
    flag.Parse()
    dac := memo.NewMemoDAClient(*rpcaddr)
    err := dac.Start()
    if err != nil {
        fmt.Println(err)
        return
    }

    data := [][]byte{common.FromHex(*testdata)}
    ids, err := dac.Submit(context.Background(), data)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(ids[0])

    gotData, err := dac.Get(context.Background(), ids)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(gotData[0])
}
```
