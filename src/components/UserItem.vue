<template>
  <li class="item">#{{ id }} {{ name }}: {{ email }}
    <span class="icon" @click="$emit('RemoveUser', id)">x</span>
  </li>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      default: 'User',
    },
    email: {
      type: String,
      required: true
    },
  },
}
</script>
