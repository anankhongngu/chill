<template>
    <NavBar/>
    <main class ="w3-main" style="margin-left: 300px;">
        <router-view></router-view>
    </main>
</template>
<script>
import NavBar from './components/NavBar/NavBar.vue';
export default { 
    data() {
       return {
             
       }
    },
    components: {
      NavBar
    },
    computed: {
        
    },
    created(){
        
    }
}
</script>

<style>

</style>