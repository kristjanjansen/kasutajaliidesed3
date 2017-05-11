<template>

    <div class="fields" style="display: flex">
        <div class="fields__left">
        <div><component
            v-for="field in fields"
            is="ShowField"
            :field="field"
            key="field"
        >    
        </component>
        </div>
        </div>
        <div class="fields__right">
        <component
            v-for="(field, key) in fields"
            is="EditField"
            :field="field"
            @deleteField="deleteField(key)"
            key="field"
        >    
        </component>
        <big @click="addField">+ Add new form element</big>
        </div>
    </div>

</template>

<script>

    import ShowField from './ShowField.vue'
    import EditField from './EditField.vue'

    export default {
        components: { ShowField, EditField },
        methods: {
            addField() {
                this.fields.push({
                    type: 'input',
                    subtype: 'text',
                    label: 'I am a label',
                    placeholder: 'I am a placeholder',
                    rows: 5,
                    options: ['Some', 'Options'],
                    help: 'I am help text'
                })
            },
            deleteField(key) {
                this.$delete(this.fields, key)
            }
        },
        data: () => ({ fields: []}),
        mounted() {
            this.$watch(
                'fields',
                fields => localStorage.fields = JSON.stringify(fields),
                { deep: true }
            )
            
            if (localStorage.fields) {
                this.fields = JSON.parse(localStorage.fields)
            } else {
                this.addField()
            }
            
        }
    }

</script>

<style>
    .fields {
        height: 100vh;
        background: #eee;
    }
    .fields__left {
        font-family: sans-serif;
        line-height: 130%;
        width: 60%;
        display: flex;
        justify-content: center;
        padding-top: 2em;
        font-size: 0.8em;
    }
    .fields__left > div {
        width: 60%;
    }
    .fields__right {
        font-family: Rubik, sans-serif;
        font-size: 1rem;
        line-height: 130%;
        width: 40%;
    }
    input, textarea, select, option {
        font-size: 16px;
        border: 2px solid #ccc;
        padding: 0.5em;
        border-radius: 3px;
        width: 100%;
    }
    hr {
        height: 0;
        border-top: 1px solid #eaeaea;
        border-top: 1px solid #fff;
        width: 100%;
    }
    p {
        margin: 0.5em 0;
        display: block;
    }
    h4 {
        margin: 0.75em 0 0.5em 0;
    }
    h4:first-child {
        margin-top: 0;
    }
    small {
        opacity: 0.5;
        font-size: 0.6em;
        margin: 0.25em 0;
        display: block;
    }
    label, input {
        display: block;
    }
    input[type=radio], input[type=checkbox] {
        display: inline;
    }
    big {
        padding: 0.5em 0;
        font-size: 1em;
        cursor: pointer;
    }
    .showfield.checkbox {
        font-size: 0.9rem;
    }
</style>
