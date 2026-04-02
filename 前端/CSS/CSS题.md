# loading动画
##### 三点跳动
```html
<style>
         @keyframes bounce {
            0%,100% {
                transform: scale(0);
            }
            50% {
                transform: scale(1);
            }
        }
        .contain{
            display: flex;
        }
       .dot {
            width: 10px;
            height: 10px;
            background: #000;
            border-radius: 50%;
            animation: bounce 1.4s infinite;
        }
        .dot:nth-child(2){
            animation-delay: 0.2s;
        }
        .dot:nth-child(3){
            animation-delay: 0.4s;
        }
</style>
<body>
    <div class="contain">
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
    </div>
</body>
```
##### 旋转圆环
```html
    <style>
        .loading {
            width: 40px;
            height: 40px;
            border: 4px solid #ccc;
            border-top-color: black;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }
    </style>
<body>
    <div class="loading"></div>
</body>
```