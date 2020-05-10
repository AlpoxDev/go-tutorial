# Go Lang

## 서론

새로운 것을 배우지 못하고 평소와 같이 JS를 하던 와중 새로운 언어를 배우는것에 관심이 생겼습니다. 그 와중 요즘 뜨는 언어인 `go`언어와 `Rust`언어를 지켜보고 있는 와중 구글이 만든 언어인 `go`언어가 내 취향에 더 맞다고 판단하여 한번 배워보도록 합니다.

## Hello, World!

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, 안녕")
}
```

- `package`
    모든 Go 프로그램은 패키지로 구성되어 있습니다. 프로그램은 main 패키지로부터 실행을 시작합니다.

- `import`
    package를 불러옵니다. 소괄호로 묶어서도, 다중으로도 가능합니다.

- `export`
    package를 import 하게되면 외부로 export 하게 된 것들을 이용할 수 있습니다. 메소드, 변수, 상수를 접근할 수 있습니다.  
    Go에서는 첫 문자가 대문자로 시작하게 되면 그 패키지를 사용하는 곳에서 접근할 수 있는 exported name이 됩니다. 예를 들어 Foo는 접근이 가능하지만, foo는 접근이 불가능합니다.

## 함수 : func

```go
package main

import "fmt"

func add(x int, y int) int {
    return x + y
}

func main() {
    fmt.Println(add(42, 13))
}

```

함수는 매개인자를 받을 수 있습니다. C/C++, Java와 달리 타입을 뒤에다 명시합니다. 왜 타입을 뒤에 명시하는지 알고 싶으면  
[http://golang.org/doc/articles/gos_declaration_syntax.html](http://golang.org/doc/articles/gos_declaration_syntax.html)을 참고하시면 됩니다.

```go
func add(x, y int) int {
    return x + y
}
```

두 개 이상의 매개변수가 타입이 같을 때는 이렇게 사용하셔도 됩니다.

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
    return y, x
}

func main() {
    a, b := swap("hello", "world")
    fmt.Println(a, b)
}
```

하나의 함수는 여러개의 결과값을 반환할 수 있습니다.

```go
package main

import "fmt"

func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}

func main() {
    fmt.Println(split(17))
}

```

반환 값에 이름을 붙이게 되면 반환 값을 지정하지 않은 `return`문장으로 결과의 현재 값을 알아서 반환하게 됩니다.

__함수 값__

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    hypot := func(x, y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }

    fmt.Println(hypot(3, 4))
}
```

함수도 값입니다.

__함수 클로져__

함수는 함수를 내포할 수 있으며, 반환할 수 있습니다.

## 변수 

__선언__

변수 선언을 위해서 var을 사용하게 됩니다. 함수의 매개변수처럼 타입은 뒤에 선언하게 됩니다.

```go
var x, y, z int
var c, python, java bool
```

__변수의 초기화__

변수는 선언과 함께 초기화를 할 수 있습니다.  

```go
var x, y, z int = 1, 2, 3
var c, python, java = true, false, "no!"
```

초기화를 하는 경우에는 타입을 생략할 수 있습니다.

__변수의 짧은 선언__

```go
var x, y, z int = 1, 2, 3
c, python, java := true, false, "no!"
```

`:=`을 사용하게 되면 `var`과 명시적인 타입(e.g int, bool)을 생략할 수 있습니다.

__상수__

```go
const Pi = 3.14
```

상수는 `const`키워드와 함께 변수처럼 선언됩니다.  
상수는 `문자`, `문자열`, `부울`, `숫자` 타입 중의 하나가 될 수 있습니다.

__숫자형 상수__

숫자형 상수는 정밀한 값을 가질 수 있습니다. 타입을 지정하지 않는 상수는 문맥에 따라 타입을 가지게 됩니다.

## 반복문 : for

```go
package main

import "fmt"

func main() {
    sum := 0
    for i := 0; i < 10; i++ {
        sum += i
    }
    fmt.Println(sum)
}

```

__for(2)__

```go
package main

import "fmt"

func main() {
    sum := 1
    for sum < 1000 {
        sum += sum
    }
    fmt.Println(sum)
}
```

조건문만 표시해서 다른 언어에서의 while처럼도 가능합니다.

__for - 무한루프__

```go
package main

func main() {
    for {
    }
}
```

for 에서 조건문을 생략하게 되면 무한 루프를 간단하게 표현할 수 있습니다.

## 조건문 : if

```go
package main

import (
    "fmt"
    "math"
)

func sqrt(x float64) string {
    if x < 0 {
        return sqrt(-x) + "i"
    }
    return fmt.Sprint(math.Sqrt(x))
}

func main() {
    fmt.Println(sqrt(2), sqrt(-4))
}

```

조건문의 if는 C나 Java와 동일합니다. 하지만, `()`를 사용하지 않고 `{}`만 이용합니다.

__if와 짧은 명령어를 같이 사용하기__

```go
package main

import (
    "fmt"
    "math"
)

func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    }
    return lim
}

func main() {
    fmt.Println(
        pow(3, 2, 10),
        pow(3, 3, 20),
    )
}

```

__if와 else문 사용하기__

다른 언어와 동일하게 사용가능합니다.


## 기본 자료형

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8의 다른 이름(alias)

rune // int32의 다른 이름(alias)
     // 유니코드 코드 포인트 값을 표현합니다. 

float32 float64

complex64 complex128
```

```go
package main

import (
    "fmt"
    "math/cmplx"
)

var (
    ToBe   bool       = false
    MaxInt uint64     = 1<<64 - 1
    z      complex128 = cmplx.Sqrt(-5 + 12i)
)

func main() {
    const f = "%T(%v)\n"
    fmt.Printf(f, ToBe, ToBe)
    fmt.Printf(f, MaxInt, MaxInt)
    fmt.Printf(f, z, z)
}

```

## 구조체 : struct

```go
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    fmt.Println(Vertex{1, 2})
}

```

type 선언으로 `struct`을 선언할 수 있습니다.

__구조체 필드__

구조체에 속한 필드(데이터)는 `.`으로 접근할 수 있습니다.

__포인터__

```go
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    p := Vertex{1, 2}
    q := &p
    q.X = 1e9
    fmt.Println(p)
}
```

포인터를 이용하는 간접적인 접근은 실제 구조체에서도 영향을 미칩니다.

__구조체 리터럴__

```go
package main

import "fmt"

type Vertex struct {
    X, Y int
}

var (
    p = Vertex{1, 2}  // has type Vertex
    q = &Vertex{1, 2} // has type *Vertex
    r = Vertex{X: 1}  // Y:0 is implicit
    s = Vertex{}      // X:0 and Y:0
)

func main() {
    fmt.Println(p, q, r, s)
}

```

구조체는 기초 선언이 int같은 경우 0으로 초기화 됩니다.  
원하는 필드를 `{Name: value}` 구문을 통해 할당할 수 있습니다.

__new 함수__

```go
package main

import "fmt"

type Vertex struct {
    X, Y int
}

func main() {
    v := new(Vertex)
    fmt.Println(v)
    v.X, v.Y = 11, 9
    fmt.Println(v)
}

```

new(T)는 모든 필드가 0(zero value)로 할당된 T타입의 포인터를 반환합니다.  

## 슬라이스 - 배열

```go
package main

import "fmt"

func main() {
    p := []int{2, 3, 5, 7, 11, 13}
    fmt.Println("p ==", p)

    for i := 0; i < len(p); i++ {
        fmt.Printf("p[%d] == %d\n",
            i, p[i])
    }
}

```

슬라이스는 배열의 값을 가리킵니다. 또한 배열의 길이를 가지고 있습니다.

__슬라이스 자르기__

```go
package main

import "fmt"

func main() {
    p := []int{2, 3, 5, 7, 11, 13}
    fmt.Println("p ==", p)
    fmt.Println("p[1:4] ==", p[1:4])

    // missing low index implies 0
    fmt.Println("p[:3] ==", p[:3])

    // missing high index implies len(s)
    fmt.Println("p[4:] ==", p[4:])
}

```

s[lo:hi]는 lo에서 hi-1까지의 요소를 포함하는 슬라이스입니다.

__슬라이스 만들기__

슬라이스는 `make`함수로 만들 수 있습니다.

```go
package main

import "fmt"

func main() {
    a := make([]int, 5)
    printSlice("a", a)
    b := make([]int, 0, 5)
    printSlice("b", b)
    c := b[:2]
    printSlice("c", c)
    d := c[2:5]
    printSlice("d", d)
}

func printSlice(s string, x []int) {
    fmt.Printf("%s len=%d cap=%d %v\n",
        s, len(x), cap(x), x)
}

```

이렇게 생성된 슬라이스는 0을 할당한 배열을 생성하고, 그것을 참조합니다. 또한 세번째 인자로 용량을 제한할 수 있습니다.

__빈 슬라이스__

슬라이스의 zero value는 nil입니다.  
nil 슬라이스의 길이와 최대크기는 0입니다.

__레인지(range)__

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    for i, v := range pow {
        fmt.Printf("2**%d = %d\n", i, v)
    }
}
```

for 반복문에서 range 를 사용하면 슬라이스나 맵을 순회(iterates)할 수 있습니다.

__레인지(2)__

```go
package main

import "fmt"

func main() {
    pow := make([]int, 10)
    for i := range pow {
        pow[i] = 1 << uint(i)
    }
    for _, value := range pow {
        fmt.Printf("%d\n", value)
    }
}
```

_를 이용해서 index를 무시할 수 있습니다.


## 맵 : Map

```go
package main

import "fmt"

type Vertex struct {
    Lat, Long float64
}

var m map[string]Vertex

func main() {
    m = make(map[string]Vertex)
    m["Bell Labs"] = Vertex{
        40.68433, -74.39967,
    }
    fmt.Println(m["Bell Labs"])
}
```

맵은 값에 키를 저장합니다.  
맵은 반드시 사용하기 전에 `make`를 명시해야 합니다.

__맵 리터럴__

```go
package main

import "fmt"

type Vertex struct {
    Lat, Long float64
}

var m = map[string]Vertex{
    "Bell Labs": Vertex{
        40.68433, -74.39967,
    },
    "Google": Vertex{
        37.42202, -122.08408,
    },
}

func main() {
    fmt.Println(m)
}
```


## 스위치 : switch

```go
package main

import (
    "fmt"
    "runtime"
)

func main() {
    fmt.Print("Go runs on ")
    switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS X.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plan9, windows...
        fmt.Printf("%s.", os)
    }
}
```

다른 언어와 달리 `break`를 알아서 한다는 점 입니다.  
또한 스위치는 위에서 아래로 평가합니다.

## 메소드

Go lang에는 클래스가 없습니다. 하지만 메소드를 struct에 붙일 수 있습니다.

```go
package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    X, Y float64
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func main() 
    v := &Vertex{3, 4}
    fmt.Println(v.Abs())
}

```