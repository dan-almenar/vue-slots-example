<template>
    <slot :loggedUsers="loggedUsers" />
</template>

<script>
export default {
    name: 'ToParentComponent'
}
</script>

<script setup>
    const loggedUsers = ( users ) => {
        return users.filter( (user) => user.isLogged )
    } 
</script>

<style>

</style>