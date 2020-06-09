<template>
  <div id="app">
    <Header :mode="mode" @endQuiz="showResults" />

    <Setup v-if="mode === 'setup'" v-on:new-quiz="newQuiz" />
    <Quiz v-else-if="mode === 'quiz'" :quiz="quiz" @show-results="showResults" />
    <Results
      v-else-if="mode === 'results'"
      :quiz="quiz"
      :correctAnswers="correctAnswers"
      :unanswered="unanswered"
      :minRange="min"
      :maxRange="max"
      @resetState="resetState"
    />
  </div>
</template>

<script>
import KanjiJSON from './assets/kanji.json';

import Header from './components/layout/Header';
import Setup from './components/Setup';
import Quiz from './components/Quiz';
import Results from './components/Results';

export default {
  name: 'App',
  components: {
    Header,
    Setup,
    Quiz,
    Results
  },
  data() {
    return {
      mode: 'setup',
      quiz: [],
      min: 1,
      max: 2200,
      correctAnswers: 0,
      unanswered: 0
    }
  },
  methods: {
    newQuiz(quizParams) {
      // Ensure state is reset
      this.quiz = [];
      this.min = 1;
      this.max = 2200;
      this.correctAnswers = 0;
      this.unanswered = 0;

      this.min = quizParams["minRange"];
      this.max = quizParams["maxRange"];
      // Build a new quiz object and go to quiz state
      let range = this.max - this.min + 1;
      let rc = quizParams["reviewCount"];

      let loops = Math.floor(rc / range);
      let rem = rc % range;
      let diff = range - rem;
      // Get a random integer from 0 to array length if the array contains at least 2 elements
      const randIndex = (arrLen) => arrLen > 1 ? Math.floor(Math.random() * arrLen) : 0;
      // Generates a question object
      const genQuestion = (entry) => {
        let answers = [];

        for (let i = 0; i < 3; i++) {
          let randNo = Math.floor(Math.random() * 2200);

          if (!answers.includes(KanjiJSON[randNo]["keyword"])) {
            answers.push(KanjiJSON[randNo]["keyword"]);
          } else {
            i--;
          }
        }
        answers.splice(randIndex(answers.length+1), 0, entry["keyword"]);

        const newQuestion = {
          kanji: entry["kanji"],
          correct: entry["keyword"],
          answers: answers,
          chosenAnswer: false
        }
        return newQuestion;
      }
      // Generates arrays of questions (in whole or partial chunks)
      const genKanjiArray = (min, max, diff = false) => {
        let tempArr = [];
        // Generate questions and add them to temp array
        for (let i = min; i <= max; i++) {
          tempArr.splice(randIndex(tempArr.length), 0, genQuestion(KanjiJSON[i-1]));
        }
        // Remove unneeded kanji if a difference is given
        tempArr = diff ? tempArr.splice(diff) : tempArr;
        return tempArr;
      }
      // Add each kanji in range once according to loop count
      if (loops) {
        for (let i = 0; i < loops; i++) {
          this.quiz = [...this.quiz, ...genKanjiArray(this.min, this.max)];
        }
      }
      // Add remaining kanji
      if (rem) {
        this.quiz = [...this.quiz, ...genKanjiArray(this.min, this.max, diff)];
      }
      this.mode = 'quiz';
    },
    showResults() {
      // Calculate number of correct answers
      this.quiz.forEach((question) => {
        if (question["chosenAnswer"] === question["correct"]) {
          this.correctAnswers++;
        } else if (question["chosenAnswer"] === false) {
          this.unanswered++;
        }
      });
      console.log(`Unanswered: ${this.unanswered} - Quiz Length: ${this.quiz.length}`)
      // Return to setup immediately if no questions were attempted
      if (this.unanswered === this.quiz.length) {
        this.mode = 'setup';
      } else {
        this.mode = 'results';
      }
    },
    resetState() {
      this.mode = 'setup';
    }
  }
}
</script>
