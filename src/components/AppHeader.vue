<template>
    
    <header class="header">
        <h1 class="title">Pangram Beer</h1>
    </header>

</template>

<script>
export default {
    
}
</script>

<style lang="scss" scoped>
@import 'src/assets/_variables.scss';

.header {
    width: 100%;
    border-bottom: 1px solid $gray;
    padding: 24px 0;
    text-transform: uppercase;

    .title {
        font-size: 36px;
        color: $color;
        font-weight: 700;
    }
}


</style>
