### Bad Guys 能看到明天的太阳吗？

#### 1. time.After()
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan int, 0)
	go func() {
		defer close(ch)
		for i := 0; i < 60*1000*1000; i++ {
			ch <- i
		}
	}()
	sum := 0
LOOP:
	for {
		select {
		case i, ok := <-ch:
			if !ok {
				break LOOP
			}
			sum += i
		case <-time.After(time.Minute):
			break LOOP
		}
	}
	fmt.Println(sum)
	fmt.Println("明天的太阳")
}

```

#### 2. sick map
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	//runtime.GOMAXPROCS(1)
	m := make(map[int]int)
	wg := sync.WaitGroup{}
	for i := 0; i < 100; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			m[i] = i
		}()
	}
	wg.Wait()
	fmt.Println("明天的太阳")
}

```

#### 3. busy array
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	//runtime.GOMAXPROCS(1)
	arr := make([]int, 0)
	wg := sync.WaitGroup{}
	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			arr = append(arr, i)
		}()
	}
	wg.Wait()
	if len(arr) == 1000 {
		fmt.Println("明天的太阳")
	}
}

```

#### 4. sad sum
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	//runtime.GOMAXPROCS(1)
	sum := 0
	wg := sync.WaitGroup{}
	for i := 0; i < 100; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			sum++
		}()
	}
	wg.Wait()
	if sum == 100 {
		fmt.Println("明天的太阳")
	}
}

```