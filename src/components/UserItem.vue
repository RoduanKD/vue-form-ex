<template>
  <li class="item">
    #{{ id }} {{ name }}:
    <slot></slot>
    {{ email }}
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
