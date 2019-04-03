---
typora-copy-images-to: ipic
---

# iview-filter-table
> 一个基于iView Table 的带搜索过滤的Table组件, 支持 Input输入框、Select下拉框、DatePicker时间框三种表格筛选方式.

## 使用

**模板**：

```vue
<filter-table @on-search="onSearch"
              :data="users"
              :columns="tableColumns">
</filter-table>
```

**列描述数据对象：**

```js
ageOptions = {
  0: {
    value: 0,
    name: '全部'
  },
  1: {
    value: 18,
    name: '18'
  },
  2: {
    value: 26,
    name: '26'
  },
  3: {
    value: 30,
    name: '30'
  },
};

columns = [
  {
    title: "姓名",
    key: "name",
    width: 400,
    filter: {
      type: 'Input'
    }
  },
  {
    title: "日期",
    key: "date",
    width: 400,
    filter: {
      type: 'DatePicker'
    }
  },
  {
    title: "地址",
    key: "address",
    width: 400,
    filter: {
      type: 'Input'
    }
  },
  {
    title: "年龄",
    key: "age",
    width: 400,
    filter: {
      type: 'Select',
      option: this.ageOptions
    }
  },
  {
    title: "功能",
    key: "action",
    width: 150,
    align: 'center',
    fixed: "right",
    render: (h, params) => {
      return h("div", [
        h("Button", {
          props: {
            type: "primary",
            size: "small"
          },
          style: {
            marginRight: "5px"
          },
          on: {
            click: () => {
              this.showIndex(params.index)
            }
          }
        }, "View"),
        h("Button", {
          props: {
            type: "error",
            size: "small"
          },
          on: {
            click: () => {
              this.removeIndex(params.index)
            }
          }
        }, "Delete")
      ]);
    }
  }
];

```

**下拉框数据：**

```js
ageOptions = {
  0: {
    value: 0,
    name: '全部'
  },
  1: {
    value: 18,
    name: '18'
  },
  2: {
    value: 26,
    name: '26'
  },
  3: {
    value: 30,
    name: '30'
  },
};
```

**触发搜索事件：**

```js
        onSearch(search) {
            let data = [];
            for (let item of this.data1) {
                if (this.screen(Object.values(item), Object.values(search))) {
                    data.push(item)
                }
            }
            this.data = data;
        }
```

在该方法中进行条件过滤，更新`data` 属性的值。

直接运行该项目可以看当前组件的Table效果。

![image-20190403103056324](https://ws3.sinaimg.cn/large/006tKfTcly1g1p8yrk8ywj31lo0u0467.jpg)

![image-20190403103126114](https://ws1.sinaimg.cn/large/006tKfTcly1g1p8yx3385j32dk0f0mza.jpg)



## Project setup

```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Run your unit tests
```
npm run test:unit
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

**Github源码地址** <https://github.com/xinlei3166/iview-filter-table>
