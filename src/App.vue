<template>
  <div id="app">
    <FormInput  v-if="showForm" @submit="submit"/>
    <FormResult :formResult="formResult"  v-else/>
  </div>
</template>

<script>
import FormInput from "./components/FormInput.vue";
import FormResult from "./components/formResult.vue"

export default {
  name: "App",
  components: {
    FormInput,
    FormResult
  },
  data (){
    return {
       showForm: true,
       formResult: {}
    }
  },
  methods:{
      submit(formResult){
         this.showForm = false
         this.formResult = formResult
      }
    }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  margin-top: 60px;
}
</style>
