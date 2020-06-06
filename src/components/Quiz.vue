<template>
  <main class="page-width">
    <div class="correct" :class="{'hide':!correct}">Correct</div>
    <div class="incorrect" :class="{'hide':!incorrect}">Incorrect</div>
    <span class="review-remaining">{{ totalQuestions - curIndex }} left</span>

    <h2 id="review-kanji">{{ quiz[curIndex]["kanji"] }}</h2>

    <div id="review-btns">
      <button 
        @click="checkAnswer(answer)"
        v-for="(answer, index) in quiz[curIndex]['answers']"
        :key="index"
        class="btn"
        :disabled="quiz[curIndex]['chosenAnswer'] !== false"
      >
      {{ answer }}
      </button>
    </div>
  </main>
</template>

<script>
export default {
  name: "Quiz",
  data() {
    return {
      curIndex: 0,
      totalQuestions: this.quiz.length,
      correct: false,
      incorrect: false,
      delay: 1500
    }
  },
  props: ["quiz"],
  methods: {
    checkAnswer(answer) {
      this.quiz[this.curIndex]["chosenAnswer"] = answer;

      // Show correct/incorrect answer splash on top-bar
      if (answer === this.quiz[this.curIndex]["correct"]) {
        this.correct = true;
      } else {
        this.incorrect = true;
      }
      if (this.curIndex + 1 < this.totalQuestions) {
        setTimeout(() => {
          this.curIndex++;
          // Hide top-bar splash
          this.correct = false;
          this.incorrect = false;
        }, this.delay);
      } else {
        // End quiz here, show results
        setTimeout(() => {
          this.correct = false;
          this.incorrect = false;

          this.$emit('show-results');
        }, this.delay);
      }
    }
  }
}
</script>