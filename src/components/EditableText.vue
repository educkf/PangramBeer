<template>
    <div contenteditable="contenteditable" ref="textarea" @input="updateInput">
        {{ content }}
    </div>
</template>

<script>
export default {
    props: ['value', "highlight"],
    data () {
        return { 
            content: '' 
        }
    },
    methods: {
        updateInput () {
            this.$emit('input', this.$refs.textarea.innerText)
        }
    },
    watch: {
        value: function(newValue, oldValue) {
            if (newValue != this.$refs.textarea.innerText) 
                this.$refs.textarea.innerText = newValue
        },
        highlight: function(newValue) {}
    }
}
</script>