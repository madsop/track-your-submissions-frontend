<template>
  <div class="wrapper">
    <h1>My submissions to conferences, meetups and other events</h1>
    <AddTalk />
    <MySubmissions />
  </div>
</template>

<script>

import AddTalk from '@/components/AddTalk.vue';
import MySubmissions from '@/components/MySubmissions.vue';

export default {
  components: {
    AddTalk,
    MySubmissions,
  },
}
</script>

<style lang="scss" scoped>

.wrapper {
  display: grid;
  align-items: center;
  justify-content: center;
  grid-template-columns: 1fr;
  margin: 0 auto;
}

</style>
