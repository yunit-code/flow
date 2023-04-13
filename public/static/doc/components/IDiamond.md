# 菱形
组件描述
## 组件类ID（IDiamond）
idm.component.flow.idiamond
## 组件开发语言（comLangue）
vue
## 组件类型（comType）
common
## 所在代码包版本
flow@1.0.0
## 组件属性
### 唯一标识【ctrlId】

- 标识：`ctrlId`
- 默认值：`@[packageid]`
### 基本属性
#### 文本内容【fontContent】

- 标识：`fontContent`
- 默认值：`自定义文本`
#### 文本水平【textAlign】

- 标识：`textAlign`
- 默认值：`center`
#### 文本垂直【textVertical】

- 标识：`textVertical`
- 默认值：`center`
### 样式设置
#### 宽【width】
填写auto则为自适应，或者使用px、%、vw等单位，比如100%、100px、100vw等等
- 标识：`width`
- 默认值：`100%`
#### 高【height】
填写auto则为自适应，或者使用px、%、vh等单位，比如100%、100px、100vh等等
- 标识：`height`
- 默认值：`100%`
#### 内外边距【box】

- 标识：`box`
- 默认值：```json{
    "marginTopVal": "",
    "marginRightVal": "",
    "marginBottomVal": "",
    "marginLeftVal": "",
    "paddingTopVal": "",
    "paddingRightVal": "",
    "paddingBottomVal": "",
    "paddingLeftVal": ""
}```
#### 背景色【bgColor】

- 标识：`bgColor`
- 默认值：```json{}```
#### 边框色【borderColor】

- 标识：`borderColor`
- 默认值：```json{}```
#### 边框宽【borderWidth】
设置菱形边框宽度，单位：px
- 标识：`borderWidth`
- 默认值：`5`
#### 文字【font】

- 标识：`font`
- 默认值：```json{
    "fontColors": {
        "hex": "#000000",
        "hex8": "#000000FF"
    },
    "fontWeight": "400",
    "fontStyle": "normal",
    "fontSize": 16,
    "fontSizeUnit": "px",
    "fontTextAlign": "center",
    "fontDecoration": "",
    "fontLetterSpacing": 0,
    "fontLetterSpacingUnit": "px"
}```
### 主题设置【themeList】
点击？查看主题设置用法指南
- 标识：`themeList`
- 默认值：```json[
    {
        "key": "blue",
        "mainColor": {
            "hex": "#CFDDFA",
            "hex8": "#CFDDFAFF"
        },
        "minorColor": {
            "hex": "#1890FF",
            "hex8": "#1890FFFF"
        }
    },
    {
        "key": "red",
        "mainColor": {
            "hex": "#FCBFBF",
            "hex8": "#FCBFBFFF"
        },
        "minorColor": {
            "hex": "#FF4444",
            "hex8": "#FF4444FF"
        }
    },
    {
        "key": "green",
        "mainColor": {
            "hex": "#94F3C7",
            "hex8": "#94F3C7FF"
        },
        "minorColor": {
            "hex": "#0EAF64",
            "hex8": "#0EAF64FF"
        }
    }
]```
### 高级
#### 点击时动作【clickFunction】
点击的时候会调用设置的自定义函数，接收参数为格式为{...自定义的}
- 标识：`clickFunction`
- 默认值：空
- 可设置函数数量：多个
#### 动态内容【dataSourceType】
通过这些方式去动态获取结果值显示成文本内容
- 标识：`dataSourceType`
- 默认值：`attribute`
#### 结果集名【dataName】
页面接口设定的结果集名称，位置为：页面设置》高级设置》页面接口
- 标识：`dataName`
- 默认值：空
- 显示：`@[dataSourceType=='pageCommonInterface']`
#### 显示字段【dataFiled】
根据接口返回数据格式指定结果集的字段，比如结果集名为resultData（自定义接口忽略）且它的值为{data:{filedName:[{"text":"","value":"","check":true}]}}，则这里应该填写data.filedName
- 标识：`dataFiled`
- 默认值：空
- 显示：`@[dataSourceType!='customFunction']`
#### 自定义函数【customFunction】
获取动态文本内容、自定义接口回调、页面统一接口回调的时候会调用此方法，返回数据格式为字符串，接收参数：{...自定义,interfaceData:自定义接口或页面统一接口的返回结果,expressData:表达式替换后的结果}
- 标识：`customFunction`
- 默认值：空
- 可设置函数数量：单个