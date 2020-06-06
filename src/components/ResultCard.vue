<template>
  <div class="result-card">
    <span class="result-card-id">{{ index + 1 }}</span>

    <span class="result-card-kanji">{{ question["kanji"] }}</span>

    <p>Your answer: <span>{{ question["chosenAnswer"] }}</span></p>

    <p>Correct answer: <span>{{ question["correct"] }}</span></p>

    <a target="_blank" :href="'https://jisho.org/search/' + question['kanji']">View on Jisho...</a>
  </div>
</template>

<script>
export default {
  name: "ResultCard",
  props: ["question", "index"]
}
</script>