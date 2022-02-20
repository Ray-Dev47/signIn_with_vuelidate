<template>
    <div class="formResult">
        <h2>Please find below submitted input data:</h2>
        <div v-for="(value, key, index) in formResult" :key="index">
            <span>{{ labels[key] || key }}: </span>
            <span> {{ value }} </span>
        </div>


    </div>
</template>

<script>
export default {
    data() {
        return {
            labels: {
                email: "Email",
                emailConfirm: "Email Confirm",
                userName: "Username",
                password: "Password",
                subscription: "Subscription",
                terms: "Terms"
            }
        }
    },
    props: {
        formResult: Object
    }
}
</script>

<style lang="scss">
@import "@/assets/styles/_form";
</style>