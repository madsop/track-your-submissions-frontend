<template>
  <div class="hello">
  Talk: <select v-model="selected" v-if="talks.length > 0">
    <option v-for="talk in this.talks" v-bind:value="talk" v-bind:key="talk.id.id">
      {{ talk.title }}<span v-if="talk.cospeaker">, with cospeaker {{ talk.cospeaker }}
    </span>
    </option>
  </select>
  <br />
  <br />
  <input v-model="conference" placeholder="Conference" /> <br />
  <input v-model="time" placeholder="Time" /> <br />
  <textarea v-model="notes" placeholder="Notes" /> <br />
  <button v-on:click="saveSubmission">Save submission</button>
  <hr />
  <br />
  <h2>My submissions:</h2>
  <ul v-for="submission in this.submissions" v-bind:key="submission.id.id">
    <li>{{ submission.time }}, {{ submission.conference }}: {{ submission.talk.title }} ({{ submission.status }}) 
      <span v-if="submission.notes">({{submission.notes}})</span> </li>
  </ul> 
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

<style scoped>

div#selectedTalk {
  margin-top: 20px;
}

input, textarea, button {
  width: 300px;
  margin: 10px;
}

textarea {
  height: 120px;
}

li {
  list-style-type: none;
}
ul {
  padding: 0;
  margin: 1px auto;
}

</style>
