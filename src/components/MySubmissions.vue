<template>
  <div class="allSubmissions">
    <section v-for="submission in submissions" :key="submission.title+submission.conference" class="mySubmissions">
      <Submission 
        :id="submission.id"
        :time="submission.time" 
        :conference="submission.conference"
        :status="submission.status"
        :talk="submission.talk"
        :notes="submission.notes"
      />
    </section>
  </div>
</template>

<script lang="ts">

import axios from 'axios';
import Submission from '@/components/Submission.vue';

export default {
  components: {
      Submission,
  },
  data() {
    return {
      submissions: [],
      baseurl: 'http://localhost:8080',
    };
  },
  created() {
    const self = this;
    axios.get(self.baseurl + '/cfp')
    .then((response) => response.data)
    .then((data) => self.submissions = data);
  },
};
</script>