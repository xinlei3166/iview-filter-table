<template>
    <div>
        <FilterTable :columns="columns" :data="data" @on-search="onSearch"></FilterTable>
    </div>
</template>

<script>
    import Vue from 'vue'
    import Component from "vue-class-component";
    import FilterTable from "@/components/FilterTable.vue"

    @Component({
        components: { FilterTable }
    })
    export default class MyTable extends Vue {
        tableSize = 'default';
        columnWidth = 300;

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

        data = [
            {
                name: "John Brown",
                age: 18,
                address: "New York No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-05'
            },
            {
                name: "Jim Green",
                age: "241111111111112121212212113123123",
                address: "London No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-04'
            },
            {
                name: "Joe Black",
                age: 30,
                address: "Sydney No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-03'
            },
            {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }
        ];

        data1 = [
            {
                name: "John Brown",
                age: 18,
                address: "New York No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-05'
            },
            {
                name: "Jim Green",
                age: "241111111111112121212212113123123",
                address: "London No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-04'
            },
            {
                name: "Joe Black",
                age: 30,
                address: "Sydney No. 1 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-03'
            },
            {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                homeAddress: "New York No. 1 Lake Park",
                date: '2019-04-02'
            }
        ];

        showTime(value) {
            alert(value)
        }

        showIndex(index) {
            this.$Modal.info({
                title: "User Info",
                content: `Name：${this.data[index].name}<br>Age：${this.data[index].age}<br>Address：${this.data[index].address}`
            })
        }

        removeIndex(index) {
            alert(index);
        }

        screen(item, search) {
            for (let s in search) {
                if (!item.includes(search[s])) {
                    return
                }
            }
            return true;
        }

        onSearch(search) {
            let data = [];
            for (let item of this.data1) {
                if (this.screen(Object.values(item), Object.values(search))) {
                    data.push(item)
                }
            }
            this.data = data;
        }
    }
</script>

<style lang="stylus" scoped>
</style>

