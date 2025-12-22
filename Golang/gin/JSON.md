## **JSON 响应 - c.JSON()**
 **1. 基本用法**
 ```go
 r.GET("/users", func(c *gin.Context) {
    // 使用 gin.H（map[string]interface{} 的便捷写法）
    c.JSON(200, gin.H{
        "message": "success",
        "data": []gin.H{
            {"id": 1, "name": "Alice"},
            {"id": 2, "name": "Bob"},
        },
        "total": 2,
    })
})

// 返回: {"message":"success","data":[{"id":1,"name":"Alice"},{"id":2,"name":"Bob"}],"total":2}
```
**2. 返回结构体**
```go
type User struct {
    ID   int    `json:"id"`
    Name string `json:"name"`
    Age  int    `json:"age"`
}

r.GET("/user/:id", func(c *gin.Context) {
    user := User{
        ID:   123,
        Name: "John",
        Age:  25,
    }
    c.JSON(200, user)  // 直接返回结构体
})

// 返回: {"id":123,"name":"John","age":25}
```
**3. 返回切片**
```go
r.GET("/users", func(c *gin.Context) {
    users := []User{
        {ID: 1, Name: "Alice", Age: 23},
        {ID: 2, Name: "Bob", Age: 28},
    }
    c.JSON(200, users)
})

// 返回: [{"id":1,"name":"Alice","age":23},{"id":2,"name":"Bob","age":28}]
```
## **JSON 绑定 - 从请求体解析**
 **1. c.ShouldBindJSON()**
```go
 type CreateUserRequest struct {
    Name  string `json:"name" binding:"required"`
    Email string `json:"email" binding:"required,email"`
    Age   int    `json:"age" binding:"required,min=1,max=120"`
}

r.POST("/users", func(c *gin.Context) {
    var req CreateUserRequest
    
    // 从请求体解析 JSON 到结构体
    if err := c.ShouldBindJSON(&req); err != nil {
        c.JSON(400, gin.H{
            "error": "Validation failed",
            "detail": err.Error(),
        })
        return
    }
    
    // 处理业务逻辑
    c.JSON(201, gin.H{
        "message": "User created successfully",
        "user": req,
    })
})
```
## **结构体标签**
**1. json 标签**
```go
type User struct {
    ID    int    `json:"id"`           // JSON 字段名
    Name  string `json:"user_name"`    // JSON 中字段名为 "user_name"
    Email string `json:"email"`        // JSON 中字段名为 "email"
    Age   int    `json:"-"`            // 忽略该字段（不会出现在 JSON 中）
}
```
**2. binding 标签（验证规则）**
```go
type User struct {
    Name string `json:"name" binding:"required"`  // 必需
    
    Email string `json:"email" binding:"required,email"` //必需且是邮箱
    
    Age int `json:"age" binding:"required,min=1,max=120"`//必需且在1-120之间
    
    Password string `json:"password" binding:"required,min=6"`//必需且至少6位
    
    Phone string `json:"phone" binding:"omitempty,e164"`// 可选，但如果有必须是E164 格式
    
    Status string `json:"status" binding:"oneof=active inactive"`//必须是指定值之一
    
    CreatedAt string `json:"created_at" binding:"required,datetime=2006-01-02"` // 日期格式
}
```
## **其他绑定方法**
**1. c.ShouldBind() - 自动检测内容类型**
```go
r.POST("/users", func(c *gin.Context) {
    var req CreateUserRequest
    
    // 自动检测 Content-Type，支持 JSON、XML、YAML、Form
    if err := c.ShouldBind(&req); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    
    c.JSON(201, req)
})
```
**2. c.ShouldBindWith() - 指定绑定方式**
```go
r.POST("/users", func(c *gin.Context) {
    var req CreateUserRequest
    
    // 明确指定使用 JSON 绑定
    if err := c.ShouldBindWith(&req, binding.JSON); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    
    c.JSON(201, req)
})
```
**3. c.MustBindWith() - 必须绑定（失败返回 400）**
```go
r.POST("/users", func(c *gin.Context) {
    var req CreateUserRequest
    
    // 必须绑定成功，失败时自动返回 400 错误
    if err := c.MustBindWith(&req, binding.JSON); err != nil {
        // 这里 err 一定是验证错误
        return  // 不需要手动处理错误
    }
    
    c.JSON(201, req)
})
```
#### *JSON 绑定完整示例*
```go
type User struct {
    ID      uint `json:"id" binding:"omitempty"` //创建时可选，更新时必需
    Name     string    `json:"name" binding:"required,min=2,max=32"`
    Email    string    `json:"email" binding:"required,email"`
    Age      int       `json:"age" binding:"required,min=1,max=120"`
    Password string    `json:"password" binding:"required,min=6"`
    Status   string    `json:"status" binding:"oneof=active inactive"`
    CreatedAt time.Time `json:"created_at" binding:"omitempty"`
}

func main() {
    r := gin.Default()
    
    // 创建用户
    r.POST("/users", func(c *gin.Context) {
        var user User
        
        if err := c.ShouldBindJSON(&user); err != nil {
            c.JSON(400, gin.H{
                "error": "Validation failed",
                "details": err.Error(),
            })
            return
        }
        
        // 模拟创建用户（实际项目中会保存到数据库）
        user.ID = 123  // 假设这是新创建的 ID
        user.CreatedAt = time.Now()
        
        c.JSON(201, gin.H{
            "message": "User created successfully",
            "user":    user,
        })
    })
    
    // 批量创建用户
    r.POST("/users/batch", func(c *gin.Context) {
        var users []User
        
        if err := c.ShouldBindJSON(&users); err != nil {
            c.JSON(400, gin.H{"error": err.Error()})
            return
        }
        
        c.JSON(201, gin.H{
            "message": "Users created successfully",
            "count":   len(users),
            "users":   users,
        })
    })
    
    r.Run()
}
```