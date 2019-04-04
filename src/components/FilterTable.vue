<template>
    <div>
        <Table :border="border" :size="size" :height="height" :highlight-row="highlightRow" :no-data-text="noDataText" :no-filtered-data-text="noFilteredDataText" @on-current-change="onCurrentChange" @on-select="onSelect" @on-select-all="onSelectAll" @on-selection-change="onSelectionChange" :columns="columns" :data="data"></Table>
    </div>
</template>

<script>
    import Vue from 'vue'
    import Component from "vue-class-component";

    @Component({
        props: {
            columns: {type: Array, required: true},
            data: {type: Array, required: true},
            size: {type: String, default: 'default'},
            height: {type: Number, default: null},
            highlightRow: {type: Boolean, default: true},
            border: {type: Boolean, default: true},
            loading: {type: Boolean, default: false},
            noDataText: {type: String, default: '暂无数据'},
            noFilteredDataText: {type: String, default: '没有找到你要搜索的数据'},
        }
    })
    export default class FilterTable extends Vue {

        search = {};  //过滤条件保存的对象,就是保存Input框和Select中值

        created() {
            for (let index in this.columns) {
                let childColumn = {...this.columns[index]};
                delete childColumn.filter;
                let children = [];
                let renderHeader = (h) => {};
                // 如果存在过滤选项
                if (this.columns[index].filter) {
                    //过滤为下拉选择框
                    if (this.columns[index].filter.type && this.columns[index].filter.type === 'Select') {
                        renderHeader = (h) => {
                            return h(this.columns[index].filter.type, {
                                props: {
                                    value: null,   // Select选项列表一般 0 为全部
                                    placeholder: this.columns[index].filter.placeholder ? this.columns[index].filter.placeholder : "请选择",
                                    size: this.columns[index].filter.size ? this.columns[index].filter.size : 'default'
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px': null,
                                    margin: this.columns[index].filter.margin ? this.columns[index].filter.margin : '5px auto'
                                },
                                on: {
                                    'on-change': (val) => {
                                        if (val === 0) {
                                            // 当选项是全部的时候删除search中该字段的过滤条件
                                            this.$delete(this.search, this.columns[index].key);
                                            this.load();
                                            return;
                                        }
                                        //添加字段过滤条件
                                        this.$set(this.search, this.columns[index].key, val);
                                        this.load();
                                    }
                                }
                            }, this.createOptionsRender(index, h));
                        }
                    } else if (this.columns[index].filter.type && this.columns[index].filter.type === 'Input') {
                        // 如果是输入框
                        renderHeader = (h) => {
                            // 通过回车和点击搜索按钮才进行数据过滤查询,如果
                            // 要输入框变化就进行过滤,把 this.load()放到
                            // input事件方法就行了
                            let inputValue = {};
                            return h(this.columns[index].filter.type, {
                                props: {
                                    // icon: 'ios-search'
                                    type: this.columns[index].filter.stype ? this.columns[index].filter.stype : 'text',
                                    placeholder: this.columns[index].filter.placeholder ? this.columns[index].filter.placeholder : "",
                                    size: this.columns[index].filter.size ? this.columns[index].filter.size : 'default'
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px': null,
                                    margin: this.columns[index].filter.margin ? this.columns[index].filter.margin : '5px auto'
                                },
                                on: {
                                    input: val => {
                                        inputValue = val;
                                        if (!inputValue) {
                                            this.validInputValue(index, inputValue);
                                        }

                                    },
                                    // input框后面的搜索按钮的点击事件
                                    'on-click': () => {
                                        this.validInputValue(index, inputValue);
                                    },
                                    'on-enter': () => {
                                        this.validInputValue(index, inputValue);
                                    }
                                }
                            })
                        };
                    } else if (this.columns[index].filter.type && this.columns[index].filter.type === 'DatePicker') {
                        // 如果是时间框
                        renderHeader = (h) => {
                            let inputValue = {};
                            return h(this.columns[index].filter.type, {
                                props: {
                                    type: this.columns[index].filter.stype ? this.columns[index].filter.stype : "date",
                                    placement: "bottom-end",
                                    placeholder: this.columns[index].filter.placeholder ? this.columns[index].filter.placeholder : "查询日期",
                                    size: this.columns[index].filter.size ? this.columns[index].filter.size : 'default'
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px': null,
                                    margin: this.columns[index].filter.margin ? this.columns[index].filter.margin : '5px auto'
                                },
                                on: {
                                    'on-change': (val) => {
                                        if (!val) {
                                            this.$delete(this.search, this.columns[index].key);
                                        } else {
                                            this.$set(this.search, this.columns[index].key, val);
                                        }
                                        this.load();
                                    },
                                    // 'on-clear': () => {
                                    //     alert(3)
                                    //     this.$delete(this.search, this.columns[index].key);
                                    //     this.load();
                                    // },
                                }
                            })
                        };
                    }
                    this.$set(childColumn, 'renderHeader', renderHeader);
                    children.push(childColumn);
                    this.$set(this.columns[index], 'children', children);
                }
            }
        }

        createOptionsRender(index, h) {
            // 选项渲染
            let optionRender = [];
            if (this.columns[index].filter.option) {
                let option = this.columns[index].filter.option;
                for (let i in option) {
                    optionRender.push(h('Option', {
                        props: {
                            value: option[i].value
                        }
                    }, option[i].name))
                }
            }
            return optionRender;
        }

        // 重新加载数据
        load() {
            // 会执行一个load的事件
            this.$emit('on-search', this.search);
            console.log(this.search)
        }

        // 验证输入框的值
        validInputValue(index, inputValue) {
            if (!inputValue) {
                console.log(index, inputValue);
                console.log(this.search);
                this.$delete(this.search, this.columns[index].key);
                console.log(this.search);
                this.load();
                return;
            }
            this.$set(this.search, this.columns[index].key, inputValue);
            this.load();
        }

        onCurrentChange(currentRow, oldCurrentRow) {
            this.$emit('on-current-change', currentRow, oldCurrentRow)
        }

        onSelect(selection) {
            this.$emit('on-select', selection)
        }

        onSelectAll(selection) {
            this.$emit('on-select-all', selection)
        }

        onSelectionChange(selection) {
            this.$emit('on-selection-change', selection)
        }
    }
</script>

<style lang="stylus" scoped>
</style>
