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
        <i class="fa fa-caret-up fa-2x symbol_color" aria-hidden="true" :class="{disabled: note.upvoted}" @click="upvote(index)" />
        <div class="vote_number">
          <span class="label label-primary">{{ note.vote }}</span>
        </div>
        <i class="fa fa-caret-down fa-2x symbol_color" :class="{disabled: note.downvoted}" @click="downvote(index)" />
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
        upvoted: false,
        downvoted: false,
        description: 'If you see this comment, something went wrong :(',
        name: 'Something went wrong :(',
        created_at: new Date(Date.now()).toLocaleString(),
        vote: 3
      },
      {
        upvoted: false,
        downvoted: false,
        description: 'If you see this comment, something went wrong :(',
        name: 'Something went wrong :(',
        created_at: new Date(Date.now()).toLocaleString(),
        vote: 9
      },
      {
        upvoted: false,
        downvoted: false,
        description: 'If you see this comment, something went wrong :(',
        name: 'Something went wrong :(',
        created_at: new Date(Date.now()).toLocaleString(),
        vote: 3
      },
      {
        upvoted: false,
        downvoted: false,
        description: '3 Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.',
        name: 'Beispiel 3',
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
    console.log(navigator)
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
      const res = await this.$axios.get('')
      this.notes = res.data
    },
    async upvote (index) {
      const note = this.notes[index]
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
      await this.$axios.post('', {
        id: note.id,
        vote: '-1',
        'access-key': 'ACH788GHD',
        'user-agent': 'navigator.userAgent'
      })
      this.getnotes()
    }
  }
}
</script>
