<template>
  <div>
    <Rank></Rank>
  </div>
</template>

<script>
import Rank from "./rank"
export default {
  components: {
    Rank
  },
  methods: {
    onGotUserInfo(e) {
      console.log(e)
    }
  },
  onload() {

  },
}
</script>

<style>

</style>
