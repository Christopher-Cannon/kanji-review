<template>
  <main class="page-width">
    <p>
      This tool will allow you to review 2200 kanji, including 
      the jouyou kanji and some meiyou kanji.
    </p>
  
    <form @submit.prevent="newQuiz">
      <label for="range-min">Min Range</label>
      <input type="number" v-model="minRange" name="range-min" min="1" value="1">
  
      <label for="range-max">Max Range</label>
      <input type="number" v-model="maxRange" name="range-max" max="2200" value="2200">
  
      <label for="review-count">Review Count</label>
      <input type="number" v-model="reviewCount" name="review-count" min="10" value="1000">
  
      <input class="btn" type="submit" value="Start Reviews">
    </form>
  </main>
</template>

<script>
export default {
  name: "Setup",
  data() {
    return {
      minRange: 1,
      maxRange: 2200,
      reviewCount: 10
    }
  },
  methods: {
    newQuiz() {
      const quizParams = {
        minRange: Number(this.minRange),
        maxRange: Number(this.maxRange),
        reviewCount: Number(this.reviewCount)
      };
      // Reset form after object is built
      this.minRange = 1;
      this.maxRange = 2200;
      this.reviewCount = 10;

      this.$emit('new-quiz', quizParams);
    }
  }
}
</script>