<template>
  <button v-on:click="addLike">&#9825; {{ likesCounter }}</button>
</template>

<script>
export default {
  name: "LikeButton",
  props: [
    'likes'
  ],
  data() {
    return {
      likesCounter: this.likes,
    }
  },
  methods: {
    addLike() {
      this.$emit('add-like', ++this.likesCounter);
    }
  }
}
</script>

<style scoped>

</style>