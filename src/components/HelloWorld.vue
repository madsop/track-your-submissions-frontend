<template>
  <div class="wrapper">
    <h1>My submissions to conferences, meetups and other events</h1>
    <AddTalk />
    <section v-for="submission in this.submissions" :key="submission.id.id" class="mySubmissions">
        <div id="time">{{submission.time }}</div>
        <div id="conference">{{ submission.conference }}</div>
        <div id="title">{{submission.talk.title}}<span v-if="submission.talk.cospeaker">, with cospeaker {{ submission.talk.cospeaker }}</span></div>
        <div id="status">{{submission.status}}</div>
        <div v-if="submission.notes" id="notes">{{ submission.notes }}</div>
    </section>
  </div>
</template>

<script>

import axios from 'axios';
import AddTalk from '@/components/AddTalk.vue';

export default {
  data() {
    return {
      conference: '',
      time: '',
      notes: '',
      submissions: [],
      baseurl: 'http://localhost:8080', 
    } 
  },
  props: {
  },
  created() {
    var self = this;
    axios.get(self.baseurl + '/cfp')
    .then(function(response) {
      self.submissions = response.data;
    })
  },
  components: {
    AddTalk,
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

section.mySubmissions {
  display: grid;
  grid-template-columns: 1fr 2fr 3fr 1fr 1fr;
}

</style>
