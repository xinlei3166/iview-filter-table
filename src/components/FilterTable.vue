<template>
    <div>
        <Table :border="border" :height="height" :highlight-row="highlightRow" :no-data-text="noDataText" :no-filtered-data-text="noFilteredDataText" :columns="columns" :data="data"></Table>
    </div>
</template>

<script>
    import Vue from 'vue'
    import Component from "vue-class-component";

    @Component({
        props: {
            columns: {type: Array, required: true},
            data: {type: Array, required: true},
            height: {type: Number, default: null},
            highlightRow: {type: Boolean, default: true},
            border: {type: Boolean, default: true},
            noDataText: {type: String, default: '暂无数据'},
            noFilteredDataText: {type: String, default: '没有找到你要搜索的数据'}
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
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px': null
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
                                    placeholder: '' + this.columns[index].title,
                                    icon: 'ios-search-strong'
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px' : null
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
                                    type: "daterange",
                                    placement: "bottom-end",
                                    placeholder: "Select date"
                                },
                                style: {
                                    width: this.columns[index].width ? this.columns[index].width - 40 + 'px' : null
                                },
                                on: {
                                    'on-change': (value) => {
                                        this.validInputValue(index, value);
                                    },
                                }
                            })
                        };
                    }
                    this.$set(childColumn, 'renderHeader', renderHeader);
                    children.push(childColumn);
                    this.$set(this.columns[index], 'children', children);
                    this.$delete(this.columns[index], 'key');
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
        }

        // 验证输入框的值
        validInputValue(index, inputValue) {
            if (!inputValue) {
                this.$delete(this.search, this.columns[index].key);
                this.load();
                return;
            }
            this.$set(this.search, this.columns[index].key, inputValue);
            this.load();
        }
    }
</script>

<style lang="stylus" scoped>
</style>
