**HTTP 方法路由**
```go
// 所有 HTTP 方法

GET(path string, handlers ...HandlerFunc) IRoutes

POST(path string, handlers ...HandlerFunc) IRoutes

PUT(path string, handlers ...HandlerFunc) IRoutes

DELETE(path string, handlers ...HandlerFunc) IRoutes

PATCH(path string, handlers ...HandlerFunc) IRoutes

HEAD(path string, handlers ...HandlerFunc) IRoutes

OPTIONS(path string, handlers ...HandlerFunc) IRoutes

ANY(path string, handlers ...HandlerFunc) IRoutes

  

// 自定义方法

Handle(httpMethod, relativePath string, handlers ...HandlerFunc) IRoutes
```
**中间件管理**
```go
// 添加中间件
Use(middleware ...HandlerFunc) IRoutes
```
**分组创建**
```go
// 创建子分组
Group(component string, handlers ...HandlerFunc) *RouterGroup
```
 **静态文件服务**
```go
// 静态文件
StaticFile(relativePath, filepath string) IRoutes
Static(relativePath, root string) IRoutes
StaticFS(relativePath string, fs http.FileSystem) IRoutes
```
## 路径组合规则
```go
// 基础路径组合规则
r := gin.Default()
api := r.Group("/api")           // basePath: "/api"
v1 := api.Group("/v1")           // basePath: "/api/v1"  
users := v1.Group("/users")      // basePath: "/api/v1/users"

// 最终路由：
// users.GET("/profile") → 实际路径: /api/v1/users/profile
// users.POST("/create") → 实际路径: /api/v1/users/create
```
### **基础用法**
```go
func main() {
    r := gin.Default()
    
    // Engine 本身就是一个 RouterGroup
    r.GET("/", handler)  // r 是 *RouterGroup
    
    // 创建分组
    api := r.Group("/api")  // 返回 *RouterGroup
    api.GET("/users", handler)
    api.POST("/users", handler)
}
```
### **分组嵌套**
```go
func main() {
    r := gin.Default()
    
    // 一级分组
    api := r.Group("/api")
    {
        api.GET("/users", handler1)
        api.POST("/users", handler2)
        
        // 二级分组
        v1 := api.Group("/v1")
        {
            v1.GET("/users", handler3)
            v1.POST("/users", handler4)
        }
        
        v2 := api.Group("/v2")
        {
            v2.GET("/users", handler5)
        }
    }
}
```
### **中间件分组**
```go
func main() {
    r := gin.Default()
    
    // 公共路由组（无中间件）
    public := r.Group("/public")
    {
        public.GET("/info", handler1)
    }
    
    // 受保护路由组（带认证中间件）
    protected := r.Group("/api")
    protected.Use(AuthMiddleware())  // 该组所有路由都使用认证中间件
    {
        protected.GET("/profile", handler2)
        protected.POST("/posts", handler3)
        
        // 在分组内再创建子分组
        admin := protected.Group("/admin")
        admin.Use(AdminMiddleware())  // 管理员中间件
        {
            admin.GET("/dashboard", handler4)
            admin.DELETE("/users/:id", handler5)
        }
    }
}
```
### **完整示例**
```go
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

func AuthMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        // 简单认证逻辑
        token := c.GetHeader("Authorization")
        if token == "" {
            c.JSON(401, gin.H{"error": "Unauthorized"})
            c.Abort()
            return
        }
        c.Next()
    }
}

func AdminMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        // 简单管理员验证
        role := c.GetHeader("Role")
        if role != "admin" {
            c.JSON(403, gin.H{"error": "Forbidden"})
            c.Abort()
            return
        }
        c.Next()
    }
}

func main() {
    r := gin.Default()
    
    // 根分组（Engine 本身）
    r.GET("/", func(c *gin.Context) {
        c.JSON(200, gin.H{"message": "Root path"})
    })
    
    // API 分组
    api := r.Group("/api")
    {
        api.GET("/health", func(c *gin.Context) {
            c.JSON(200, gin.H{"status": "ok"})
        })
        
        // 用户相关接口
        users := api.Group("/users")
        users.Use(AuthMiddleware())  // 用户接口需要认证
        {
            users.GET("/", func(c *gin.Context) {
                c.JSON(200, gin.H{"users": []string{"user1", "user2"}})
            })
            users.GET("/:id", func(c *gin.Context) {
                id := c.Param("id")
                c.JSON(200, gin.H{"id": id, "name": "John"})
            })
            users.POST("/", func(c *gin.Context) {
                c.JSON(201, gin.H{"message": "User created"})
            })
        }
        
        // 管理员接口
        admin := api.Group("/admin")
        admin.Use(AuthMiddleware(), AdminMiddleware())  // 需要认证+管理员权限
        {
            admin.GET("/dashboard", func(c *gin.Context) {
                c.JSON(200, gin.H{"message": "Admin dashboard"})
            })
            admin.GET("/users", func(c *gin.Context) {
                c.JSON(200, gin.H{"message": "Admin users list"})
            })
        }
    }
    
    // 静态文件
    static := r.Group("/static")
    static.Static("/files", "./files")  // 静态文件服务
    
    r.Run()
}
```