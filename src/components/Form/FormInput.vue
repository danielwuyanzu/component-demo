<template>
    <label>
        <input :type="type" :value="value" @input="onInput"/>
    </label>
</template>

<script>
    import emitter from "../../utils/emitter";

    export default {
        name: "FormInput",
        // 混入通用方法
        mixins: [emitter],
        inject: {
            form: {
                default: ''
            },
            formItem: {
                default: ''
            }
        },
        props: {
            type: {
                type: String,
                default: 'text'
            },
            value: {
                type: String,
                default: ''
            },
        },
        methods: {
            onInput(e) {
                let value = e.target.value;
                this.$emit('input', value);
                // 指定了 prop 属性则做校验
                if (this.formItem && this.formItem.prop) {
                    this.dispatch('FormItem', 'validate', value);
                }
            }
        },
    }
</script>

<style scoped>

</style>
