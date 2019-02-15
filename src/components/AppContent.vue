<template>
    
    <div class="container">

        <div>
            <main class="main">
                <!-- <textarea-autosize v-model="pangram" class="textarea" placeholder="Type here"></textarea-autosize> -->
                <EditableText v-model="pangram" class="textarea" :highlight="highlighted" />
                <div class="counter">{{ pangramLength }}</div>
            </main>
            
            <AlphabetChecker :pangram="pangram" @typed="typed" @backspace="backspace" @highlight="highlighted = $event" :highlight="highlighted" />

            <div class="suggestions" v-if="suggestions.length > 0">
                <span v-for="suggestion in suggestions" :key="suggestion" @click="addWord(suggestion, true)" class="suggestion">{{ suggestion }}</span>
            </div>

            <div class="suggestions" v-if="hints.length > 0">
                <span v-for="hint in hints" :key="hint" @click="addWord(hint, false)" class="suggestion">{{ hint }}</span>
            </div>
        </div>

        <div class="flex buttons-area">
            <div class="btn clear" @click="clear()">Clear</div>
            <div class="btn suggestion" @click="hint()">Suggestion</div>
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
            highlighted: '',
            dictionary: ['Acetaldehyde', 'Acid Rest', 'Acrospire', 'Adjunct', 'Aeration', 'Alcohol', 'ABV', 'Alcoholic', 'Ale', 'Airlock', 'Saccharomyces cerevisiae', 'Extract', 'Alpha Acid', 'Alpha Amylase', 'Beta Amylase', 'Aroma', 'Astringency', 'Attenuation', 'Barley', 'Barrel', 'Beta Acids', 'Bitterness', 'IBU', 'BJCP', 'Body', 'Boiling', 'Bottle', 'Bottom Fermentation', 'Brettanomyces', 'Brewpub', 'Brew Kettle', 'Sulphur', 'Carbon Dioxide', 'Carbonation', 'Cellaring', 'Chill', 'Haze', 'Hazy', 'Cold Break', 'Color', 'Craft', 'Decoction Mash', 'Plato', 'Dextrin', 'Diacetyl', 'Diastatic', 'Dry Hopping', 'Essential Hop Oils', 'Esters', 'Ethanol', 'Fermentable Sugars', 'Fermentation', 'Fermentation Lock', 'Filtration', 'Filter', 'Final gravity', 'Flocculation', 'Forced Carbonation', 'Fresh Hopping', 'Germination', 'Grainy', 'Grist', 'Growler', 'Gruit', 'Head Retention', 'Homebrewing', 'Hops', 'Hopping', 'Humulene', 'Hydrometer', 'Immersion Chiller', 'Infusion', 'Inoculate', 'Irish Moss', 'Gelatine', 'Keg', 'Kraeusen', 'Lactobacillus', 'Lager', 'Lagering', 'Lightstruck', 'Liquorous', 'Lovibond', 'Malt', 'Malt Extract', 'Maltose', 'Mash', 'Mashing', 'Mash Out', 'Microbrewery', 'Mouthfeel', 'Musty', 'Myrcene', 'Natural Carbonation', 'Ninkasi', 'Nitrogen', 'Noble Hops', 'Oasthouse', 'Original Gravity', 'OG', 'Oxidation', 'pH', 'Phenols', 'Pitching', 'Primary Fermentation', 'Priming', 'Prohibition', 'Racking', 'Real Ale', 'Residual', 'Alkalinity', 'Residual Sugar', 'Resin', 'Saccharification', 'Saccharomyces', 'Secondary Fermentation', 'Session Beer', 'Solvent', 'Sorghum', 'Sour', 'Sparging', 'Specific Gravity', 'Sulfur Aroma', 'Tannins', 'Temperature Rests', 'Top Fermentation', 'Trigeminal Nerves', 'Trub', 'Turbidity', 'Volatile', 'Water', 'Wet Hopping', 'Whirlpool', 'Wort', 'Yeast', 'Zymurgy', 'Admiral', 'Agnus', 'Ahtanum', 'AlphAroma', 'Amarillo', 'Amethyst', 'Apollo', 'Aramis', 'Atlas', 'Aurora', 'Beata', 'Belma', 'Bitter Gold', 'Boadicea', 'Bobek', 'Bouclier', 'Bramling Cross', 'Bravo', 'Brewers Gold', 'British Kent Goldings', 'Bullion', 'Calicross', 'California Cluster', 'Calypso', 'Cascade', 'Cashmere', 'Cekin', 'Celeia', 'Centennial', 'Challenger', 'Chelan', 'Chinook', 'Cicero', 'Citra', 'Cluster', 'Cobb’s Golding', 'Columbia', 'Columbus', 'Comet', 'Dana', 'Delta', 'Dr. Rudi', 'Early Green', 'El Dorado', 'Ella', 'Endeavour', 'Equinox', 'Eroica', 'Falconers Flight', 'First Gold', 'Flyer', 'Fuggle', 'Galaxy', 'Galena', 'Glacier', 'Golding', 'Green Bullet', 'Hallertau', 'HBC 342 Experimental', 'Helga', 'Herald', 'Herkules', 'Hersbrucker', 'Horizon', 'Huell Melon', 'Ivanhoe', 'Jester', 'Junga', 'Kazbek', 'Kohatu', 'Liberty', 'Lubelski', 'Magnum', 'Mandarina Bavaria', 'Mathon', 'Marynka', 'Meridian', 'Merkur', 'Millennium', 'Mittelfruh', 'Mosaic', 'Motueka', 'Mt. Hood', 'Mt. Rainier', 'Nelson Sauvin', 'Newport', 'Northdown', 'Northern Brewer', 'Nugget', 'Opal', 'Orbit', 'Orion', 'Outeniqua', 'Pacific Gem', 'Pacific Jade', 'Pacific Sunrise', 'Pacifica', 'Palisade', 'Perle', 'Phoenix', 'Pilgrim', 'Pilot', 'Pioneer', 'Polaris', 'Premiant', 'Pride of Ringwood', 'Progress', 'Rakau', 'Riwaka', 'Saaz', 'Santiam', 'Saphir', 'Satus', 'Select', 'Serebrianka', 'Simcoe', 'Sladek', 'Smaragd', 'Sonnet', 'Sorachi Ace', 'Southern Brewer', 'Southern Cross', 'Southern Promise', 'Southern Star', 'Sovereign', 'Spault', 'Spaulter Select', 'Sterling', 'Strickelbract', 'Strisselspault', 'Styrian Gold', 'Styrian Golding', 'Summer', 'Summit', 'Super Galena', 'Super Pride', 'Sussex', 'Sybilla', 'Sylva', 'Tahoma', 'Tardif de Burgogne', 'Target', 'Taurus', 'Teamaker', 'Tettnanger', 'Tillicum', 'Topaz', 'Tradition', 'Triple Pearl', 'Triskel', 'Ultra', 'Universal', 'Vanguard', 'Victoria', 'Viking', 'Vital', 'Vojvodina', 'Wai-iti', 'Waimea', 'Wakatu', 'Warrior', 'Whitbread Goldings', 'Willamette', 'Yakima Cluster', 'Yakima Gold', 'Zenith', 'Zythos', 'Pale Malt', 'Wheat Malt', 'Rye Malt', 'Vienna Malt', 'Munich Malt', 'Carapils', 'Caramel', 'Crystal', 'Victory Malt', 'Special Roast', 'Chocolate Malt', 'Roasted Barley', 'Black Barley', 'Weizen', 'Melanoidin', 'Amber Malt', 'Aromatic', 'Belgian Special', 'Biscuit Malt', 'CaraVienne', 'CaraMunich', 'Rauchmalt'],
            hints: []
        }
    },
    computed: {
        pangramLength() {
            return this.pangram.replace(/ /g,'').length
        },
        suggestions() {
            const toArray = this.pangram.split(' ');
            const lastWord = toArray[toArray.length - 1];
            console.log('lastWord: ', lastWord)
            if (lastWord && lastWord != '') {
                const suggestions = this.dictionary.filter(item => item.toLowerCase().search(lastWord.toLowerCase()) > -1)
                return suggestions
            } else {
                return []
            }
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
        },
        addWord: function(word, replace = false) {
            const toArray = this.pangram.split(' ');
            if (replace) {
                toArray[toArray.length - 1] = word + ' ';
            } else {
                toArray.push(word)
            }
            this.pangram = toArray.join(' ')
        },
        hint() {
            const alphabet = ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p', 'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'z', 'x', 'c', 'v', 'b', 'n', 'm']
            const toArray = this.pangram.split('').map(item => item.toLowerCase());
            const missingLetters = alphabet.filter(item => !toArray.includes(item.toLowerCase()))
            if (missingLetters.length > 0) {
                const allLetters = this.dictionary.filter(item => {
                    const hints = missingLetters.filter(missing => {
                        return item.toLowerCase().search(missing.toLowerCase()) > -1
                    })
                    return missingLetters.length <= 4 ? hints.length == missingLetters.length : hints.length > missingLetters.length - 3
                })
                this.hints = allLetters
                setTimeout(() => {
                    this.hints = []
                }, 3000)
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
    font-size: 36px;
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
    z-index: 3;
}

.suggestions {
    width: 900px;
    margin: 36px auto;
}

.suggestion {
    border: 1px solid $color;
    padding: 6px;
    margin-right: 3px;
    margin-bottom: 3px;
    display: inline-block;
    border-radius: 50px;
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

    &.suggestion {
        width: 180px;
        background-color: $color;
        color: #fff;
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
