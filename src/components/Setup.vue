<template>
  <main class="page-width">
    <form @submit.prevent="newQuiz">
      <label for="range-min">Min Range</label>
      <input type="number" v-model="minRange" name="range-min" min="1" value="1">
  
      <label for="range-max">Max Range</label>
      <input type="number" v-model="maxRange" name="range-max" max="2200" value="2200">
  
      <label for="review-count">Review Count</label>
      <input type="number" v-model="reviewCount" name="review-count" min="10" value="1000">
  
      <input class="btn" type="submit" value="Start Reviews">
    </form>

    <h2 class="page-heading">About Kanji Review</h2>

    <p>
      This tool will allow you to review 2200 kanji, including 
      the jouyou kanji and some meiyou kanji.
    </p>

    <p>
      It makes use of the 
      <a href="https://ankiweb.net/shared/info/1956010956">
        NihongoShark.com Kanji Deck
      </a> with changes to two keywords ('wherefore' is now 
      'therefore' and 'but of course' is now 'still more') and 
      cards with alternative kanji have been reduced to just one for 
      consistency.
    </p>

    <p>
      Select a range within the deck to review and the number of reviews 
      you wish to do. You can select anywhere between 10 and 1000 reviews.
    </p>

    <p>
      Any reviews you get wrong will be present on the results screen at the 
      end of the review session with a link to that kanji's entry on 
      <a href="https://jisho.org/">Jisho.org</a>.
    </p>
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
      if (this.minRange < 1 
        || this.maxRange > 2200 
        || this.reviewCount < 10 
        || this.reviewCount > 1000) return;

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