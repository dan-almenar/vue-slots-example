<template>
<div class="wrapper">
<div class="header">
    <h1>Vue {{ '<slot>' }} example</h1>
    <h3>by <a href="https://danielalmenar.com">Dan Almenar</a></h3>
    <h3>Click <a href="https://github.com/dan-almenar/vue-slots-example" target="blank">here</a> 
    to see the complete code of this app.</h3>
</div>
<div class="content-block">
<h2>Normal rendering:</h2>
<p>(The data displayed in this block is defined in the <span class="parent">ParentComponent</span>,
    yet this data is subject to be changed by data comming from the <span class="from-parent">
    FromParentComponent</span>...)</p>
</div>
<div class="users-list">
    <h2>Current users in database:</h2>
    <div class="user" v-for="user in users" :key="user.name">
        <ul>
            <li>{{ user.name }}</li>
            <p>{{ user.email }}</p>
            <p>{{ user.password }}</p>
        </ul>
    </div>
    <div class="content-block">
        <h2>Placing items into child components and receiving data from them:</h2>
        <p>(The submit button is passed to the <span class="from-parent">FromParentComponent</span> directly from the
        <span class="parent">ParentComponent</span>
            and when it is clicked it calls a function defined in the <span class="parent">ParentComponent</span> with data
            passed by the <span class="from-parent">FromParentComponent</span> via the {{ '<slot>' }} tag.)</p>
        <FromParentComponent v-slot='{ userName, userEmail, userPassword }'>
            <button @click="handleUsers(userEmail, userPassword, userName)">Submit</button>
        </FromParentComponent>
    </div>
    <div class="content-block">
        <h2>Receiving functions:</h2>
        <p>(The list of users is getting filtered by a function defined on the <span
        class="to-parent">ToParentComponent</span> and passed to the <span class="parent">
        ParentComponent</span> via the {{ '<slot>' }} tag.)</p>
        <ToParentComponent v-slot="{ loggedUsers }">
            <h2>Currently logged users:</h2>
            <ul v-for="user in loggedUsers(users)" :key="user.name">
                <li>{{ user.name }}</li>
                <p> {{ user.email }}</p>
            </ul>
        </ToParentComponent>
    </div>
</div>
</div>
</template>

<script setup>
import { provide } from 'vue'
import FromParentComponent from '@/components/FromParentComponent.vue'
import ToParentComponent from '@/components/ToParentComponent.vue'
import { ref } from 'vue'

const clearForm = ref(false)
provide('clearForm', clearForm)

const users = ref([
    {
        name: 'John Doe',
        email: 'johndoe@example.com',
        password: 'johnpassword',
        isLogged: true
    },
    {
        name: 'Jane Doe',
        email: 'janedoe@example.com',
        password: 'janepassword',
        isLogged: false
    },
    {
        name: 'Jack Doe',
        email: 'jackdoe@example.com',
        password: 'jackpassword',
        isLogged: false
    },
    {
        name: 'Jill Doe',
        email: 'jilldoe@example.com',
        password: 'jillpassword',
        isLogged: false
    }
])
const handleUsers = (email, password, name) => {
    // form validation (for simplicity this is not checking if the email is an actual email)
    if (email === '' || password === '') {
        alert('Please fill in all fields')
        return
    }
    const user = users.value.find(user => user.email === email)
    if (user) {
        if (password === user.password) {
            user.isLogged = true
        } else {
            alert('Wrong password')
        }
    } else {
        users.value.push({
            name: name === '' ? 'Unknown' : name,
            email: email,
            password: password,
            isLogged: true
        })
    }

    //set state for child to clear form:
    clearForm.value = true
    setTimeout(() => {
        clearForm.value = false
    }, 300)
}   
</script>

<style scoped>
.wrapper {
    max-width: 600px;
    margin: 50px auto;
}
.header >*, a {
    font-weight: bold;
}
span {
    font-size: 1.3rem;
    font-weight: bold;
}
.parent {
    color: firebrick;
}
.from-parent {
    color: green;
}
.to-parent {
    color: blue;
}
.content-block {
    margin-top: 30px;
}
h3 {
    width: 300px;
    text-align: center;
}
.content-block > h2 {
    font-size: 1.6rem;
    margin-bottom: 10px;
    font-weight: bold;
}
.content-block > p {
    font-size: 1.2rem;
    margin-bottom: 10px;
}
button {
    width: 350px;
    margin: 5px auto;
    background: none;
    border: 3px solid blue;
    border-radius: 8px;
    font-size: 1.2rem;
    color: firebrick;
}
</style>