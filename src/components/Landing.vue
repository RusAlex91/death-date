<template>
  <div class="landing-page-wrapper">
    <div class="landing-page">
      <section v-if="!quizStart" class="intro">
        <!-- <img class="image-at-back" src="../assets/blik.svg" alt="" /> -->
        <div class="pseudo-header">
          <h3>Лучшие астрологи и экстрасенсы Румынии</h3>
          <hr />
          <span>Точность прогноза: 97%</span>
        </div>
        <div class="intro-images">
          <div class="images-at-front">
            <img
              class="images-at-front__pray"
              src="../assets/pray.png"
              alt=""
            />
            <img
              class="images-at-front__sigil"
              src="../assets/sigil.svg"
              alt=""
            />
          </div>
        </div>
        <div class="landing-page__starting-quiz">
          <div class="question">
            <p>
              Вас беспокоит вопрос о том,
            </p>
            <p>
              когда Вы покинете этот Мир и при <br />
              каких обстоятельствах?
            </p>
          </div>
          <button @click="handleFirstClick">
            Да
            <span class=""></span>
          </button>
          <button @click="handleFirstClick">
            Нет
            <span class=""></span>
          </button>
          <span>Онлайн предсказание</span>
        </div>
      </section>

      <section
        v-if="!quizStart"
        class="landing-page__proposal"
        ref="date"
        :class="{ visible: isDateMessageVisible }"
      >
        <img src="../assets/hands.png" alt="" />
        <p>
          Позвольте нам раскрыть эту волнующую тайну и
          <span> с точностью определить дату и время вашей смерти,</span> а
          также предшествующую этому событию причину
        </p>
      </section>
      <section
        v-if="!quizStart"
        class="landing-page__quote"
        :class="{ visible: isWomanImageVisible }"
      >
        <p>
          Многие не верят предсказаниям и мы решили доказать каждому,<br />
          <span>что прогноз может изменить жизнь любого человека!</span>
        </p>
      </section>
      <section class="landing-page__main-quiz">
        <Quiz @send="receive" :quizStart="this.quizStart" />
      </section>
      <section v-if="!quizStart" class="quiz-quote">
        <img class="quiz-quote__rune1" src="../assets/rune1.svg" alt="" />
        <p class="quote">
          Вы, конечно, умрете. <br />
          И все, с кем вы знакомы, <br />
          тоже однажды умрут.
        </p>
        <img class="quiz-quote__rune2" src="../assets/rune2.svg" alt="" />
        <img class="quiz-quote__moon" src="../assets/moon.svg" alt="" />
        <img class="quiz-quote__eye" src="../assets/eye.svg" alt="" />
      </section>
    </div>
    <div class="pseudo-footer" v-if="quizStart == false">
      <p>
        TERMENI SI CONDITII: ACESTA ESTE UN SERVICIU DE DIVERTISMENT. PRIN
        FOLOSIREA LUI DECLARATI CA AVETI 18 ANI IMPLINITI,
      </p>
    </div>
  </div>
</template>

<script>
import Quiz from './Quiz.vue'
export default {
  data () {
    return {
      quizStart: false,
      isDateMessageVisible: false,
      isWomanImageVisible: false
    }
  },
  components: {
    Quiz
  },
  methods: {
    receive (childData) {
      this.quizStart = childData
    },
    handleFirstClick () {
      window.scrollTo({ top: 2150, behavior: 'smooth' })
    },
    handleScroll () {
      const dateMessageElement = this.$refs.date

      if (dateMessageElement.getBoundingClientRect().top < 600) {
        this.isDateMessageVisible = true
      }
      if (dateMessageElement.getBoundingClientRect().top < 100) {
        this.isWomanImageVisible = true
      }
    }
  },
  created () {
    window.addEventListener('scroll', this.handleScroll)
  }
}
</script>

<style scoped>
.landing-page-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.landing-page {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.pseudo-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #ffffff99;
  row-gap: 30px;
}

.pseudo-header h3 {
  font-family: Roboto;
  font-size: 16px;
  font-style: normal;
  font-weight: 300;
  line-height: 19px;
  letter-spacing: 0.1em;
  z-index: 3;
}

.pseudo-header hr {
  border: 1px solid #ffffff33;
  width: 1900px;
  z-index: 3;
}

.pseudo-header span {
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 300;
  line-height: 29px;
  letter-spacing: 0.1em;
  text-align: center;
  z-index: 3;
}

.intro {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 40px;
  background: url(../assets/blik.svg) no-repeat center/cover;
}
.intro-images {
  margin-top: 62px;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.images-at-front__pray {
  width: 200px;
  height: 200px;
  border-radius: 120px;
  position: relative;
  left: 170px;
  bottom: 70px;
  z-index: 2;
}
.images-at-front__sigil {
  width: 344px;
  height: 344px;
  filter: invert(0.6);
  position: relative;
  right: 100px;
  z-index: 1;
}

.image-at-back {
  position: absolute;
  height: 1098px;
  width: 1900px;
  bottom: 0px;
  z-index: 1;
  opacity: 0.5;
}

.landing-page__starting-quiz {
  display: flex;
  flex-direction: column;
  z-index: 3;
  align-items: center;
}

.landing-page__starting-quiz span {
}
.landing-page__starting-quiz p {
  color: #f6c866;
}
.landing-page__starting-quiz p:last-child {
  margin-bottom: 60px;
}

.landing-page__starting-quiz button {
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

.landing-page__starting-quiz span {
  font-family: Roboto;
  font-size: 16px;
  font-style: normal;
  font-weight: 300;
  line-height: 19px;
  letter-spacing: 0.1em;
  text-align: center;
  color: #ffffff99;
}

.landing-page__starting-quiz .question > p:last-child {
  text-transform: uppercase;
}

.landing-page__starting-quiz .question {
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
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
.landing-page__starting-quiz > button > span {
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

.landing-page__proposal {
  margin-top: 100px;
  width: 450px;
  height: 450px;
  border: 1px solid #ffffff;
  display: flex;
  flex-direction: column;
  position: relative;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 1s ease;
}
.landing-page__proposal p {
  width: 350px;
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 500;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  color: #ffffff;
}

.landing-page__proposal span {
  color: #f6c866;
}

.landing-page__proposal img {
  position: absolute;
  top: -65px;
  left: 150px;
  width: 135px;
  height: 135px;
}

.landing-page__quote {
  margin-top: 100px;
  position: relative;
  width: 1440px;
  height: 600px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 1s ease;
}

.landing-page__quote::before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: -1;
  background-image: url('../assets/background-pray.jpg');
  background-size: cover;
  background-position: center;
  opacity: 0.25; /*Value from 0.0 to 1.0*/
}

.landing-page__quote p {
  width: 350px;
  color: #fff;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
}

.landing-page__quote span {
  color: #f6c866;
}
.quiz-quote {
  margin-top: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 150px;
  position: relative;
}

.quiz-quote p {
  margin-top: 60px;
  margin-bottom: 60px;
  font-family: 'Bad Script', cursive;
  font-family: Bad Script;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  width: 300px;
  color: #ffffff;
}

.quiz-quote__rune1 {
  height: 20px;
}
.quiz-quote__rune2 {
  height: 20px;
}

.quiz-quote__moon {
  position: absolute;
  width: 160px;
  top: 250px;
  right: 800px;
  opacity: 0.2;
}

.quiz-quote__eye {
  position: absolute;
  width: 300px;
  top: -150px;
  left: 700px;
  opacity: 0.2;
}

.pseudo-footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 40px;
  color: #9d9d9d;
  width: 1440px;
}

.visible {
  opacity: 1;
}
@media only screen and (max-width: 320px) {
  .landing-page-wrapper {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .landing-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .pseudo-header {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #ffffff99;
    row-gap: 4vw;
  }

  .pseudo-header h3 {
    font-family: Roboto;
    font-size: 3.133vw;
    font-style: normal;
    font-weight: 300;
    line-height: 2.533vw;
    letter-spacing: 0.1em;
    z-index: 3;
  }

  .pseudo-header hr {
    border: 0.133vw solid #ffffff33;
    width: 380px;
    z-index: 3;
  }

  .pseudo-header span {
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 300;
    line-height: 3.867vw;
    letter-spacing: 0.1em;
    text-align: center;
    z-index: 3;
  }

  .intro {
    width: 520px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 5.333vw;
    background: url(../assets/blik.svg) no-repeat center/cover;
  }
  .intro-images {
    margin-top: 8.267vw;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .images-at-front__pray {
    width: 26.667vw;
    height: 26.667vw;
    border-radius: 16vw;
    position: relative;
    left: 22.667vw;
    bottom: 9.333vw;
    z-index: 2;
  }
  .images-at-front__sigil {
    width: 45.867vw;
    height: 45.867vw;
    filter: invert(0.6);
    position: relative;
    right: 13.333vw;
    z-index: 1;
  }

  .image-at-back {
    position: absolute;
    height: 146.4vw;
    width: 253.333vw;
    bottom: 0vw;
    z-index: 1;
    opacity: 0.5;
  }

  .landing-page__starting-quiz {
    display: flex;
    flex-direction: column;
    z-index: 3;
    align-items: center;
  }

  .landing-page__starting-quiz span {
  }
  .landing-page__starting-quiz p {
    color: #f6c866;
  }
  .landing-page__starting-quiz p:last-child {
    margin-bottom: 8vw;
  }

  .landing-page__starting-quiz button {
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

  .landing-page__starting-quiz span {
    font-family: Roboto;
    font-size: 2.133vw;
    font-style: normal;
    font-weight: 300;
    line-height: 2.533vw;
    letter-spacing: 0.1em;
    text-align: center;
    color: #ffffff99;
  }

  .landing-page__starting-quiz .question > p:last-child {
    text-transform: uppercase;
  }

  .landing-page__starting-quiz .question {
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
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
  .landing-page__starting-quiz > button > span {
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

  .landing-page__proposal {
    margin-top: 13.333vw;
    width: 60vw;
    height: 60vw;
    border: 0.133vw solid #ffffff;
    display: flex;
    flex-direction: column;
    position: relative;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity 1s ease;
  }
  .landing-page__proposal p {
    width: 46.667vw;
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 500;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    color: #ffffff;
  }

  .landing-page__proposal span {
    color: #f6c866;
  }

  .landing-page__proposal img {
    position: absolute;
    top: -8.667vw;
    left: 20vw;
    width: 18vw;
    height: 18vw;
  }

  .landing-page__quote {
    margin-top: 13.333vw;
    position: relative;
    width: 192vw;
    height: 80vw;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity 1s ease;
  }

  .landing-page__quote::before {
    content: '';
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: -1;
    background-image: url('../assets/background-pray.jpg');
    background-size: cover;
    background-position: center;
    opacity: 0.25; /*Value from 0.0 to 1.0*/
  }

  .landing-page__quote p {
    width: 46.667vw;
    color: #fff;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
  }

  .landing-page__quote span {
    color: #f6c866;
  }
  .quiz-quote {
    margin-top: 13.333vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20vw;
    position: relative;
  }

  .quiz-quote p {
    margin-top: 8vw;
    margin-bottom: 8vw;
    font-family: 'Bad Script', cursive;
    font-family: Bad Script;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    width: 40vw;
    color: #ffffff;
  }

  .quiz-quote__rune1 {
    height: 2.667vw;
  }
  .quiz-quote__rune2 {
    height: 2.667vw;
  }

  .quiz-quote__moon {
    position: absolute;
    width: 21.333vw;
    top: 33.333vw;
    right: 106.667vw;
    opacity: 0.2;
  }

  .quiz-quote__eye {
    position: absolute;
    width: 40vw;
    top: -20vw;
    left: 93.333vw;
    opacity: 0.2;
  }

  .pseudo-footer {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 5.333vw;
    color: #9d9d9d;
    width: 90vw;
    font-size: 10px;
  }

  .visible {
    opacity: 1;
  }
}
</style>
