<template>
  <div class="home">
    <RiskInterface />
  </div>
</template>

<script>
// @ is an alias to /src
import RiskInterface from "@/components/RiskInterface.vue";

export default {
  name: "Home",
  components: {
    RiskInterface
  }
};
</script>
