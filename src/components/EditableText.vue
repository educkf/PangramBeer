<template>
    <div class="editable-text">
        <div class="editable-textarea" contenteditable="contenteditable" ref="textarea" @input="updateInput">
            {{ content }}
        </div>
        <div class="editable-highlight" v-html="hContent"></div>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: String,
            required: false
        }, 
        highlight: {
            type: String,
            required: false
        }
    },
    data () {
        return { 
            content: '',
            hContent: ''
        }
    },
    methods: {
        updateInput () {
            this.$emit('input', this.$refs.textarea.innerText)
            this.hContent = this.$refs.textarea.innerText
        }
    },
    watch: {
        value: function(newValue, oldValue) {
            if (newValue != this.$refs.textarea.innerText) 
                this.$refs.textarea.innerText = newValue
                this.hContent = newValue
        },
        highlight: function(newValue) {
            console.log(newValue)

            if (newValue != null) {
                this.hContent = this.$refs.textarea.innerText
                const hArray = this.hContent.split(' ')
                const newhArray = hArray.map(item => {
                    return item.search(newValue) > -1 ? `<span>${item}</span>` : item
                })
                this.hContent = newhArray.join(' ');
            }

            if (newValue == null) {
                this.hContent = this.$refs.textarea.innerText
            }
            
        }
    }
}
</script>

<style lang="scss" scoped>
.editable-text {
    position: relative;

    .editable-textarea {
        width: 100%;
        padding: 24px;
        font-size: 36px;
        color: #42b983;
        font-weight: 700;
        outline: none;
        border: none;
        resize: none;
        border-radius: 12px;
        position: relative;
        z-index: 2;
    }

    .editable-highlight {
        width: 100%;
        padding: 24px;
        font-size: 36px;
        color: transparent;
        font-weight: 700;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 1;

        span {
            background: yellow;
        }
    }
}
</style>

<style lang="scss">
.editable-highlight {
    span {
        background: yellow;
    }
}
</style>

