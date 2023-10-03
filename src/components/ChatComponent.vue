<template>
  <div>
    <div class="chat-container">
      <div class="chat-messages">
        <!-- Отображение сообщений чата -->
        <div v-for="(message, index) in messages" :key="index" :class="['message', message.sender == username ? 'message-right' : 'message-left']">
          <div class="message-content">{{ message.text }}</div>
          <div class="message-owner">{{ message.sender }}</div>
        </div>
      </div>
      <div class="chat-input">
        <!-- Поле ввода сообщения -->
        <v-text-field v-model="message.text" @keydown.enter="sendMessage" label="Введите сообщение"></v-text-field>
      </div>
    </div>
  </div>
</template>
  
<script lang="ts">
import { type } from 'os';
type Message = { 
  text: string,
  sender: string 
}

type SocketConn = {
  message: Message,
  messages: Message[],
  socket: WebSocket
}
export default {
  data(): SocketConn {
    return {
      message: { text: '', sender: '' },
      messages: [],
      socket: new WebSocket("ws://localhost:8080/ws"),
    };
  },
  mounted() {
    this.socket.onopen = () => {
      if (this.username) {
        this.sendNickName(this.username)
      }
    };

    this.socket.onmessage = (event: { data: string; }) => {
      const message = JSON.parse(event.data); 
      if (message.sender != this.username) {
        this.messages.push(message);
      }
    };
  },
  props: {
    username: String
  },
  methods: {
    sendNickName(nickname: string) {
      this.socket.send(nickname);
    },
    sendMessage() {
      if (this.message.text.trim() !== '') {
        if (this.username) {
          const message = { text: this.message.text, sender: this.username };
          this.socket.send(JSON.stringify(message));

          this.messages.push(message);
          this.message = {text: '', sender: ''};
        }
      }
    },
  },
};
</script>
  
<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
}

.message {
  padding: 8px;
  background-color: #e0e0e0;
  margin: 8px;
  border-radius: 8px;
  max-width: 70%;
}

.message-content {
  font-size: 24px;
}

.message-owner {
  font-size: 12px;
  font-style: italic;
}

.message-right {
  align-self: flex-end;
  background-color: #007bff;
  color: #fff;
}

.message-left {
  align-self: flex-start;
  background-color: #f3f3f3;
}

.chat-input {
  position: fixed;
  bottom: 0;
  left: 18%;
  width: 80%;
  background-color: white; /* Добавьте фон, если необходимо */
  border-top: 1px solid #ccc; /* Добавьте верхнюю границу, если необходимо */
}
</style>