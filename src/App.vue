<template>
  <div>
    <ScoreBoard :winCount="winCount" :loseCount="loseCount" />

    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <template v-for="(answer, index) in this.answers" v-bind:key="index">
        <input
          :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosen_answer"
        />
        <label v-html="answer"></label><br />
      </template>
      <button
        v-if="!this.answerSubmitted"
        @click="submitAnswer()"
        class="send"
        type="button"
      >
        Send
      </button>

      <section v-if="this.answerSubmitted" class="result">
        <h4
          v-if="this.chosen_answer == this.correctAnswer"
          v-html="
            '&#9989; Congratulations, the answer ' +
            this.correctAnswer +
            ' is correct!'
          "
        ></h4>
        <h4
          v-else
          v-html="
            '&#10060; I`m sorry, you picked the wrong answer. The correct is ' +
            this.correctAnswer +
            '.'
          "
        ></h4>
        <button @click="this.getNewQuestion()" class="send" type="button">
          Next question
        </button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from "./components/ScoreBoard.vue";

export default {
  name: "App",
  data() {
    return {
      question: undefined,
      correctAnswer: undefined,
      incorrectAnswers: undefined,
      chosen_answer: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0,
    };
  },
  components: { ScoreBoard },

  computed: {
    answers() {
      let answers = [...this.incorrectAnswers];
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswer
      );
      return answers;
    },
  },

  methods: {
    submitAnswer() {
      if (!this.chosen_answer) {
        alert("Pick one of the options!");
      } else if (this.chosen_answer == this.correctAnswer) {
        this.winCount++;
        this.answerSubmitted = true;
      } else {
        this.loseCount++;
        this.answerSubmitted = true;
      }
    },
    getNewQuestion() {
      this.chosen_answer = undefined;
      this.answerSubmitted = false;
      this.question = undefined;

      this.axios
        .get("https://opentdb.com/api.php?amount=1&category=15")
        .then((response) => {
          this.question = response.data.results[0].question;
          this.correctAnswer = response.data.results[0].correct_answer;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
        });
    },
  },
  created() {
    this.getNewQuestion();
  },
  //https://opentdb.com/api.php?amount=1&category=15
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
}

input[type="radio"] {
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}
</style>
