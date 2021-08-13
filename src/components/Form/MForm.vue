<template>
    <div>
        <slot/>
    </div>
</template>

<script>

    import emitter from "../../utils/emitter";

    export default {
        name: "MForm",
        componentName: "MForm",
        mixins: [emitter],
        // 提供数据给子组件访问
        provide() {
            return {
                mForm: this,
            }
        },
        data() {
            return {
                fields: []
            }
        },
        created() {
            // 子组件先挂载，父组件后挂载，所以需要在子组件 mounted() 之前设置事件监听
            this.$on('addField', (field) => {
                this.fields.push(field)
            })
        },
        methods: {
            validateForm() {
                // 让每个 FormItem 去校验自己的数据
                this.fields.forEach(field => {
                    field.validateValue()
                })
            }
        },
    }
</script>

<style scoped>

</style>
