<template>
    
    <div class="editfield">
        <span style="float: right; margin: 10px 30px 0 0" @click="onDeleteField">×</span>
        <h4>Type</h4>
        <p><span
            style="border-radius: 10px; display: inline-block; margin-right: 5px; padding: 0px 4px; line-height: 1.6em; border: 1px solid #ccc; cursor: pointer; font-size: 0.8em"
            :style="{background: field.type === type ? '#ccc' : 'none'}"
            v-for="type in types"
            @click="field.type = type"
        >{{ type }}
        </span></p>
        <template v-if="field.type === 'input'">
            <h4>Label</h4>
            <input
                type="text"
                v-model="field.label"
            >
            <h4>Placeholder</h4>
            <input
                type="text"
                v-model="field.placeholder"
            >
        </template>
        <template v-if="field.type === 'textarea'">
            <h4>Label</h4>
            <input
                type="text"
                v-model="field.label"
            >
            <h4>Rows: {{ field.rows }}</h4>
            <input
                type="range"
                v-model="field.rows"
                min="1"
                max="8"
            >
            <h4>Placeholder</h4>
            <input
                type="text"
                v-model="field.placeholder"
            >
        </template>
        <template v-if="field.type === 'select' || field.type === 'radio'">
            <h4>Label</h4>
            <input
                type="text"
                v-model="field.label"
            >
            <h4>Options</h4>
            <textarea v-model="options" rows="5"></textarea>
        </template>
        <template v-if="field.type === 'help'">
            <h4>Help</h4>
            <input type="text" v-model="field.help">
        </template>

    </div>

</template>

<script>

    export default {
        props: {
            field: { default: () => {} }
        },
        data: () => ({
            types: ['input', 'textarea', 'select', 'checkbox', 'radio', 'help', 'line'],
            options: ''
        }),
        mounted() {
            this.options = this.field.options.join("\n")
        },
        methods: {
            onDeleteField() {
                var doDelete = confirm('Are you sure you want to delete a field?')
                if (doDelete) {
                    this.$emit('deleteField')
                }
            }
        },
        watch: {
            options() {
                this.field.options = this.options.split(/\n/)
            }
        }
    }

</script>

<style>
    .editfield {
        padding: 0.5em 40px 1em 1em;
        margin-bottom: 10px;
        border-bottom: 1px solid #ddd;
        background: white;
    }
    .editfield h4 {
        font-weight: normal;
    }
</style>
