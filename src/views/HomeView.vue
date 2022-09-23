<template>
  <Banner />
  <div class="main-cont">
    <h1>Hora de montar seu Sanduba!!</h1>
    <SandForm />
  </div>
</template>

<script>
import Banner from "../components/Banner.vue"
import SandForm from "../components/SandForm.vue"

export default {
    name: "Home",
    components: {
        Banner,
        SandForm
    }   
}

</script>
