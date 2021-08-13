<template>
    <div>
        <m-form :model="model" :rules="rules" ref="mForm">
            <form-item label="a label" prop="field1">
                <form-input v-model="model.field1"/>
            </form-item>
            <form-item label="name" prop="name">
                <form-input v-model="model.name"/>
            </form-item>
            <form-item>
                <button @click="validateForm">Validate</button>
            </form-item>
        </m-form>
        <h1>{{model.field1}}</h1>

    </div>
</template>

<script>
    import MForm from "./Form/MForm";
    import FormInput from "./Form/FormInput";
    import FormItem from "./Form/FormItem";

    export default {
        name: "Test",
        components: {FormItem, FormInput, MForm},
        data() {
            return {
                model: {
                    field1: '',
                    name: '',
                },
                rules: {
                    field1: [
                        {required: true},
                        {
                            validator(rule, value) {
                                const errors = [];
                                // test if email address already exists in a database
                                // and add a validation error to the errors array if it does
                                if (!/^\d+$/.test(value)) {
                                    errors.push("it must be integer")
                                }
                                return errors;
                            }
                        }
                    ],
                    name: [{required: true}],
                }
            }
        },
        methods: {
            validateForm() {
                this.$refs.mForm.validateForm();
            }
        },
    }
</script>

<style scoped>

</style>
