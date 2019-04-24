<template>
    <div>
        <Table :loading="loading" :border="border" :size="size" :height="height" :highlight-row="highlightRow" :no-data-text="noDataText"
               :no-filtered-data-text="noFilteredDataText" @on-current-change="onCurrentChange"
               @on-row-dblclick="onRowDbclick" @on-select="onSelect" @on-select-all="onSelectAll"
               @on-selection-change="onSelectionChange" :columns="columns" :data="data"></Table>
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
            for (let col of this.columns) {
                let childColumn = {...col};
                delete childColumn.filter;
                let children = [];
                let renderHeader = (h) => {
                };
                // 如果存在过滤选项
                if (col.filter) {
                    //过滤为下拉选择框
                    if (col.filter.type && col.filter.type === 'Select') {
                        renderHeader = (h) => {
                            return h(col.filter.type, {
                                props: {
                                    value: null,   // Select选项列表一般 '' 或 null 为全部
                                    placeholder: col.filter.placeholder ? col.filter.placeholder : "请选择",
                                    size: col.filter.size ? col.filter.size : 'default'
                                },
                                style: {
                                    width: col.width ? col.width - 40 + 'px' : null,
                                    margin: col.filter.margin ? col.filter.margin : '5px auto'
                                },
                                on: {
                                    'on-change': (val) => {
                                        if (val === '' || val === null) {
                                            // 当选项是全部的时候删除search中该字段的过滤条件
                                            this.$delete(this.search, col.key);
                                            this.load();
                                            return;
                                        }
                                        //添加字段过滤条件
                                        this.$set(this.search, col.key, val);
                                        this.load();
                                    }
                                }
                            }, this.createOptionsRender(col, h));
                        }
                    } else if (col.filter.type && col.filter.type === 'Input') {
                        // 如果是输入框
                        renderHeader = (h) => {
                            // 通过回车和点击搜索按钮才进行数据过滤查询,如果
                            // 要输入框变化就进行过滤,把 this.load()放到
                            // input事件方法就行了
                            let inputValue = {};
                            return h(col.filter.type, {
                                props: {
                                    // icon: 'ios-search'
                                    type: col.filter.stype ? col.filter.stype : 'text',
                                    placeholder: col.filter.placeholder ? col.filter.placeholder : "",
                                    size: col.filter.size ? col.filter.size : 'default'
                                },
                                style: {
                                    width: col.width ? col.width - 40 + 'px' : null,
                                    margin: col.filter.margin ? col.filter.margin : '5px auto'
                                },
                                on: {
                                    input: val => {
                                        inputValue = val;
                                        if (!inputValue) {
                                            this.validInputValue(col, inputValue);
                                        }

                                    },
                                    // input框后面的搜索按钮的点击事件
                                    'on-click': () => {
                                        this.validInputValue(col, inputValue);
                                    },
                                    'on-enter': () => {
                                        this.validInputValue(col, inputValue);
                                    }
                                }
                            })
                        };
                    } else if (col.filter.type && col.filter.type === 'DatePicker') {
                        // 如果是时间框
                        renderHeader = (h) => {
                            let inputValue = {};
                            return h(col.filter.type, {
                                props: {
                                    type: col.filter.stype ? col.filter.stype : "date",
                                    placement: "bottom-end",
                                    placeholder: col.filter.placeholder ? col.filter.placeholder : "查询日期",
                                    size: col.filter.size ? col.filter.size : 'default'
                                },
                                style: {
                                    width: col.width ? col.width - 40 + 'px' : null,
                                    margin: col.filter.margin ? col.filter.margin : '5px auto'
                                },
                                on: {
                                    'on-change': (val) => {
                                        if (!val) {
                                            this.$delete(this.search, col.key);
                                        } else {
                                            this.$set(this.search, col.key, val);
                                        }
                                        this.load();
                                        console.log(this.search)
                                    },
                                    // 'on-clear': () => {
                                    //     alert(3)
                                    //     this.$delete(this.search, col.key);
                                    //     this.load();
                                    // },
                                }
                            })
                        };
                    }
                    this.$set(childColumn, 'renderHeader', renderHeader);
                    children.push(childColumn);
                    this.$set(col, 'children', children);
                }
            }
        }

        createOptionsRender(col, h) {
            // 选项渲染
            let optionRender = [];
            if (col.filter.options) {
                let options = col.filter.options;
                for (let option of options) {
                    optionRender.push(h('Option', {
                        props: {
                            value: option.value
                        }
                    }, option.name))
                }
            }
            return optionRender;
        }

        // 重新加载数据
        load() {
            // 会执行一个load的事件
            this.$emit('on-search', this.search);
        }

        // 验证输入框的值
        validInputValue(col, inputValue) {
            if (!inputValue) {
                this.$delete(this.search, col.key);
                this.load();
                return;
            }
            this.$set(this.search, col.key, inputValue);
            this.load();
        }

        onCurrentChange(currentRow, oldCurrentRow) {
            this.$emit('on-current-change', currentRow, oldCurrentRow)
        }

        onRowDbclick(currentRow, index) {
            this.$emit('on-row-dblclick', currentRow, index)
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
