
```
gin.Engine
├── RouterGroup (路由组，包含所有路由方法)
│   ├── GET/POST/PUT/DELETE 等路由方法
│   ├── Group() 路由分组
│   └── Use() 中间件
├── HTTP 服务功能
│   ├── Run() 启动服务器
│   └── RunTLS() HTTPS 服务
├── 中间件管理
│   └── Use() 添加全局中间件
└── 错误处理
    ├── NoRoute() 404 处理
    └── NoMethod() 405 处理
```
### 方法
**路由相关（继承自 RouterGroup）：**
```go
// HTTP 方法

GET(path string, handlers ...HandlerFunc) IRoutes

POST(path string, handlers ...HandlerFunc) IRoutes

PUT(path string, handlers ...HandlerFunc) IRoutes

DELETE(path string, handlers ...HandlerFunc) IRoutes

PATCH(path string, handlers ...HandlerFunc) IRoutes

HEAD(path string, handlers ...HandlerFunc) IRoutes

OPTIONS(path string, handlers ...HandlerFunc) IRoutes

ANY(path string, handlers ...HandlerFunc) IRoutes
```
**中间件和分组（继承自 RouterGroup）：**
```go
Use(middleware ...HandlerFunc) IRoutes

Group(component string, handlers ...HandlerFunc) *RouterGroup
```
**静态文件（继承自 RouterGroup）：**
```go
StaticFile(relativePath, filepath string) IRoutes

Static(relativePath, root string) IRoutes

StaticFS(relativePath string, fs http.FileSystem) IRoutes
```
**其他方法**
```go
Run(addr ...string) error

RunTLS(addr, certFile, keyFile string) error

RunUnix(file string) error

RunListener(listener net.Listener) error

  

NoRoute(handlers ...HandlerFunc)

NoMethod(handlers ...HandlerFunc)

  

LoadHTMLGlob(pattern string)

LoadHTMLFiles(files ...string)

SetHTMLTemplate(templ *template.Template)

SetFuncMap(funcMap template.FuncMap)

  

Routes() (routes RoutesInfo)

HandleMethodNotAllowed bool
```