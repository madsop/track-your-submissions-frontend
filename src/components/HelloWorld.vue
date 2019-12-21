<template>
  <div class="wrapper">
    <h1>My submissions to conferences, meetups and other events</h1>
    <section id="submission">
      <div>Talk</div>
      <div>
        <select v-model="selected" v-if="talks.length > 0">
          <option v-for="talk in this.talks" v-bind:value="talk" v-bind:key="talk.id.id">
          {{ talk.title }}<span v-if="talk.cospeaker">, with cospeaker {{ talk.cospeaker }}</span>
          </option>
        </select>
      </div>
      <div><input v-model="conference" placeholder="Conference" /></div>
      <div><input v-model="time" placeholder="Time" /></div>
      <div><textarea v-model="notes" placeholder="Notes" /></div>
      <div><button v-on:click="saveSubmission">Save submission</button></div>
    </section>
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
  margin: 0 auto;
}

section {
  display: grid;
}

section.mySubmissions {
  grid-template-columns: 1fr 2fr 3fr 1fr 1fr;
}

section#submission {
  grid-template-columns: 1fr;
  align-self: center;
  justify-self: center;
  margin-bottom: 1em;
}

#submission div {
  align-self: center;
  justify-self: center;
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
div {
  border: 1px solid blue;
}

</style>
