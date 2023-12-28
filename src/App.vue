<template>
  <div class="telegram-mini-app">
    1
    <div class="test">{{ telegramWebAppData.user.id }}</div>
    2
    <div class="profile">
      <h2>Профиль пользователя</h2>
      <p>Здравствуйте, {{`${userData?.first_name} ${userData.last_name}`}}</p>
      <p>{{`@${userData?.username}`}}</p>
      <img :src="decodeURIComponent(userData?.photo_url)" >
    </div>
    <div class="channels">
      <h3>Ваши каналы</h3>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios';

export default {
  name: 'TelegramMiniApp',
  data() {
    return {
      telegramWebAppData: null,
      title: "Telegram Mini App",
      requestText: "",
      buttonDisabled: false,
      response: [],
      responseKey: 0,
    };
  },
  computed: {
    userData() {
      return this.$store.getters.getUserData;
    },
  },
  created(){
    let uri = window.location.href.split('?');
    if(uri.length == 2) {
      let vars = uri[1].split('&');
      let getVars = {};
      let tmp = '';
      vars.forEach(function(v) {
        tmp = v.split('=');
        if(tmp.length == 2)
          getVars[tmp[0]] = tmp[1];
      });
      console.log(getVars);
      this.updateUserData(getVars)
    }
  },
  mounted() {
    if (window.Telegram && window.Telegram.WebApp) {
      // Получаем данные из Telegram.WebApp
      this.telegramWebAppData = decodeURIComponent(window.Telegram.WebApp.initData);
      // query_id=AAEdq3R2AAAAAB2rdHb-RVel&user={"id":1987357469,"first_name":"Dmitriy","last_name":"Alexandrov","username":"ditritus_h","language_code":"ru","allows_write_to_pm":true}&auth_date=1703739367&hash=4c71272061cb0c6abb6832d9167b1d8a6f846a44e02177d819cc442e990aba1d    }

  },
  methods: {
    updateUserData(data) {
      this.$store.commit('setUserData', data);
    },
    sendRequest() {
      // Отправка запроса
      this.buttonDisabled = true
      this.response.unshift(`${this.requestText} ${this.responseKey}`);
      this.responseKey += 1;
      this.requestText = ""
      this.buttonDisabled = false
    },
  },
};
</script>

<style scoped>

.telegram-mini-app {
  background-color: white;
}

h1 {
  font-size: 1.5em;
}

textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
}

button {
  background-color: #42b983;
  color: white;
  padding: 10px;
  cursor: pointer;
  border: none;
}

.response {
  background-color: #f0f0f0;
  padding: 10px;
  margin-top: 10px;
}

.fade-enter-active {
  transition: all 0.3s ease-out;
}
.fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.fade-enter-from,
.fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}

</style>
