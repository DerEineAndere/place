<script setup lang="ts">

const credentials = reactive({
    username: "",
    password: "",
});

credentials.username = "";

function goTo() {
    navigateTo("");
};
//navigateTo("");
</script>

<template>
    <h1>Login</h1>
    <input type="text" v-model="credentials.username" placeholder="username">
    <br>
    <input type="password" v-model="credentials.password" placeholder="pissword">
    <br>
    <button v-on:click=goTo()>Login</button>
    <p>{{ credentials.username }}</p>
    <p>{{ credentials.password }}</p>

</template>
