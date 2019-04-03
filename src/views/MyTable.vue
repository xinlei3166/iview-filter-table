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

        data = [
            {
                name: "John Brown",
                age: 18,
                address: "New York No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Jim Green",
                age: "241111111111112121212212113123123",
                address: "London No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Joe Black",
                age: 30,
                address: "Sydney No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }
        ];

        data1 = [
            {
                name: "John Brown",
                age: 18,
                address: "New York No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Jim Green",
                age: "241111111111112121212212113123123",
                address: "London No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Joe Black",
                age: 30,
                address: "Sydney No. 1 Lake Park",
                date: '2019-04-02'
            },
            {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
                date: '2019-04-02'
            }, {
                name: "Jon Snow",
                age: 26,
                address: "Ottawa No. 2 Lake Park",
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
            console.log(item);
            console.log(search);
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

