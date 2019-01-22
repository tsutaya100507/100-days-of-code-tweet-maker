<template>
  <section>
    <div class="description">このサイトでは、#100DaysOfCodeの学習報告tweetを簡単に作ることができます。<br>学習日数、今日の学習時間、トータルの学習時間、今日学習した内容を報告しよう。<br><span>※本サイトは、<a href="https://twitter.com/monmonta0127" target="_blank">@monmonta0127</a>が個人的に作成したものであり、ウェブカツ!!とは関係ありません。</span></div>
    <form>
      <div class="form-group">
        <label>今日で何日目？
          <span :class="{'error-message': isRequiredError($v.days)}" v-if="$v.days.$dirty && !$v.days.required"><img class="error-img" src="../../assets/error.svg">必須項目です</span>
          <span :class="{'error-message': isNumericError($v.days)}" v-else-if="!$v.days.numeric"><img class="error-img" src="../../assets/error.svg">半角数字を入力してください</span>
          <span v-else-if="days">DAY{{ days }}</span><br>
          <input type="text" v-model.trim="days" @input="$v.days.$touch" :class="{error: $v.days.$error }" placeholder="例：3">
        </label>
      </div>
      <div class="form-group">
        <label>今日は何時間プログラミングした？
          <span :class="{'error-message': isRequiredError($v.todayTime)}" v-if="$v.todayTime.$dirty && !$v.todayTime.required"><img class="error-img" src="../../assets/error.svg">必須項目です</span>
          <span :class="{'error-message': isNumericError($v.todayTime)}" v-else-if="!$v.todayTime.numeric"><img class="error-img" src="../../assets/error.svg">半角数字を入力してください</span>
          <span v-else-if="todayTime">{{ todayTime }}h</span><br>
          <input type="text" v-model.trim="todayTime" @input="$v.todayTime.$touch" :class="{error: $v.todayTime.$error }" placeholder="例：3.5">
        </label>
      </div>
      <div class="form-group">
        <label>合計何時間プログラミングした？
          <span :class="{'error-message': isRequiredError($v.totalTime)}" v-if="$v.totalTime.$dirty && !$v.totalTime.required"><img class="error-img" src="../../assets/error.svg">必須項目です</span>
          <span :class="{'error-message': isNumericError($v.totalTime)}" v-else-if="!$v.totalTime.numeric"><img class="error-img" src="../../assets/error.svg">半角数字を入力してください</span>
          <span v-else-if="totalTime">{{ totalTime }}h</span><br>
          <input type="text" v-model.trim="totalTime" @input="$v.totalTime.$touch" :class="{ error: $v.totalTime.$error }" placeholder="例：50">
        </label>
      </div>
      <div class="form-group">
        <label>今日学習した内容は？<br>
          <textarea name="" id="" cols="30" rows="3" v-model="content" wrap=”hard” placeholder="例：HTML・CSS部を完了させた。"></textarea>
        </label>
      </div>
      <div class="hashtag-title">#ハッシュタグ</div>

      <div class="hashtag-area">
        <div class="form-group hashtag">
          <input id="webukatsu" type="checkbox" value="#ウェブカツ" name="webukatsu" v-model="checkedTags"><label for="webukatsu">#ウェブカツ</label>
        </div>
        <div class="form-group hashtag">
          <input id="TechBaton" type="checkbox" value="#TechBaton" name="TechBaton" v-model="checkedTags"><label for="TechBaton">#TechBaton</label>
        </div>
        <div class="form-group hashtag">
          <input id="new" type="checkbox" value="#駆け出しエンジニアと繋がりたい" name="new" v-model="checkedTags"><label for="new">#駆け出しエンジニアと繋がりたい</label>
        </div>
        <div class="form-group hashtag">
          <input id="study" type="checkbox" value="#プログラミング学習" name="study" v-model="checkedTags"><label for="new">#プログラミング学習
          </label>
        </div>
      </div>
      <div class="button-area">
        <button @click.prevent="createMessage" :disabled="$v.$invalid">Tweet作成</button>
      </div>
    </form>
    <div class="message-area" v-if="tweetMessage">
      {{ tweetMessage }}
    </div>
    <div class="tweet-button-area" v-if="tweetMessage">
      <a :href="tweetUrl" target="_blank"><img src="../../assets/Twitter_Social_Icon_Circle_Color.svg" alt="tweet"><span>tweet</span></a>
    </div>
  </section>
</template>

<script>
import Vue from 'vue'
import Vuelidate from 'vuelidate'
import { required, numeric } from 'vuelidate/lib/validators'
Vue.use(Vuelidate)

export default {
  data() {
    return {
      days: '',
      todayTime: '',
      totalTime: '',
      content: '',
      tweetMessage: '',
      checkedTags: ['#ウェブカツ'],
      tweetUrl: '',
    }
  },
  validations: {
    days: {
      required,
      numeric
    },
    todayTime: {
      required,
      numeric
    },
    totalTime: {
      required,
      numeric
    }
  },
  methods: {
    createMessage: function() {
      this.tweetMessage = `\n【DAY${this.days} / TODAY：${this.todayTime}h / TOTAL：${this.totalTime}h】#100DaysOfCode\n${this.content}\n\n${this.checkedTags.join(' ')}
      `
      this.createUrl()
    },
    createUrl: function() {
      let txt = encodeURIComponent(this.tweetMessage)
      this.tweetUrl = `https://twitter.com/intent/tweet?text=${txt}`
    },
    clearMessage: function() {
      this.days = ''
      this.todayTime = ''
      this.totalTime = ''
      this.content = ''
    },
    isRequiredError: function(item) {
      if (item.$dirty && !item.required) {
        return true
      } else {
        return false
      }
    },
    isNumericError: function(item) {
      if (item.$dirty && !item.numeric) {
        return true
      } else {
        return false
      }
    }
  }
}
</script>

<style scoped>
  .description {
    width: 64%;
    margin: 0 auto 35px;
  }

  .description span {
    font-size: 12px;
  }

  .description a {
    color: rgb(43, 138, 216);
    font-weight: bold;
  }

  .description a:hover {
    text-decoration: none;
  }

  form {
    width: 80%;
    margin: 0 auto;
    font-family: 'M PLUS 1p', sans-serif;
  }

  .form-group {
    width: 80%;
    min-width: 300px;
    margin: 10px auto 0;
  }

  label {
    width: 100%;
  }

  input[type="text"], textarea {
    width: 100%;
    margin-top: 10px;
    padding: 5px 10px;
    background-color: #000000;
    border: 1px solid rgba(0, 255, 0, 0.5);
    font-size: 15px;
    color: #00FF00;
  }

  input:focus, textarea:focus {
    outline: none;
    border-color: #00FF00;
  }

  span {
    padding-left: 20px;
    color: rgb(43, 138, 216);
  }

  .hashtag-title {
    width: 80%;
    margin: 0 auto 20px;
  }

  .hashtag-area {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    width: 80%;
    margin: 0 auto 30px;
  }

  .hashtag-area .form-group {
    min-width: 0;
    margin: 0;
  }

  .hashtag-area label {
    /* width: 50%; */
    display: block;
  }

  .hashtag {
    display: flex;
    flex-wrap: nowrap;
    width: 50%;
    line-height: 14px;
  }

  .hashtag label {
    margin-left: 5px;
  }

  .button-area {
    padding: 20px;
    text-align: center;
  }

  .button-area button {
    outline: none;
    border: none;
    background: #00FF00;
    border-radius: 50px;
    padding: 5px 15px;
  }

  .message-area {
    width: 64%;
    margin: 0 auto;
    border: 1px solid white;
    white-space: pre-wrap;
    word-wrap: break-word;
    text-align: left;
    text-align: justify;
    text-justify: inter-ideograph;
  }

  .tweet-button-area {
    text-align: center;
  }

  .tweet-button-area span {
    padding: 0;
    color: #000000;
  }

  .tweet-button-area a {
    display: block;
    height: 60px;
    width: 200px;
    margin: 30px auto;
    border: 1px solid #00FF00;
    border-radius: 50px;
    background-color: #00FF00;
    line-height: 58px;
    font-size: 25px;
  }

  .tweet-button-area a:hover {
    text-decoration: none;
  }

  img {
    width: 50px;
    height: 50px;
  }

  input.error {
    outline: none;
    border: 1px solid red;
  }

  input.error:focus {
    border: 1px solid red;
  }

  .error-message {
    color: red;
  }

  .error-img {
    width: 20px;
    height: 20px;
    margin-right: 5px;
  }
</style>
