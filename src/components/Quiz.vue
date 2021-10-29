<template>
  <div class="quiz-group">
    <div class="wrapper-quiz" v-for="question in quiz" :key="question.id">
      <div
        class="quiz-question"
        v-if="question.id == this.question && this.loading != true"
      >
        <div class="quiz-head" v-if="question.id != 1 && this.userAge == null">
          <p>
            {{ question.quote }}
          </p>
          <img class="quiz-head__eye" src="../assets/eye.svg" alt="" />
          <hr />
        </div>
        <div class="quiz-head__alternative" v-if="userAge != null">
          <p>
            {{ question.quote }}
          </p>
          <img class="quiz-head__eye" src="../assets/eye.svg" alt="" />
          <hr />
        </div>
        <div class="quiz">
          <h3>{{ question.question }}</h3>
          <div class="starting-quiz" v-if="question.id == 1">
            <button @click="startQuiz">Да <span></span></button>
            <button @click="startQuiz">Нет <span></span></button>
          </div>
          <div v-else class="next-quiz">
            <button
              @click="nextQuestion"
              v-for="answer in question.answers"
              :key="answer"
            >
              {{ answer }}<span></span>
            </button>
          </div>

          <form v-if="question.form" v-on:submit.prevent="handleSubmit">
            <div class="quiz-select">
              <select
                v-model="day"
                :class="{ invalid: isSubmitting && !day.trim() }"
              >
                <option value="" default disabled selected>День</option>
                <option v-for="day in days" v-bind:key="day">{{ day }}</option>
              </select>
              <select
                v-model="month"
                :class="{ invalid: isSubmitting && !month.trim() }"
              >
                <option value="" default disabled selected>Месяц</option>
                <option v-for="month in months" v-bind:key="month">{{
                  month
                }}</option>
              </select>
              <select
                v-model="year"
                :class="{ invalid: isSubmitting && !year.trim() }"
              >
                <option value="" default disabled selected>Год</option>
                <option v-for="year in years" v-bind:key="year">{{
                  year
                }}</option>
              </select>
            </div>
            <button v-if="question.form" type="submit">
              Далее
              <span></span>
            </button>
          </form>

          <img
            v-if="question.id != 1"
            class="quiz__moon"
            src="../assets/moon.svg"
            alt=""
          />
          <span class="quiz__question-counter"
            >Вопрос {{ realQuestion }}-5</span
          >
        </div>
      </div>
    </div>
    <div class="loading" v-if="loading">
      <loadingSpinner></loadingSpinner>
      <span>Loading</span>
    </div>
    <div class="mic" v-if="showMic">
      <loadingMic @send="receive"></loadingMic>
    </div>
    <div class="quiz-end" v-if="showEndQuiz">
      <quizEnd></quizEnd>
    </div>
  </div>
</template>

<script>
import loadingSpinner from './LoadingSpinner.vue'
import loadingMic from './LoadingMic.vue'
import quizEnd from './QuizEnd.vue'
export default {
  components: {
    loadingSpinner,
    loadingMic,
    quizEnd
  },
  data () {
    return {
      question: 1,
      realQuestion: 1,
      quiz: [
        {
          id: 1,
          quote: null,
          question: 'Боитесь ли вы умереть?',
          answers: ['Да', 'Нет'],
          form: false,
          ageRange: null
        },
        {
          id: 2,
          quote:
            'Мы расскажем Вам не только подробности вашей смерти, но также поможем Вам избежать этой ужасной даты и продлить вашу жизнь на многие годы.',
          question: 'Когда Вы чувствуете себя наиболее комфортно?',
          answers: ['Утро', 'День', 'Вечер', 'Ночь'],
          form: false,
          ageRange: null
        },
        {
          id: 3,
          quote:
            'Уже совсем скоро Вы узнаете много интересного о своем будущем!',
          question: 'Укажите свою дату рождения:',
          answers: null,
          form: true,
          ageRange: null
        },
        {
          id: 4,
          quote:
            'Смерть родного человека – одно из тяжелейших испытаний в жизни каждого из нас!',
          question: 'Снятся ли вам умершие люди?',
          answers: ['Да', 'Нет', 'Иногда'],
          form: false,
          ageRange: null
        },
        {
          id: 5,
          quote:
            'По вам скучает очень близкий человек, которого больше нет в мире живых.',
          question:
            'Запись, которую Вы услышите, может шокировать людей с неокрепшей психикой. Вы готовы узнать, что ждет именно Вас?',
          answers: ['Да', 'Затрудняюсь ответить'],
          form: false,
          ageRange: { min: 18, max: 35 }
        },
        {
          id: 6,
          quote:
            'По вам скучает очень близкий человек, которого больше нет в мире живых. Возможно это дедушка или бабушка.',
          question:
            'Запись, которую Вы услышите, может шокировать людей с неокрепшей психикой. Вы готовы узнать, что ждет именно Вас?',
          answers: ['Да', 'Затрудняюсь ответить'],
          form: false,
          ageRange: { min: 36, max: 45 }
        },
        {
          id: 7,
          quote:
            'По вам скучает очень близкий человек, которого больше нет в мире живых. Возможно это кто-то из Ваших родителей.',
          question:
            'Запись, которую Вы услышите, может шокировать людей с неокрепшей психикой. Вы готовы узнать, что ждет именно Вас?',
          answers: ['Да', 'Затрудняюсь ответить'],
          form: false,
          ageRange: { min: 46, max: 120 }
        }
      ],
      day: '',
      month: '',
      year: '',
      days: [...Array(32).keys()].slice(1),
      months: [
        'Январь',
        'Февраль',
        'Март',
        'Апрель',
        'Май',
        'Июнь',
        'Июль',
        'Август',
        'Сентябрь',
        'Октябрь',
        'Ноябрь',
        'Декабрь'
      ],
      years: [],
      isSubmitting: false,
      userAge: null,
      loading: false,
      showMic: false,
      showEndQuiz: false
    }
  },
  methods: {
    startQuiz () {
      this.$emit('send', true)
      this.question++
    },
    createYears () {
      const startYear = 1950
      const endYear = new Date().getFullYear()

      for (let i = startYear; i <= endYear; i++) {
        this.years.push(i)
      }
    },
    handleSubmit () {
      this.isSubmitting = true

      if (this.day && this.month && this.year) {
        const currentYear = new Date().getFullYear()
        this.userAge = currentYear - this.year
        this.loading = true
      }
    },
    nextQuestion () {
      this.question++
      if (this.userAge != null) {
        this.showMic = true
      }
    },
    receive (bool) {
      this.showMic = bool
      this.showEndQuiz = true
    }
  },
  props: {
    quizStart: {
      type: Boolean
    }
  },
  created () {
    if (!this.year[0]) {
      this.createYears()
    }
  },
  watch: {
    loading: function (bool) {
      // consider change to somthing more flexible
      if (bool === true) {
        setTimeout(() => {
          this.loading = false
          this.question++
        }, 2000)
      }
    },
    userAge: function (age) {
      this.quiz.forEach(question => {
        if (age >= question?.ageRange?.min && age <= question?.ageRange?.max) {
          this.question = question.id - 1
        }
      })
    },
    showMic: function (bool) {},
    question: function (num) {
      if (this.question === 5 || this.question === 6 || this.question === 7) {
        this.realQuestion = 5
      } else {
        this.realQuestion = this.question
      }
    }
  }
}
</script>

<style scoped>
.quiz-group {
  display: flex;
  flex-direction: column;
}

.quiz-question {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.quiz-head {
  position: relative;
  height: 200px;
  width: 1440px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.quiz-head__alternative {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  row-gap: 40px;
}

.quiz-head__alternative p::after {
  content: '';
  position: absolute;
  right: 18px;
  bottom: -9px;
  width: 0;
  height: 0;
  border-top: 18px solid #fff;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  z-index: -1;
}

.quiz-head__alternative p {
  font-size: 25px;
  font-style: normal;
  font-weight: 700;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  font-family: Bad Script;
  margin-top: 40px;
  width: 480px;
  height: 164px;
  background-color: #fff;
  border-radius: 5px;
  padding: 0 12px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  animation: 1s fadeIn linear;
  position: relative;
}

.quiz-head__alternative hr {
  border: 1px solid #ffffff33;
  width: 1440px;
}

.quiz-head__alternative .quiz-head__eye {
  width: 140px;
  position: absolute;
  left: 800px;
  top: 140px;
  opacity: 0.2;
}

@keyframes fadeIn {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}

.quiz-head p {
  font-family: Bad Script;
  font-size: 25px;
  font-style: normal;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  width: 430px;
  color: #fff;
  opacity: 0.6;
  margin-top: 40px;
  margin-bottom: 40px;
}

.quiz-head hr {
  border: 1px solid #ffffff33;
  width: 1440px;
  z-index: 3;
}

.quiz-head__eye {
  width: 140px;
  position: absolute;
  right: 400px;
  top: 80px;
  opacity: 0.2;
}

.quiz {
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.quiz h3 {
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  margin-bottom: 50px;
  color: #f6c866;
  text-transform: uppercase;
  width: 500px;
}

.quiz span {
  color: #ffffff99;
  opacity: 0.5;
}

.quiz .next-quiz {
  display: flex;
  flex-direction: column;
}

.starting-quiz {
  display: flex;
  flex-direction: column;
}

.quiz__moon {
  margin-top: 20px;
  margin-bottom: 15px;
  margin-right: 440px;
  width: 80px;
  opacity: 0.2;
}

button {
  width: 315.95px;
  height: 69.44px;
  font-weight: normal;
  font-size: 20px;
  line-height: 23px;
  border-radius: 50px;
  color: #ffffff;
  cursor: pointer;
  margin-bottom: 34.72px;
  background: linear-gradient(
    90deg,
    rgba(246, 200, 102, 0.9) -6.2%,
    rgba(254, 173, 53, 0.9) 100%
  );
  position: relative;
  outline: none;
  border: none;
}

@keyframes slidein {
  0% {
    left: 0%;

    opacity: 0;
  }

  25% {
    opacity: 1;
  }

  70% {
    left: 35%;
    opacity: 1;
  }

  100% {
    left: 50%;
    opacity: 0;
  }
}
button > span {
  background: linear-gradient(
    248.67deg,
    rgba(255, 255, 255, 0) 30.84%,
    rgba(255, 255, 255, 0.29) 46.06%,
    rgba(255, 255, 255, 0) 64.04%
  );
  width: 157px;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  animation: 1.5s infinite slidein linear;
}

/* third question css */

.quiz-select {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.quiz-select select {
  width: 315.95px;
  height: 69.44px;
  font-weight: normal;
  font-size: 20px;
  line-height: 23px;
  border-radius: 50px;
  cursor: pointer;
  margin-bottom: 34.72px;
  background: url('../assets/selectArrow.svg') no-repeat bottom 28px right 20px/8%,
    linear-gradient(
      90deg,
      rgba(228, 228, 228, 0.9) -6.2%,
      rgba(203, 203, 203, 0.9) 100%
    );
  box-shadow: inset 0px 4px 4px rgba(0, 0, 0, 0.25);
  outline: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  position: relative;
}

.quiz-select select:first-child {
  padding-left: 32%;
}

.quiz-select select:nth-child(2) {
  padding-left: 29%;
}

.quiz-select select:nth-child(3) {
  padding-left: 32%;
}

.quiz-select option[default] {
  display: none;
}

.invalid {
  border: 1px solid #ee5353;
}

.quiz__question-counter {
  font-size: 20px;
}

@media only screen and (max-width: 320px) {
  .quiz-group {
    display: flex;
    flex-direction: column;
  }

  .quiz-question {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .quiz-head {
    position: relative;
    height: 26.667vw;
    width: 192vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .quiz-head__alternative {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    row-gap: 5.333vw;
  }

  .quiz-head__alternative p::after {
    content: '';
    position: absolute;
    right: 2.4vw;
    bottom: -1.2vw;
    width: 0;
    height: 0;
    border-top: 2.4vw solid #fff;
    border-left: 1.333vw solid transparent;
    border-right: 1.333vw solid transparent;
    z-index: -1;
  }

  .quiz-head__alternative p {
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 700;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    font-family: Bad Script;
    margin-top: 5.333vw;
    width: 64vw;
    height: 21.867vw;
    background-color: #fff;
    border-radius: 0.667vw;
    padding: 0 1.6vw 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    animation: 1s fadeIn linear;
    position: relative;
  }

  .quiz-head__alternative hr {
    border: 0.133vw solid #ffffff33;
    width: 192vw;
  }

  .quiz-head__alternative .quiz-head__eye {
    width: 18.667vw;
    position: absolute;
    left: 106.667vw;
    top: 18.667vw;
    opacity: 0.2;
  }

  @keyframes fadeIn {
    0% {
      opacity: 0;
      transform: scale(0.9);
    }
    100% {
      opacity: 1;
      transform: scale(1);
    }
  }

  .quiz-head p {
    font-family: Bad Script;
    font-size: 3.333vw;
    font-style: normal;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    width: 57.333vw;
    color: #fff;
    opacity: 0.6;
    margin-top: 5.333vw;
    margin-bottom: 5.333vw;
  }

  .quiz-head hr {
    border: 0.133vw solid #ffffff33;
    width: 192vw;
    z-index: 3;
  }

  .quiz-head__eye {
    width: 18.667vw;
    position: absolute;
    right: 53.333vw;
    top: 10.667vw;
    opacity: 0.2;
  }

  .quiz {
    margin-top: 8vw;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .quiz h3 {
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    margin-bottom: 6.667vw;
    color: #f6c866;
    text-transform: uppercase;
    width: 66.667vw;
  }

  .quiz span {
    color: #ffffff99;
    opacity: 0.5;
  }

  .quiz .next-quiz {
    display: flex;
    flex-direction: column;
  }

  .starting-quiz {
    display: flex;
    flex-direction: column;
  }

  .quiz__moon {
    margin-top: 2.667vw;
    margin-bottom: 2vw;
    margin-right: 58.667vw;
    width: 10.667vw;
    opacity: 0.2;
  }

  button {
    width: 42.127vw;
    height: 9.259vw;
    font-weight: normal;
    font-size: 2.667vw;
    line-height: 3.067vw;
    border-radius: 6.667vw;
    color: #ffffff;
    cursor: pointer;
    margin-bottom: 4.629vw;
    background: linear-gradient(
      90deg,
      rgba(246, 200, 102, 0.9) -6.2%,
      rgba(254, 173, 53, 0.9) 100%
    );
    position: relative;
    outline: none;
    border: none;
  }

  @keyframes slidein {
    0% {
      left: 0%;

      opacity: 0;
    }

    25% {
      opacity: 1;
    }

    70% {
      left: 35%;
      opacity: 1;
    }

    100% {
      left: 50%;
      opacity: 0;
    }
  }
  button > span {
    background: linear-gradient(
      248.67deg,
      rgba(255, 255, 255, 0) 30.84%,
      rgba(255, 255, 255, 0.29) 46.06%,
      rgba(255, 255, 255, 0) 64.04%
    );
    width: 20.933vw;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    animation: 1.5s infinite slidein linear;
  }

  /* third question css */

  .quiz-select {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .quiz-select select {
    width: 42.127vw;
    height: 9.259vw;
    font-weight: normal;
    font-size: 2.667vw;
    line-height: 3.067vw;
    border-radius: 6.667vw;
    cursor: pointer;
    margin-bottom: 4.629vw;
    background: url('../assets/selectArrow.svg') no-repeat bottom 3.733vw right
        2.667vw/8%,
      linear-gradient(
        90deg,
        rgba(228, 228, 228, 0.9) -6.2%,
        rgba(203, 203, 203, 0.9) 100%
      );
    box-shadow: inset 0vw 0.533vw 0.533vw rgba(0, 0, 0, 0.25);
    outline: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    appearance: none;
    position: relative;
  }

  .quiz-select select:first-child {
    padding-left: 32%;
  }

  .quiz-select select:nth-child(2) {
    padding-left: 29%;
  }

  .quiz-select select:nth-child(3) {
    padding-left: 32%;
  }

  .quiz-select option[default] {
    display: none;
  }

  .invalid {
    border: 0.133vw solid #ee5353;
  }
  .quiz__question-counter {
    font-size: 12px;
  }
}
</style>
