<template>
  <div class="email-display">
    <div>
      <button @click="toggleArchive">{{email.archived ? 'Move to Inbox (e)' : 'Archive (e)'}}</button>
      <button @click="toggleRead">{{email.read ? 'Mark Unread (r)' : 'Mark Read (r)'}}</button>
      <button @click="goNewer">Newer (k)</button>
      <button @click="goOlder">Older (j)</button>
      <button @click="goNewerAndArchive">{{email.archived ? 'NewerAndUnarchive ([)' : 'NewerAndArchive ([)'}}</button>
      <button @click="goOlderAndArchive">{{email.archived ? 'OlderAndUnarchive (])' : 'OlderAndArchive (])'}}</button>
    </div>
    <h2 class="mb-0">Subject: <strong>{{email.title}}</strong></h2>
    <div><em>From user{{email.userId}} on {{format(new Date(email.sentAt), 'MMM do yyyy')}}</em></div>
    <div class="text" v-html="marked(email.body)" />
  </div>
</template>

<script>
  import  { format } from 'date-fns'
  import marked from 'marked'
  import useKeydown from '../composables/use-keydown'

  export default {
    setup(props, {emit}){
      let email = props.email;
      let toggleRead = () => { emit('changeEmail', {toggleRead: true})}
      let toggleArchive = () => { emit('changeEmail', {toggleArchive: true, closeModal: true})}
      let goNewer = () => { emit('changeEmail', {changeIndex: -1})}
      let goOlder = () => { emit('changeEmail', {changeIndex: 1})}
      let goNewerAndArchive = () => { emit('changeEmail', {changeIndex: -1, toggleArchive: true})}
      let goOlderAndArchive = () => { emit('changeEmail', {changeIndex: 1, toggleArchive: true})}

      useKeydown([
        {key: 'r', fn: toggleRead},
        {key: 'e', fn: toggleArchive},
        {key: 'k', fn: goNewer},
        {key: 'j', fn: goOlder},
        {key: '[', fn: goNewerAndArchive},
        {key: ']', fn: goOlderAndArchive},
      ])

      return {
        format,
        marked,
        toggleRead,
        toggleArchive,
        goNewer,
        goOlder,
        goNewerAndArchive,
        goOlderAndArchive,
      }
    },
    props: {
      email: {
        type: Object,
        required: true
      }
    }
  }
</script>

<style scoped>

.email-display {
  font-size: 2em;
  color: snow;
}

h2, em, .text {
  text-shadow: black 0 0 4px,   black 0 0 4px,   black 0 0 4px,
  black 0 0 4px,   black 0 0 4px,   black 0 0 4px;
}

</style>
