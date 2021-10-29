<template>
  <div class="answerPage">
    <div class="answerPage__message">
      <p>
        Спасибо за Ваши ответы! <br /><span
          >Мы подготовили для Вас персональную аудио запись с Вашим
          прогнозом.</span
        >
      </p>
    </div>
    <p class="answerPage__anonsement">
      Вы можете узнать, как повлиять на события, которые ожидают вас в ближайшем
      будущем.
    </p>
    <div class="answerPage__date-message">
      <p class="answerPage-message">
        <span
          >Первое значимое событие может произойти уже
          <span v-if="tomorrowDate !== false">{{ tomorrowDate }}</span
          >,
        </span>
        Вам надо быть готовым, что бы последствия не оказались
        <span>необратимыми.</span>
      </p>
    </div>
    <p class="answerPage__aboveButtonText">
      Нажмите на кнопку ниже прямо сейчас и наберите наш номер телефона.
      Прослушайте важную информацию!
    </p>
    <div class="answerPage__buttons">
      <button v-on:click="handleClick">
        Позвонить и прослушать
        <span></span>
      </button>
    </div>
    <div v-if="data !== null" class="answerPage__recieved-data">
      <h3>{{ data.name }}</h3>
      <p>Birth year: {{ data.birth_year }}</p>
      <p>Gender: {{ data.gender }}</p>
      <p>Height: {{ data.height }}</p>
      <p>Weight: {{ data.mass }}</p>
    </div>
    <div
      v-if="data === null && error !== null"
      class="answerPage__response-error"
    >
      <p>{{ error.message }}</p>
    </div>
    <div class="answerPage__terms">
      <p>
        TERMENI SI CONDITII: ACESTA ESTE UN SERVICIU DE DIVERTISMENT. PRIN
        FOLOSIREA LUI DECLARATI CA AVETI 18 ANI IMPLINITI
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnswerPage',
  data: () => ({
    data: null,
    tomorrowDate: '',
    error: null
  }),
  methods: {
    handleClick () {
      this.getData()
    },
    async getData () {
      try {
        return fetch('https://swapi.dev/api/people/1/')
          .then(res => res.json())
          .then(data => (this.data = data))
      } catch (error) {
        this.error = error
      }
    }
  },
  computed: {
    getUserAge () {
      if (this.$store.getters.getUserAge !== null) {
        return this.$store.getters.getUserAge
      } else return 0
    }
  },
  created () {
    const currentDate = new Date(new Date().getTime() + 24 * 60 * 60 * 1000)
    let day = currentDate.getDate()
    let month = currentDate.getMonth() + 1
    const year = currentDate.getFullYear()

    if (day < 10) day = '0' + day
    if (month < 10) month = '0' + month

    const date = `${day}.${month}.${year}`

    this.tomorrowDate = date
  }
}
</script>

<style scoped>
.answerPage {
  padding: 24px 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  position: relative;
}

.answerPage__message {
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
  margin-bottom: 34px;
}

.answerPage__message::after {
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

.answerPage__message > p {
  width: 450px;
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
}

.answerPage__message > p > span {
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 700;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
}

.answerPage__anonsement {
  max-width: 430px;
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 300;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;

  line-height: 35px;
  color: #fff;
  margin-bottom: 21px;
  padding: 0 30px;
}

.answerPage__date-message {
  height: 280px;
  width: 450px;
  height: 172px;
  border: 1px solid #fff;
  padding-left: 50px;
  padding-right: 50px;
  padding-top: 64px;
  padding-bottom: 38px;

  display: flex;
  align-items: center;
}

.answerPage__aboveButtonText {
  width: 460px;
  height: 100px;
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 300;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
  margin-top: 30px;
  margin-bottom: 30px;
  color: #fff;
}

.answerPage__date-message .answerPage-message {
  color: #f6c866;
  font-family: Roboto;
  font-size: 25px;
  font-style: normal;
  font-weight: 400;
  line-height: 35px;
  letter-spacing: 0em;
  text-align: center;
}

.answerPage__date-message > p > span {
  text-transform: uppercase;
  font-weight: bold;
}

.answerPage__buttons > button {
  background: linear-gradient(
    90deg,
    rgba(76, 217, 100, 0.9) -6.2%,
    rgba(50, 185, 73, 0.9) 100%
  );
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

.answerPage__terms {
  width: 294px;
  font-family: Roboto;
  font-size: 12px;
  font-style: normal;
  font-weight: 400;
  line-height: 18px;
  letter-spacing: 3px;
  text-align: center;
  color: #9d9d9d;
}

.answerPage__recieved-data {
  padding-bottom: 20px;
  color: #fff;
}

.answerPage__recieved-data h3 {
  color: #f6c866;
}

@media only screen and (max-width: 320px) {
  .answerPage {
    padding: 3.2vw 0;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    position: relative;
  }

  .answerPage__message {
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
    margin-bottom: 4.533vw;
  }

  .answerPage__message::after {
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

  .answerPage__message > p {
    width: 60vw;
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
  }

  .answerPage__message > p > span {
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 700;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
  }

  .answerPage__anonsement {
    max-width: 57.333vw;
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 300;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;

    line-height: 4.667vw;
    color: #fff;
    margin-bottom: 2.8vw;
    padding: 0 4vw;
  }

  .answerPage__date-message {
    height: 37.333vw;
    width: 60vw;
    height: 22.933vw;
    border: 0.133vw solid #fff;
    padding-left: 6.667vw;
    padding-right: 6.667vw;
    padding-top: 8.533vw;
    padding-bottom: 5.067vw;

    display: flex;
    align-items: center;
  }

  .answerPage__aboveButtonText {
    width: 61.333vw;
    height: 13.333vw;
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 300;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
    margin-top: 4vw;
    margin-bottom: 4vw;
    color: #fff;
  }

  .answerPage__date-message .answerPage-message {
    color: #f6c866;
    font-family: Roboto;
    font-size: 3.333vw;
    font-style: normal;
    font-weight: 400;
    line-height: 4.667vw;
    letter-spacing: 0em;
    text-align: center;
  }

  .answerPage__date-message > p > span {
    text-transform: uppercase;
    font-weight: bold;
  }

  .answerPage__buttons > button {
    background: linear-gradient(
      90deg,
      rgba(76, 217, 100, 0.9) -6.2%,
      rgba(50, 185, 73, 0.9) 100%
    );
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

  .answerPage__terms {
    width: 39.2vw;
    font-family: Roboto;
    font-size: 1.6vw;
    font-style: normal;
    font-weight: 400;
    line-height: 2.4vw;
    letter-spacing: 0.4vw;
    text-align: center;
    color: #9d9d9d;
  }

  .answerPage__recieved-data {
    padding-bottom: 2.667vw;
    color: #fff;
  }

  .answerPage__recieved-data h3 {
    color: #f6c866;
  }
}
</style>
