<template>
    <button @click="foo" class="foo">Nested Foo</button>
    <slot/>
</template>

<script setup>
const foo = () => {
    alert('foo')
}
</script>


<style scoped>
* {
    margin: 0 auto;
}
button {
    width: 350px;
    margin-top: 25px;
    background: none;
    border: 3px solid blue;
    border-radius: 8px;
    font-size: 1.2rem;
    color: firebrick;
}
</style>