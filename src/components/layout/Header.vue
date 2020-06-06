<template>
  <header id="top-bar" class="group">
    <div class="page-width">
      <h1>漢字復習</h1>

      <button class="btn" v-if="mode === 'quiz'" @click="endQuiz">End</button>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  props: ['mode'],
  methods: {
    endQuiz() {
      this.$emit('endQuiz');
    }
  }
}
</script>