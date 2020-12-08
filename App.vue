<template>
    <h1 class="asdf">{{msg}}</h1>
</template>

<script>
export default {
    data() {
        return {
            msg: 'Hello'
        }
    }
}
</script>

<style lang="scss" scoped>
$color: red;
.asdf {
    color: $color;
}
</style>