<template>
    <div class="form-item">
        <span>{{label}} </span>
        <slot/>
        <div class="error" v-if="error">{{error}}</div>
    </div>
</template>

<script>
    import Schema from "async-validator";
    import emitter from "../../utils/emitter";

    export default {
        name: "FormItem",
        componentName: "FormItem",
        // 接收上级组件的数据
        inject: ['mForm'],
        mixins: [emitter],
        provide() {
            return {
                formItem: this
            }
        },
        props: {
            label: {
                type: String,
                default: ""
            },
            // 校验是以 FormItem 为单位进行的，所以每个 FormItem 只能校验一个字段
            prop: {
                type: String,
            }
        },
        data() {
            return {
                error: "",
            }
        },
        mounted() {
            this.$on('validate', this.validateValue);
            this.prop && this.dispatch('MForm', 'addField', [this]);
            console.log(this.prop)
        },
        methods: {
            validateValue() {
                const value = this.mForm.$attrs.model[this.prop];
                const rules = this.mForm.$attrs.rules;
                const description = this.filterRules(rules, this.prop);
                const validator = new Schema(description);
                validator.validate({[this.prop]: value}, (errors) => {
                    if (errors) {
                        this.error = errors.pop().message
                    } else {
                        this.error = ''
                    }
                    // validation passed
                });
            },
            filterRules(allRules, prop) {
                return {[prop]: allRules[prop]}
            }
        },
    }
</script>

<style scoped>
    .error {
        color: orangered;
    }

    .form-item + .form-item {
        margin-top: 1em;
    }
</style>
