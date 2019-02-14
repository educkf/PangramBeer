<template>
    
    <div>
        <main class="main">
            <textarea v-model="pangram" class="textarea" placeholder="Type here"></textarea>
        </main>
        <AlphabetChecker :pangram="pangram" @typed="typed" @backspace="backspace" />
    </div>

</template>

<script>
import AlphabetChecker from '@/components/AlphabetChecker';

export default {
    components: {
        AlphabetChecker
    },
    data: function() {
        return {
            pangram: ''
        }
    },
    methods: {
        typed: function(value) {
            this.pangram = this.pangram + value
        },
        backspace: function(value) {
            this.pangram = this.pangram.substr(0, this.pangram.length - 1)
        }
    }
}
</script>

<style lang="scss" scoped>
@import 'src/assets/_variables.scss';

.main {
    padding: 24px 0 12px;
    width: 90%;
    max-width: 650px;
    margin: 0 auto;
}

.textarea {
    width: 100%;
    min-height: 120px; 
    background: #eee;
    padding: 36px;
    font-size: 36px;
    color: $color;
    font-weight: 700;
    outline: none;
    border: none;
    resize: none;
    border-radius: 12px;
}


</style>
