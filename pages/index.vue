<template>
  <div id="app">
    <h2>{{ title }}</h2>
    <br>
    <strong>Name der App</strong>
    <br class="form_titles">
    <br class="form_titles">
    <input v-model="note.name" type="text">
    <br>
    <strong>Beschreibung</strong>
    <br class="form_titles">
    <br class="form_titles">
    <textarea v-model="note.description" />
    <br>
    <button class="button" @click="addNote">
      Senden
    </button>
    <br>
    <div v-for="(note, index) in notesOrdered" :key="index" class="loop_for ">
      <div class="top">
        <i>{{ note.created_at | formatDate }}</i>
      </div>
      <h3>{{ note.name | capitalize }}</h3>
      <br>
      <p>{{ note.description }}</p>
      <li class="list-group-item">
        <i v-if="upvoted" class="fa fa-caret-up fa-2x symbol_color" aria-hidden="true" :class="{up: note['user-vote'] === '+1'}" @click="novote(index)" />
        <i v-else class="fa fa-caret-up fa-2x symbol_color" aria-hidden="true" :class="{up: note['user-vote'] === '+1'}" @click="upvote(index)" />
        <div class="vote_number">
          <span class="label label-primary">{{ note.vote }}</span>
        </div>
        <i v-if="downvoted" class="fa fa-caret-down fa-2x symbol_color" :class="{down: note['user-vote'] === '-1'}" @click="novote(index)" />
        <i v-else class="fa fa-caret-down fa-2x symbol_color" :class="{down: note['user-vote'] === '-1'}" @click="downvote(index)" />
      </li>
    </div>
    <br>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  filters: {
    formatDate (value) {
      if (value) {
        return moment(String(value)).format('DD.MM.YYYY hh:mm')
      }
    },
    capitalize (value) {
      if (!value) { return '' }
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },
  data () {
    return {
      title: 'App Ideas',
      note: {
        upvoted: false,
        downvoted: false,
        name: '',
        description: '',
        created_at: '',
        vote: 0
      },
      notes: [{
        description: 'If you see this comment, something went wrong :(',
        name: 'Please try reloading the Page',
        created_at: new Date(Date.now()).toLocaleString(),
        vote: 3
      }
      ]

    }
  },
  computed: {
    notesOrdered () {
      return this.notes.sort(function (a, b) {
        return b.vote - a.vote
      })
    }
  },

  mounted () {
    this.getnotes()
  },

  methods: {
    async addNote () {
      const {
        name,
        description
      } = this.note
      await this.$axios.post('', {
        name,
        description,
        'access-key': 'ACH788GHD'
      })
      this.getnotes()
      this.note.description = ''
      this.note.name = ''
    },
    async getnotes () {
      const res = await this.$axios.post('', {
        'access-key': 'ACH788GHD',
        'user-agent': 'navigator.userAgent'
      })
      this.notes = res.data
    },
    async upvote (index) {
      const note = this.notes[index]
      this.upvoted = true
      this.downvoted = false
      await this.$axios.post('', {
        id: note.id,
        vote: '+1',
        'access-key': 'ACH788GHD',
        'user-agent': 'navigator.userAgent'
      })
      this.getnotes()
    },
    async downvote (index) {
      const note = this.notes[index]
      this.upvoted = false
      this.downvoted = true
      await this.$axios.post('', {
        id: note.id,
        vote: '-1',
        'access-key': 'ACH788GHD',
        'user-agent': 'navigator.userAgent'
      })
      this.getnotes()
    },
    async novote (index) {
      const note = this.notes[index]
      this.upvoted = false
      this.downvoted = false
      await this.$axios.post('', {
        id: note.id,
        vote: '0',
        'access-key': 'ACH788GHD',
        'user-agent': 'navigator.userAgent'
      })
      this.getnotes()
    }
  }
}
</script>
