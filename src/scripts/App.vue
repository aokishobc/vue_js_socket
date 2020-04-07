<template>
  <div>
    <p>
      <img class="logo" src="../images/logo.jpg" alt="ロゴ">
      <span class="sample">test</span>
    </p>
    <MyComponent :message="$data.message" />
    <form @submit="onSubmit">
      name : <input v-model="$data.name" type="text"><br>
      text : <input v-model="$data.text" type="text"><br>
      <button type="submit">送信</button>
    </form>
    <Chat :chatList="$data.chatList"/>
  </div>
</template>

<script>
import Vue from 'vue';
import socket from './utils/socket';

// components
import MyComponent from './components/MyComponent.vue';
import Chat from './components/Chat.vue';

export default Vue.extend({
  components: {
    MyComponent,
    Chat
  },
  data() {
    const chatList = [];
    return {
      message: '',
      text: '',
      name: '',
      chatList: chatList.map((item, index) => ({ ...item, id: index }))
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!');
    });

    socket.on('send', (message) => {
      this.$data.message = message;
      this.$data.chatList.unshift({
        message: message
      });
    });
  },
  methods: {
    /**
     * Enterボタンを押したとき
     */
    onSubmit(e) {
      e.preventDefault();
      socket.emit('send', this.$data.text);
    }
  }
});
</script>

<style lang="scss" scoped>
.logo {
  width: 40px;
}

.sample {
  color: $red;
}
</style>
