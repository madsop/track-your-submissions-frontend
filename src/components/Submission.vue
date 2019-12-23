<template>
    <section class="submission">
        <div id="time">{{time }}</div>
        <div id="conference">{{ conference }}</div>
        <div id="title">{{talk.title}}<span v-if="talk.cospeaker">, with cospeaker {{ talk.cospeaker }}</span></div>
        <div id="status">{{status}}</div>
        <div v-if="notes" id="notes">{{ notes }}</div>
    </section>
</template>

<script lang="ts">
import Vue from 'vue';
import { Prop } from 'vue-property-decorator';
import Component from 'vue-class-component';

@Component
export default class Submission extends Vue {
    @Prop() private conference!: string;
    @Prop() private time!: string;
    @Prop() private notes!: string;
    @Prop() private id!: any;
    @Prop() private talk!: any;
    @Prop() private status!: any;

    public setConference(conference: string) {
        this.conference = conference;
    }
    public setId(id: any) {
        this.id = id;
    }
    public setTalk(talk: any) {
        this.talk = talk;
    }
    public setTime(time: string) {
        this.time = time;
    }
    public setNotes(notes: string) {
        this.notes = notes;
    }
    public setStatus(status: any) {
        this.status = status;
    }
}
</script>

<style lang="scss" scoped>
    div#notes {
        margin-left: 1rem;
    }
    .submission {
        display: grid;
        grid-template-columns: 1fr 2fr 3fr 1fr 1fr;
        width: 75rem;
        margin-top: 0.5rem;
        text-align: left;
    }
</style>