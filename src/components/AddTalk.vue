<template>
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
          baseurl: 'http://localhost:8080', 
      }
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
    created() {
        var self = this;
        axios.get(self.baseurl + '/talks')
        .then(function (response) {
             self.talks = response.data;
        });
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

  section#submission {
      display: grid;
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
</style>