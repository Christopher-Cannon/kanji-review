<template>
  <main class="page-width" id="results">
    <h2 class="page-heading">Results</h2>

    <p>Correct Reviews: {{ correctAnswers }}/{{ quiz.length - unanswered }}<br>Range: {{ minRange }}-{{ maxRange }}</p>

    <div v-if="correctAnswers !== quiz.length">
      <h3>Incorrect Answers</h3>

      <div v-for="(question, index) in quiz" :key="index">
        <ResultCard
          v-if="question['chosenAnswer'] !== question['correct'] && question['chosenAnswer'] !== false"
          :question="question"
          :index="index"
        />
      </div>
    </div>

    <div v-else>
      <h3>Full marks!</h3>
    </div>

    <button class="btn btn-return" @click="toSetup">Return to Setup</button>
  </main>
</template>

<script>
import ResultCard from './ResultCard';

export default {
  name: "Results",
  components: {
    ResultCard
  },
  props: ["quiz", "correctAnswers", "unanswered", "minRange", "maxRange"],
  methods: {
    toSetup() {
      this.$emit('resetState');
    }
  }
}
</script>