# Ant Design

UI效果的组件库

ant-design PC端

antd-mobile 移动端

进入组件搜索组件


## 安装和配置

```
npm install antd --save

# yarn add antd
```

在package.json加入：

```
"antd": "^5.6.2",
```

引入组件和样式

```typescript
import {Button} from 'antd'; //需要查询组件库支持的属性，例如onClick等
import 'antd/dist/antd.css'; //需要加入对应的样式，可以新建一个.css文件夹以管理样式
```


## 使用AntD （例子）

### 加入带格式的按钮

```typescript
import { Button } from 'antd';
import React from 'react';
import 'antd/dist/antd.css';

const App: React.FC = () => (
  <div className="App">
    <Button type="primary">Button</Button>
  </div>
);

export default App;
```

### 栅格系统

### layout系统
