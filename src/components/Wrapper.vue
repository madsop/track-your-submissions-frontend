<template>
  <div class="wrapper">
    <h1>My submissions to conferences, meetups and other events</h1>
    <div class="content">
      <AddTalk />
      <MySubmissions />
    </div>
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

div {
  justify-content: center;
  margin: 0 auto;
}

div.content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  font-size: 1.3em;
}


</style>