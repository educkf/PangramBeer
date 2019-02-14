<template>
    
    <div class="container">

        <div>
            <main class="main">
                <!-- <textarea-autosize v-model="pangram" class="textarea" placeholder="Type here"></textarea-autosize> -->
                <EditableText v-model="pangram" class="textarea" :highlight="highlight" />
                <div class="counter">{{ pangramLength }}</div>
            </main>
            <AlphabetChecker :pangram="pangram" @typed="typed" @backspace="backspace" @highlight="highlight = $event" />
        </div>

        <div class="flex buttons-area">
            <div class="btn clear" @click="clear()">Clear</div>
            <div class="btn" @click="save()">Save</div>
        </div>
        
        <div v-if="pangrams.length">
            <h3 class="sub-title">Saved</h3>
            <ul class="saved">
                <li class="saved-item" 
                    v-for="(item, i) in pangrams" 
                    :key="i"
                >
                    <span @click="pangram = item">{{ item }}</span>
                    <span @click="eraseItem(i)" class="remove">remove</span>
                </li>
            </ul>
        </div>
    </div>

</template>

<script>
import AlphabetChecker from '@/components/AlphabetChecker';
import { TextareaAutosize } from 'vue-textarea-autosize'
import EditableText from '@/components/EditableText'

export default {
    components: {
        AlphabetChecker,
        TextareaAutosize,
        EditableText
    },
    data: function() {
        return {
            pangram: '',
            pangrams: [],
            highlight: ''
        }
    },
    computed: {
        pangramLength() {
            return this.pangram.replace(/ /g,'').length
        }
    },
    methods: {
        typed: function(value) {
            this.pangram = this.pangram + value
        },
        backspace: function(value) {
            this.pangram = this.pangram.substr(0, this.pangram.length - 1)
        },
        save: function() {
            this.pangrams.push(this.pangram)
            this.saveToLocalStorage()
        },
        clear: function() {
            this.pangram = ""
        },
        eraseItem: function(index) {
            this.pangrams.splice(index, 1)
            this.saveToLocalStorage()
        },
        saveToLocalStorage: function() {
            localStorage.setItem('pangram', JSON.stringify(this.pangrams))
        },
        computeTyping: function(value) {
            const alphabet = ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p', 'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'z', 'x', 'c', 'v', 'b', 'n', 'm']
            console.log(value)
            if (alphabet.includes(value.key.toLowerCase())) {
                this.pangram = this.pangram + value.key
            } else if (value.key == 'ce') {
                this.pangram = this.pangram + ' '
            }
        }
    },
    created() {
        const pangrams = localStorage.getItem('pangram')
        if (pangrams) {
            this.pangrams = JSON.parse(pangrams)
        }
    }
}
</script>

<style lang="scss" scoped>
@import 'src/assets/_variables.scss';

.container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 80vh;
}

.main {
    margin: 24px auto 12px;
    width: 90%;
    max-width: 650px;
    position: relative;
}

.textarea {
    width: 100%;
    background: #eee;
    padding: 24px;
    font-size: 36px;
    color: $color;
    font-weight: 700;
    outline: none;
    border: none;
    resize: none;
    border-radius: 12px;
}

.counter {
    position: absolute;
    bottom: 0;
    right: 0;
    background: $color;
    padding: 6px 0;
    color: #fff;
    font-weight: 700;
    border-top-left-radius: 6px;
    border-bottom-right-radius: 6px;
    width: 40px;
    text-align: center;
}

.buttons-area {
    padding: 36px 0;
}

.btn {
    border: 1px solid $color;
    padding: 12px 0;
    text-transform: uppercase;
    color: $color;
    width: 120px;
    margin: 0 auto;
    border-radius: 50px;
    font-weight: 500;
    user-select: none;

    &.clear {
        border-color: $gray;
        color: $gray;
    }
}

.sub-title {
    font-weight: bold;
    margin: 24px auto 12px;
}

.saved {
    width: 90%;
    max-width: 400px;
    margin: 0 auto;
    border-top: 1px solid $gray;
}

.saved-item {
    padding: 18px 0;
    border-bottom: 1px solid $gray;
    display: flex;
    justify-content: space-between;
    align-items: center;

    span {
        font-size: 12px; 
        text-align: left;

        &.remove {
            color: $color;
            cursor: pointer;
            
            &:hover {
                text-decoration: underline; 
            }
        }
    }
} 

</style>
