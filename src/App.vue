<template>
  <header>
    <h1 id="name">JS 101</h1>
  </header>

  <section class="container">
    <form @submit.prevent="addToList">
      <h1>Add User</h1>
      <div v-if="message" class="msg" :class="messageClass" v-text="message"></div>
      <div>
        <label for="name">Name:</label>
        <input type="text" name="first_name" v-model.trim="firstName">
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" name="email" v-model.trim="email">
      </div>
      <input class="btn" type="submit" value="Submit">
    </form>
    <div>
      <h3>Preview</h3>
      {{ preview }}
      {{ preview }}
      {{ preview }}
    </div>
    <hr>
    <button @click="removeFirstUser" class="mx-2">Remove First user</button>
    <button @click="emptyUsersList" class="mx-2">Clear users</button>
    <button @click="removeLastUser" class="mx-2">Remove Last user</button>
    <button @click="removeUserAt" class="mx-2">Remove user at</button>
    <input type="number" min="1" :max="usersList.length" placeholder="index" v-model="selectedIndex">
    <button @click="loadUsersFromApi" class="mx-2">Load Users from API</button>

    <input type="search" placeholder="Name" v-model.trim="searchValue">

    <ul class="items">
      <li v-for="(user, i) in filteredList" class="item" :key="user.id">#{{ user.id }} {{ user.name }}: {{ user.email }}
        <span class="icon" @click="removeUser(i)">x</span>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      message: '',
      messageClass: '',

      firstName: '',
      email: '',

      usersList: [],
      selectedIndex: '',
      searchValue: '',
    }
  },

  computed: {
    filteredList() {
      return this.usersList.filter((user) => {
        return user.name.toLowerCase()
          .includes(this.searchValue.toLowerCase())
      })
    },

    preview() {
      return this.firstName + ': ' + this.email
    }
  },

  created() {
    this.loadUsersFromApi()
  },

  methods: {
    addToList() {
      if (this.firstName === '' || this.email === '') {
        this.showMsg('Please enter all fields', true)
        return;
      }

      this.usersList.push({
        id: this.usersList.length + 1,
        name: this.firstName,
        email: this.email,
      })
      this.showMsg('User was Added')


      this.firstName = ''
      this.email = ''
    },

    showMsg(message, isError = false, duration = 3000) {
      if (isError)
        this.messageClass = 'error'
      else
        this.messageClass = 'success'

      this.message = message

      setTimeout(this.removeMsg, duration)
    },

    removeMsg() {
      this.message = ''
      this.messageClass = ''
    },

    emptyUsersList() {
      this.usersList = []
      this.showMsg('Users list was emptied')
    },

    removeLastUser() {
      this.usersList.pop()
    },

    removeFirstUser() {
      this.usersList.shift()
    },

    removeUserAt() {
      const index = this.selectedIndex - 1

      this.usersList.splice(index, 1)
    },

    removeUser(index) {
      this.usersList.splice(index, 1)
    },

    loadUsersFromApi() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(res => res.json())
        .then(users => {
          this.usersList = users
        })
    }
  }
}
</script>
