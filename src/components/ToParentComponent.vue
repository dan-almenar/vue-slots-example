<template>
    <slot :loggedUsers="loggedUsers" />
</template>

<script setup>
    const loggedUsers = ( users ) => {
        return users.filter( (user) => user.isLogged )
    } 
</script>

<style>

</style>