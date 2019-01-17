<template>
  <section>
    <form>
      <div class="form-group">
        <label>今日で何日目？ <span v-if="days">{{ days }}日目</span><br>
          <input type="text" v-model="days">
        </label>
      </div>
      <div class="form-group">
        <label>今日は何時間プログラミングした？<span v-if="todayTime">{{ todayTime }}h</span><br>
          <input type="text" v-model="todayTime">
        </label>
      </div>
      <div class="form-group">
        <label>合計何時間プログラミングした？<br>
          <input type="text" v-model="totalTime">
        </label>
        <span v-if="totalTime">{{ totalTime }}h</span>
      </div>
      <div class="form-group">
        <label>やった内容は？<br>
          <textarea name="" id="" cols="30" rows="3" v-model="content" wrap=”hard”></textarea>
        </label>
      </div>
      <div class="hashtag-area">
        <div class="form-group hashtag">
          <input id="webukatsu" type="checkbox" value="#ウェブカツ" name="webukatsu" v-model="checkedTags"><label for="webukatsu">#ウェブカツ</label>
        </div>
        <div class="form-group hashtag">
          <input id="100days" type="checkbox" value="#100DaysOfCode" name="100days" v-model="checkedTags"><label for="100days">#100DaysOfCode</label>
        </div>
        <div class="form-group hashtag">
          <input id="new" type="checkbox" value="#駆け出しエンジニアと繋がりたい" name="new" v-model="checkedTags"><label for="new">#駆け出しエンジニアと繋がりたい</label>
        </div>
      </div>
      <div class="button-area">
        <button @click.prevent="createMessage">Tweet作成</button>
      </div>
    </form>
    <div class="message-area" v-if="tweetMessage">
      {{ tweetMessage }}
      {{ checkedTags.join(' ') }}
    </div>
    <div class="tweet-button-area" v-if="tweetMessage">
      <a :href="tweetUrl" target="_blank"><img src="../../assets/Twitter_Social_Icon_Circle_Color.svg" alt="tweet"><span>tweet</span></a>
    </div>
  </section>
</template>

<script>
import Vue from 'vue'
import Vuelidate from 'vuelidate'
import { required } from 'vuelidate/lib/validators'
Vue.use(Vuelidate)

export default {
  data() {
    return {
      days: '',
      todayTime: '',
      totalTime: '',
      content: '',
      tweetMessage: '',
      checkedTags: ['#ウェブカツ', '#100DaysOfCode', '#駆け出しエンジニアと繋がりたい'],
      tweetUrl: '',
    }
  },
  methods: {
    createMessage: function() {
      this.tweetMessage = `\n【DAY${this.days} / TODAY：${this.todayTime}h / TOTAL：${this.totalTime}h】\n${this.content}
      `
      this.createUrl()
      this.content = ''
    },
    createUrl: function() {
      let txt = encodeURIComponent(this.tweetMessage)
      let hashtag = encodeURIComponent(this.checkedTags.join(' ').slice(1))
      this.tweetUrl = `https://twitter.com/intent/tweet?text=${txt}&hashtags=${hashtag}`
    }
  },
  computed: {

  }
}
</script>

<style scoped>
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
    outline: 0;
    border-color: #00FF00;
  }

  span {
    padding-left: 20px;
    color: rgb(43, 138, 216);
  }

  .hashtag-area {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    width: 80%;
    margin: 0 auto;
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
</style>
