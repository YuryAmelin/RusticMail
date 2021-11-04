<template>
  <div class="header">
    <h1>rusticMail Inbox</h1>
    <img src="./img/log.png" alt="">
  </div>

  <Suspense>
    <template #default>
      <MailTable />
    </template>
    <template #fallback>
      Loading...
    </template>
  </Suspense>
</template>

<script>
import MailTable from '@/components/MailTable.vue';
import useEmailSelection from '@/composables/use-email-selection';

export default {
  name: 'App',
  components: {
    MailTable
  },
  setup(){
    return {
      emailSelection: useEmailSelection()
    }
  }
};
</script>

<style>

h1 {
  font-size: 3em;
  color: black;
}

.header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin-bottom: 25px;
}

img {
  width: 150px;
  height: 150px;
}

body {
  background-image: url("./img/tree.png");
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  font-size: 20px;
  padding: 8px;
  border-radius: 5px;
  margin: 5px 10px 5px 0px;
  cursor: pointer;
}

button:disabled {
  cursor: auto;
}

button.selected {
  cursor: auto;
  color: black;
  border-color: black;
  border-width: 2px;
}

.clickable {
  cursor: pointer;
}

input[type='checkbox'] {
  -webkit-appearance:none;
  cursor: pointer;
  width:24px;
  height:24px;
  background:white;
  border-radius: 8px;
  border: 3px solid saddlebrown;
  position: relative;
  vertical-align: middle;
  padding: 10px;
}

input[type='checkbox'].partial-check {
  background: yellowgreen;
}

input[type='checkbox']:checked {
  background: orange;
}

.mb-0 {
  margin-bottom: 0;
}

/* Modal */

.modal, .overlay {
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 20;
}
.overlay {
  opacity: 0.5;
  background-color: gray;
}
.modal-card {
  position: relative;
  max-width: 60%;
  margin: auto;
  margin-top: 30px;
  padding: 20px;
  background-color: white;
  min-height: 500px;
  z-index: 20;
  opacity: 1;
}

/* Email Modal */

.email-display {
  text-align: left;
}

/* Mail Table */

.mail-table {
  max-width: 1200px;
  margin: auto;
  border-collapse: collapse;
}
.mail-table tr.read td {
  background-color: white;
}

.mail-table tr {
  font-size: 20px;
}
.mail-table td {
  background-color: blanchedalmond;
  border-bottom: 6px solid #5D3E24;
  padding: 0 5px;
  text-align: left;
}

.mail-table tr:first-of-type div {
  border-top: 7px solid #5D3E24;
}

.mail-table tr:first-of-type div.leaf-div {
  border-top: 4px solid transparent;
}

.mail-table td p {
  max-height: 1.2em;
  overflow-y: hidden;
  margin: 0;
}

.mail-table td.date {
  width: 150px;
}

/* Bulk Action Bar */

.bulk-action-bar {
  width: 100%;
  max-width: 1000px;
  margin: auto;
  text-align: left;
  padding-bottom: 8px;
}

.bulk-action-bar input {
  margin: 5px;
}

.bulk-action-bar .checkbox {
  margin-right: 6px;
  margin-left: 3px;
}
</style>
