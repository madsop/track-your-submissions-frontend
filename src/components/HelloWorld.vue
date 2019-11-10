<template>
  <div class="wrapper">
    <h1>My submissions to conferences, meetups and other events</h1>
    <grid-container id="submission">
      <grid-item>Talk</grid-item>
      <grid-item>
        <select v-model="selected" v-if="talks.length > 0">
          <option v-for="talk in this.talks" v-bind:value="talk" v-bind:key="talk.id.id">
          {{ talk.title }}<span v-if="talk.cospeaker">, with cospeaker {{ talk.cospeaker }}</span>
          </option>
        </select>
      </grid-item>
      <grid-item><input v-model="conference" placeholder="Conference" /></grid-item>
      <grid-item><input v-model="time" placeholder="Time" /></grid-item>
      <grid-item><textarea v-model="notes" placeholder="Notes" /></grid-item>
      <grid-item><button v-on:click="saveSubmission">Save submission</button></grid-item>
    </grid-container>
    <grid-container v-for="submission in this.submissions" :key="submission.id.id" class="mySubmissions">
        <grid-item id="time">{{submission.time }}</grid-item>
        <grid-item id="conference">{{ submission.conference }}</grid-item>
        <grid-item id="title">{{submission.talk.title}}</grid-item>
        <grid-item id="status">{{submission.status}}</grid-item>
        <grid-item v-if="submission.notes" id="notes">{{ submission.notes }}</grid-item>
    </grid-container>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  data() {
    return {
      selected: undefined,
      conference: '',
      time: '',
      notes: '',
      talks: [],
      submissions: [],
      baseurl: 'http://localhost:8080', 
    } 
  },
  props: {
  },
  created() {
    var self = this;
    axios.get(self.baseurl + '/talks')
    .then(function (response) {
      self.talks = response.data;
    });
    axios.get(self.baseurl + '/cfp')
    .then(function(response) {
      self.submissions = response.data;
    })
  },
  methods: {
    saveSubmission() {
      var submission = {
        'conference': this.conference,
        'time': this.time,
        'talk': {
           id: this.selected.id.id
         },
        'notes': this.notes
      }
      axios.post(this.baseurl + '/cfp/add', submission)
    }
  },
}
</script>

<style lang="scss" scoped>

input, textarea, button {
  width: 300px;
  margin: 10px;
}

textarea {
  height: 120px;
}

.wrapper {
  display: grid;
  align-items: center;
  justify-content: center;
  grid-template-columns: 1fr;
}

grid-container {
  display: grid;
}

grid-container.mySubmissions {
  grid-template-columns: 1fr 2fr 3fr 1fr 1fr;
}

grid-container#submission {
  grid-template-columns: 1fr;
  align-self: center;
  justify-self: center;
  margin-bottom: 1em;
}

#submission grid-item {
  align-self: center;
  justify-self: center;
}

grid-item {
  display: flex;
  justify-content: left;
  $mobile-max: 700px;
  text-align: left;
  @media screen and (max-width: $mobile-max) {
      padding: 0.2rem 0.1rem;
      margin: 0.3rem 0.1rem;
      word-break: break-word;
  }
  @media screen and (min-width: $mobile-max) {
      padding: 0.2rem 1.5rem;
  }
}

</style>
