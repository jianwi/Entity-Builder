<template>
    <div>
        <div class="input-group">
            <span class="input-group-addon">Search</span>
            <input ref="filter" v-model="filter" v-on:keyup.esc="clear" type="text" class="form-control" />
        </div>
        <br />
        <ul class="list-group select-list">
            <li v-for="item in list" :value="item" @click="select(item)" :key="text(item)" class="list-group-item">
                <div>{{ text(item) }}</div>
                <div v-if="lData.infoName">{{ info(item) }}</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export class ListData {
        /**
         * item list
         * member name of item, for displaying
         * member name of item, for displaying extra info
         * call back function
         **/
        constructor(list, textName, infoName, CallBack) {
            this.list = list;
            this.textName = textName;
            this.infoName = infoName;
            this.CallBack = CallBack;
        }
    }

    export const List = {
        name: 'List',
        props: ['lData'],
        data() {
            return {
                filter: ''
            };
        },
        mounted() {
            this.$refs.filter.focus();
        },
        computed: {
            list() {
                if (this.filter) {
                    return this.lData.list.filter(item => {
                        let text = this.text(item);
                        return text.toLowerCase().indexOf(this.filter) >= 0;
                    });
                }
                return this.lData.list;
            }
        },
        methods: {
            text(item) {
                if (this.lData.textName) {
                    return this.point(item[this.lData.textName]);
                }
                return this.point(item);
            },
            point(text) {
                if (text) {
                    return text;
                }
                return '...';
            },
            info(item) {
                if (this.lData.infoName) {
                    return item[this.lData.infoName];
                }
                return item;
            },
            select(item) {
                if (this.lData.CallBack) {
                    this.lData.CallBack(item);
                }
                this.$emit('select', item);
            },
            clear() {
                if (this.filter) {
                    this.filter = '';
                } else {
                    this.$emit('close');
                }
            }
        }
    };

    export default List;
</script>

<style>
    .select-list li {
        cursor: pointer;
    }

    .list-group {
        max-height: 400px;
        margin-bottom: 10px;
        overflow-y: auto;
    }
</style>
