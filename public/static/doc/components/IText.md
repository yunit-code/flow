# 文本
组件描述
## 组件类ID（IText）
idm.component.flow.text
## 组件开发语言（comLangue）
vue
## 组件类型（comType）
common
## 所在代码包版本
oacommon@1.0.0
## 组件属性
### 唯一标识【ctrlId】

- 标识：`ctrlId`
- 默认值：@[packageid]
### 基本属性
#### 文本内容【fontContent】

- 标识：`fontContent`
- 默认值：文本内容
#### title【htmlTitle】
将内容显示为html标签的title，也就是鼠标放上去会有显示文本内容的文字
- 标识：`htmlTitle`
- 默认值：空
#### 默认状态【defaultStatus】

- 标识：`defaultStatus`
- 默认值：default
- 选项	 - 普通【default】	 - 隐藏【hidden】
### 样式设置
#### 内外边距【box】

- 标识：`box`
- 默认值：空
#### 宽高【undefined】

- 标识：`undefined`
- 默认值：空
#### 背景设置
##### 背景色【bgColor】

- 标识：`bgColor`
- 默认值：[object Object]
##### 背景图片【bgImgUrl】

- 标识：`bgImgUrl`
- 默认值：空
##### 横向偏移【positionX】

- 标识：`positionX`
- 默认值：空
- 显示：@[bgImgUrl]
##### 纵向偏移【positionY】

- 标识：`positionY`
- 默认值：空
- 显示：@[bgImgUrl]
##### 背景大小【bgSize】

- 标识：`bgSize`
- 默认值：空
- 显示：@[bgImgUrl]
- 选项	 - 裁切显示【cover】	 - 完全显示【contain】	 - 自定义【custom】
##### 宽度【bgSizeWidth】

- 标识：`bgSizeWidth`
- 默认值：空
- 显示：@[bgSize=='custom']
##### 高度【bgSizeHeight】

- 标识：`bgSizeHeight`
- 默认值：空
- 显示：@[bgSize=='custom']
##### 平铺模式【bgRepeat】

- 标识：`bgRepeat`
- 默认值：空
- 显示：@[bgImgUrl]
- 选项	 - 双向重复【repeat】	 - 水平重复【repeat-x】	 - 垂直重复【repeat-y】	 - 不重复【no-repeat】	 - 继承【inherit】
##### 背景模式【bgAttachment】

- 标识：`bgAttachment`
- 默认值：空
- 显示：@[bgImgUrl]
- 选项	 - 固定【fixed】	 - 滚动【scroll】	 - 继承【inherit】
#### 边框【border】

- 标识：`border`
- 默认值：空
#### 文字【font】

- 标识：`font`
- 默认值：空
### 高级
#### 点击时动作【clickFunction】
点击的时候会调用设置的自定义函数，接收参数为格式为{...自定义的}
- 标识：`clickFunction`
- 默认值：空
- 可设置函数数量：多个
#### 动态内容【dataSourceType】
通过这些方式去动态获取结果值显示成文本内容

- 标识：`dataSourceType`
- 默认值：customInterface
#### 接口地址【customInterfaceUrl】
用于获取数据源的接口地址，请按照统一标准的返回格式返回文本数据，格式如下：{"code":"200","data":"",...}

- 标识：`customInterfaceUrl`
- 默认值：空
- 显示：@[dataSourceType=='customInterface']
#### 结果集名【dataName】
页面接口设定的结果集名称，位置为：页面设置》高级设置》页面接口

- 标识：`dataName`
- 默认值：空
- 显示：@[dataSourceType=='pageCommonInterface']
#### 显示字段【dataFiled】
根据接口返回数据格式指定结果集的字段，比如结果集名为resultData（自定义接口忽略）且它的值为{data:{filedName:[{"text":"","value":"","check":true}]}}，则这里应该填写data.filedName
- 标识：`dataFiled`
- 默认值：空
- 显示：@[dataSourceType!='customFunction']
#### 自定义函数【customFunction】
获取动态文本内容、自定义接口回调、页面统一接口回调的时候会调用此方法，返回数据格式为字符串，接收参数：{...自定义,interfaceData:自定义接口或页面统一接口的返回结果,expressData:表达式替换后的结果}
- 标识：`customFunction`
- 默认值：空
- 可设置函数数量：单个