```go
import (
    "bufio"
    "fmt"
    "os"
    "io"
    "bytes" //快读字符串才需要
)
var reader = bufio.NewReader(os.Stdin)   //包级变量
var writer = bufio.NewWriter(os.Stdout) //包级变量
defer writer.Flush() //一定要写在main()开头
func readI() int64 { //快读int64
    var x int64
    var neg bool = false
    var c byte
    var err error
    for c,err= reader.ReadByte(); c < '0' || c > '9'; c, err= reader.ReadByte() {
        if c == '-' {
            neg = true
        }
        if err== io.EOF {
            return 0
        }
    }
    for c >= '0' && c <= '9' {
        x = x*10 + int64(c-'0')
        c, _ = reader.ReadByte()
    }
    if neg {
        return -x
    }
    return x
}
func puts(x...any) { //快写
    for _,val:=range x{
        fmt.Fprint(writer,val)
    }
}
func readS() string { //快读字符串
    var strBuffer bytes.Buffer
    var c byte
    var err error
    for {
        c, err = reader.ReadByte()
        if err != nil {
            if err == io.EOF {
                return ""
            }
            return ""
        }
        if c == ' ' || c == '\n' || c == '\r' || c == '\t' {
            continue
        }
        break
    }
    strBuffer.WriteByte(c)
    for {
        c, err = reader.ReadByte()
        if err != nil {
            if err == io.EOF {
                break
            }
        }
        if c == ' ' || c == '\n' || c == '\r' || c == '\t' {
            reader.UnreadByte()
            break
        }
        strBuffer.WriteByte(c)
    }
    return strBuffer.String()
}
```