<template>
    
    <footer class="footer">
        <p class="text">Pangram Beer - Dumb sh*t project by <a href="https://github.com/educkf/PangramBeer" target="_blank">@educkf</a></p>
    </footer>

</template>

<script>
export default {
    
}
</script>

<style lang="scss" scoped>
@import 'src/assets/_variables.scss';

.footer {
    width: 100%;
    border-top: 1px solid $gray;
    padding: 12px 0;
    margin-top: 36px;

    .text {
        font-size: 12px;
        color: $text;
        font-weight: 500;
    }
}


</style>
