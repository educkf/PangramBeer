<template>
    <div>
        <aside class="alphabet">
            <span
                v-for="letter in alphabet" 
                :key="letter" 
                @click="$emit('typed', letter)"
                @mouseover="$emit('highlight', letter)" 
                @mouseleave="$emit('highlight', null)"
                class="letter" 
                :class="{filled: pangramArray.includes(letter)}">
                {{ letter }}
            </span>
            <span v-if="keyboard" class="letter space" @click="$emit('typed', ' ')">space</span>
            <span v-if="keyboard" class="letter backspace" @click="$emit('backspace')">←</span>
        </aside>
        <ToggleButton v-model="keyboard" :width="100" :labels="{checked: 'Keyboard', unchecked: 'Alphabet'}" />
    </div>

</template>

<script>
import ToggleButton from 'vue-js-toggle-button/src/Button.vue';

export default {
    components: {
        ToggleButton
    },
    props: {
        pangram: {
            type: String,
            required: true
        }
    },
    data: function() {
        return {
            keyboard: false,
        }
    },
    computed: {
        alphabet() {
            if (this.keyboard) {
                return ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p', 'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'z', 'x', 'c', 'v', 'b', 'n', 'm']
            } else {
                return ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
            }
        },
        pangramArray() {
            return this.pangram.toLowerCase().split('')
        }
    }
}
</script>

<style lang="scss" scoped>
@import 'src/assets/_variables.scss';

.alphabet {
    width: 92%;
    max-width: 650px;
    margin: 0 auto 24px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    .letter {
        border: 1px solid $gray;
        width: 36px;
        height: 36px;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 6px;
        margin: 0 3px 3px 0;
        user-select: none;

        &.filled {
            border-color: $color;
            background: rgba(66, 185, 131, 0.2);
            color: $color;
            font-weight: 700;
        }

        &.space {
            width: 75%;
        }
    }
}


</style>
