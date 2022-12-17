<template>
  <li class="item">#{{ id }} {{ name }}: {{ email }}
    <span class="icon" @click="removeUser(i)">x</span>
  </li>
</template>

<script>
export default {
  props: ['id', 'name', 'email'],
}
</script>
