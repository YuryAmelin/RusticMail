<template>
  <div class="wrapper">
    <button @click="selectedScreen = 'inbox'"
            :disabled="selectedScreen === 'inbox'">Inbox</button>
    <button @click="selectedScreen = 'archive'"
            :disabled="selectedScreen === 'archive'">Archived</button>

    <BulkActionBar :emails="filteredEmails" />

    <table class="mail-table">
      <tbody>
        <tr v-for="email in filteredEmails"
            :key="email.id"
            :class="['clickable', email.read ? 'read' : '']">
          <td style="padding-left: 0">
          <div class="wood-div">
            <img id="wood" src="../img/wood.jpg" alt="">
          </div>
          </td>
          <td>
            <input type="checkbox"
                   @click="emailSelection.toggle(email)"
                   :checked="emailSelection.emails.has(email)" />
          </td>
          <td @click="openEmail(email)">User{{email.userId}}</td>
          <td @click="openEmail(email)">
            <p><strong>{{email.title}}</strong> - {{email.body}}</p>
          </td>
          <td class="date" @click="openEmail(email)" style="border-right: 6px solid #5D3E24;">
            {{format(new Date(email.sentAt), 'MMM do yyyy')}}
          </td>
            <div class="leaf-div">
              <img  id="leaf" src="../img/leaf.png" alt="">
              <button @click="archiveEmail(email)" class="leaf">{{ myScreen === 'inbox' ? 'Archive' : 'to Inbox' }}</button>
            </div>
        </tr>
      </tbody>
    </table>

    <ModalView v-if="openedEmail" @closeModal="openedEmail = null">
      <MailView :email="openedEmail" @changeEmail="changeEmail" />
    </ModalView>
  </div>
</template>

<script>
  import { format } from 'date-fns';
  import axios from 'axios';
  import MailView from '@/components/MailView.vue';
  import ModalView from '@/components/ModalView.vue';
  import BulkActionBar from '@/components/BulkActionBar.vue';
  import { reactive, ref, provide, computed } from 'vue';
  import useEmailSelection from '@/composables/use-email-selection'

  export default {
    async setup(){
      let selectedScreen = ref('inbox')
      let myScreen = computed(() => {return selectedScreen.value})
      provide('screen', myScreen)

      let {data: emails} = await axios.get('https://jsonplaceholder.typicode.com/posts')
      emails = emails.slice(0, 30)
      emails.forEach(email => {
        email.sentAt = Math.random()*(1633950452445 - 2800000000000)+1600000000000
        email.read = false
      })

      return {
        emailSelection: useEmailSelection(),
        format,
        emails: ref(emails),
        openedEmail: ref(null),
        selectedScreen,
        myScreen
      }
    },
    components: {
      MailView,
      ModalView,
      BulkActionBar,
    },
    computed: {
      sortedEmails() {
        return this.emails.sort((e1, e2) => {
          return e1.sentAt < e2.sentAt ? 1 : -1
        })
      },
      filteredEmails() {
        if(this.selectedScreen === 'inbox') {
          return this.sortedEmails.filter(e => !e.archived)
        } else {
          return this.sortedEmails.filter(e => e.archived)
        }
      }
    },
    methods: {
      selectScreen(newScreen) {
        this.selectedScreen = newScreen
        this.emailSelection.clear()
      },
      openEmail(email) {
        this.openedEmail = email

        if(email) {
          email.read = true
        }
      },
      archiveEmail(email) {
        email.archived = !email.archived
      },
      changeEmail({toggleRead, toggleArchive, closeModal, changeIndex}) {
        let email = this.openedEmail
        if(toggleRead) { email.read = !email.read }

        let emails = this.filteredEmails
        let currentIndex = emails.indexOf(email)
        let newEmail = emails[currentIndex + changeIndex]

        if(toggleArchive) { email.archived = !email.archived }
        if(closeModal) { this.openedEmail = null }
        if(changeIndex) {
          this.openEmail(newEmail)
        }
      },
    }
  }
</script>

<style scoped>

#wood {
  width: 120px;
  height: 80px;
  object-fit: cover;
}

#leaf {
  width: 120px;
  height: 80px;
}

.wood-div {
  width: 120px;
  height: 80px;
}

.leaf-div {
  position: relative;
}

.leaf {
  position: absolute;
  bottom: 20px;
  left: 15px;
  border: none;
  background-color: transparent;
  z-index: 10;
  font-weight: bold;
}

</style>
