# ux-icons

### 图标预览
[ux-icons图标预览](https://hexinkuo.gitee.io/uni-x/iconfont/demo_index.html)

### 使用方式

将ux-icons组件拉下来到uni_modules

```
<ux-icons name="search" :size="14" color="#007aff"></ux-icons>
```

### 参数说明

|参数名     |类型        | 默认值        |备注       |
|:---:      |:---:       |:---:        |:---:       |
|name       | string     |             | 图标名     |
|size       | number     | 16          | 图标大小   |
|color      | string     | default     | 图标颜色   |

### 扩展自定义图标  

> ux-icons支持扩展自定义图标，使用如下方法：  

1. 将自己的图标以svg形式上传到iconfont下的对应项目  
2. 切换Unicode模式下的图标，点击右侧下载到本地  
3. 解压后，找到目录中的iconfont.json和iconfont.ttf  
4. 打开iconfont.json，找到glyphs数组，将整个数组复制到组件下的logic目录中的config.uts中  
5. 找到数组中每一项的unicode属性，将值改为\u开头，建议使用搜索替换  
6. 将找到的iconfont.ttf更名为ux.ttf，同时，复制到组件的fonts目录下   
7. 如果有必要，请将字体文件上传服务器，使用远程方式，并更新ux-icons.uvue文件中的@font-face  