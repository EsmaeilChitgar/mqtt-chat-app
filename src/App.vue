<template>
  <div>
    <h1>MQTT Chat</h1>
    <div class="chat-box">
      <div v-for="(msg, index) in messages" :key="index">{{ msg }}</div>
    </div>
    <input
        v-model="input"
        @keydown.enter="sendMessage"
        placeholder="Type your message..."
    />
    <button @click="sendMessage">Send</button>
  </div>
</template>

<script setup>
import {onBeforeUnmount, onMounted, ref} from 'vue';
import mqtt from 'mqtt';

const messages = ref([]);
const input = ref('');
let client = null;

onMounted(() => {
  // Request Notification permission
  if (Notification.permission !== 'granted') {
    Notification.requestPermission();
  }

  // Connect to MQTT
  client = mqtt.connect('ws://127.0.0.1:8083/mqtt');

  client.on('connect', () => {
    console.log('Connected to MQTT broker');
    client.subscribe('chat/topic');
  });

  client.on('message', (topic, message) => {
    const msg = message.toString();
    messages.value.push(msg);

    if (Notification.permission === 'granted') {
      new Notification('New MQTT Message', {
        body: msg,
        icon: '/notification-icon.png'
      });
    }
  });
});

onBeforeUnmount(() => {
  if (client) client.end();
});

function sendMessage() {
  if (client && input.value.trim()) {
    client.publish('chat/topic', input.value);
    input.value = '';
  }
}
</script>


<style scoped>
.chat-box {
  height: 200px;
  overflow-y: scroll;
  border: 1px solid #000;
  margin-bottom: 10px;
  padding: 5px;
}
</style>
