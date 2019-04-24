---
typora-copy-images-to: ipic
---

参考与借鉴：<https://github.com/azhengyongqin/iview-filter-table>

# iview-filter-table

> 一个基于iView Table 的带搜索过滤的Table组件, 支持 Input输入框、Select下拉框、DatePicker时间框三种表格筛选方式.

## 使用

**模板**：

```vue
<div>
  <FilterTable :columns="columns" :data="data" @on-search="onSearch"></FilterTable>
</div>
```

**列描述数据对象：**

```js
ageOptions = [
    {
        value: '',
        name: '全部'
    },
    {
        value: 18,
        name: '18'
    },
    {
        value: 26,
        name: '26'
    },
    {
        value: 30,
        name: '30'
    },
];

columns = [
    {
        title: "姓名",
        key: "name",
        width: this.columnWidth,
        filter: {
            type: 'Input',
            stype: 'text',
            placeholder: '请输入姓名',
            size: this.tableSize,	// 和table的size尽量一致
            margin: '5px auto',
        }
    },
    {
        title: "日期",
        key: "date",
        width: this.columnWidth,
        filter: {
            type: 'DatePicker',
            stype: 'date',
            placeholder: '选择日期',
            size: this.tableSize,	// 和table的size尽量一致
            margin: '5px auto',
        }
    },
    {
        title: "地址",
        key: "address",
        width: this.columnWidth,
        filter: {
            type: 'Input',
            stype: 'text',
            placeholder: '请输入地址',
            size: this.tableSize,	// 和table的size尽量一致
            margin: '5px auto',
        }
    },
    {
        title: "家庭地址",
        key: "homeAddress",
        stype: 'text',
        width: this.columnWidth,
        filter: {
            type: 'Input',
            placeholder: '请输入地址',
            size: this.tableSize,	// 和table的size尽量一致
            margin: '5px auto',
        }
    },
    {
        title: "年龄",
        key: "age",
        width: this.columnWidth,
        filter: {
            type: 'Select',
            options: this.ageOptions,
            placeholder: '请选择',
            size: this.tableSize,	// 和table的size尽量一致
            margin: '5px auto',
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
ageOptions = [
    {
        value: '',
        name: '全部'
    },
    {
        value: 18,
        name: '18'
    },
    {
        value: 26,
        name: '26'
    },
    {
        value: 30,
        name: '30'
    },
];
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
