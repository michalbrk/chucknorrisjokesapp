<template>
  <div id="app">
    <nav class="nav-bar">
      <div class="img"></div>
      <h2 class="title">Chuck Norris jokes</h2>
    </nav>
    <button class="get-joke" v-bind:class="{ hidden: isHidden }" v-on:click.prevent="togglingElements(); fetchingJokes();">Get me some joke!</button>
    <div class="joke-content" v-bind:class="{ active: isActive }">
      <p class="joke-text" v-bind:class="{ hidden: this.warning.isJokeHidden }">{{ this.jokeContent }}</p>
      <button class="next-joke" v-bind:class="{ hidden: this.warning.isJokeHidden }" v-on:click.prevent="fetchNextJoke"></button>
      <p class="warningMsg" v-bind:class="{ activeWarning: this.warning.isWarning }">{{ this.warning.msg }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data() {
    return {
      isActive: false,
      isHidden: false,
      jokeContent: '',
      warning: {
        msg: 'Ops! Something went wrong, please try again later...',
        isWarning: false,
        isJokeHidden: false
      }
    }
  },
  methods: {
    togglingElements() {
      this.isActive = true
      this.isHidden = true
    },
    async fetchingJokes() {
      try {
        var config = {
          headers: {
            'Content-Type': 'application/json;charset=UTF-8'
          }
        }
        var response = await axios.get("https://api.chucknorris.io/jokes/random", config)
        this.jokeContent = response.data.value
      } catch (error) {
        this.warning.isJokeHidden = true
        this.warning.isWarning = true
        console.log(error)
      }
    },
    fetchNextJoke() {
      this.jokeContent = ''
      this.fetchingJokes()
    }
  }
}
</script>

<style lang='scss'>

$appOrange: #F5853F;
$orangeShdw: #a36b48;
$arrOrngDrker: #c27342;
$initBtnLghter: #4eb5e5;
$initBtnDrker: #389ed5;
$initBtnBttm: #009dff;
$warnMsg: #ff0000;
$appFont: #fbfbfb;
$mobNavFntSize: 1.3rem;
$mobBtnFntSize: 1.3rem;
$mobTxtFntSize: 1.2rem;
$tabNavFntSize: 1.5rem;
$tabBtnFntSize: 1.4rem;
$tabTxtFntSize: 1rem;
$desNavFntSize: 1.7rem;
$desBtnFntSize: 1.5rem;
$desTxtFntSize: 1.1rem;
$hdDesNavFntSize: 1.9rem;

@import url(https://fonts.googleapis.com/css?family=Open+Sans:400,600,700);

*,*::before,*::after {
  box-sizing: border-box;
  overflow: hidden;
}
html, body {
  margin: 0;
  padding: 0;
  font-family: 'Open Sans', sans-serif;
}
body {
  background: $appOrange;
}
#app {
  display: flex;
  flex-direction: column;
  .nav-bar {
    width: 100%;
    min-height: 15vh;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-evenly;
    .img {
      align-self: center;
      width: 60px;
      height: 60px;
      border: 3px solid $initBtnBttm;
      border-radius: 50%;
      background-image: url('./assets/1.jpeg');
      background-repeat: no-repeat;
      background-size: cover;
      background-position: center;
    }
    .title {
      font-size: $mobNavFntSize;
      color: $appFont; 
    }
  }
  .get-joke {
    width: 80vw;
    min-height: 10vh;
    margin-top: 20vh;
    align-self: center;
    background: linear-gradient(to left, $initBtnLghter, $initBtnDrker);
    border: none;
    border-radius: 3px;
    color: $appFont;
    font-size: $mobBtnFntSize;
    font-weight: 600;
    cursor: pointer;
    border-bottom: 3px solid $initBtnBttm;
  }
  .joke-content {
    display: none;
    flex-direction: row;
    align-items: center;
    align-self: center;
    justify-content: center;
    background: $initBtnLghter;
    width: 90vw;
    min-height: 40vh;
    border-radius: 4px;
    margin-top: 15vh;
    .joke-text {
      width: 85%;
      font-size: $mobTxtFntSize;
      font-style: italic;
      color: $appFont;
      padding: 10px;
    }
    .next-joke {
      width: 25px;
      height: 25px;
      background: transparent;
      border-top: 7px solid $appOrange;
      border-right: 7px solid $appOrange;
      border-bottom: 0;
      border-left: 0;
      border-radius: 4px;
      transform: rotate(45deg);
      outline: none;
      cursor: pointer;
    }
    .next-joke:active {
      border-top: 7px solid $arrOrngDrker;
      border-right: 7px solid $arrOrngDrker;
    }
  }
  .warningMsg {
    display: none;
    width: 80%;
    font-size: $mobBtnFntSize;
    color: $warnMsg;
    font-weight: 700;
  }  
}
@media screen and (min-width: 370px) {
  #app {
    .nav-bar {
      min-height: 20vh;
      justify-content: flex-start;
      .img {
        margin-left: 7vw;
      }
      .title {
        margin-left: 4vw;
      }
    }
    .get-joke {
      width: 60vw;
    }
    .joke-content {
      width: 80vw;
      min-height: 30vh;
      .joke-text {
        width: 75%;
        padding-left: 0;
      }
    }
  }
}
@media screen and (min-width: 480px) {
  #app {
    .nav-bar {
      .img {
        width: 70px;
        height: 70px;
      }
      .title {
        font-size: $tabNavFntSize;
      }
    }
    .get-joke {
      width: 45vw;
    }
    .joke-content {
      justify-content: space-around;
      width: 70%;
      .joke-text {
        font-size: $tabTxtFntSize;
      }

    }
  }
}
@media screen and (min-width: 700px) {
  #app {
    .nav-bar {
      .img {
        width: 80px;
        height: 80px;
        margin-left: 3vw;
      }
      .title {
        font-size: $desNavFntSize;
      }
    }
    .get-joke {
      width: 40vw;
      font-size: $desBtnFntSize; 
    }
    .joke-content {
      width: 65%;
      .joke-text {
        font-size: $desTxtFntSize;
      }
      .next-joke {
        width: 40px;
        height: 40px;
        border-top: 8px solid $appOrange;
        border-right: 8px solid $appOrange;
      }
      .next-joke:active {
        border-top: 8px solid $arrOrngDrker;
        border-right: 8px solid $arrOrngDrker;
      }
    }
  }
}
@media screen and (min-width: 900px) {  
  #app {
    .nav-bar {
      margin-top: 1vh;
      .img {
        width: 90px;
        height: 90px;
        margin-left: 2vw;
      }
      .title {
        margin-left: 2vw;
        font-size: $hdDesNavFntSize;
      }
    }
    .get-joke {
      width: 30vw;
      min-height: 15vh;
      border-radius: 4px;
      margin-top: 15vh;
    }
    .joke-content {
      width: 50%;
      justify-content: space-evenly;
    }
  }
}
.active {
  display: flex !important;
}
.activeWarning {
  display: flex !important;
}
.hidden {
  display: none;
}
</style>
