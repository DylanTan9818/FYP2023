<template>
  <div class="chatbot">
    <div class="chat-window">
      <div class="messages">
        <div v-for="(message, index) in chat" :key="index" class="message" :class="{ 'user': message.type === 'user' }">
          {{ message.text }}
        </div>
      </div>
    </div>
    <input v-model="userMessage" @keyup.enter="sendMessage" placeholder="Type a message..." />
  </div>
</template>

<script>
export default {
  data () {
    return {
      chat: [],
      userMessage: ''
    }
  },
  methods: {
    sendMessage () {
      if (this.userMessage.trim() === '') { return }

      // Add the user's message to the chat
      this.chat.push({ text: this.userMessage, type: 'user' })

      // Send the user's message to the chatbot (you can replace this with actual API calls)
      this.sendMessageToChatbot(this.userMessage)

      // Clear the user's input
      this.userMessage = ''
    },
    async sendMessageToChatbot (message) {
      // Simulate a chatbot response (replace this with actual chatbot integration)
      try {
        const response = await fetch('http://127.0.0.1:5000/predict', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            ans: message
            // Add other features as needed
          })
        })
        const result = await response.json()
        const answer = result.answerbot
        this.chat.push({ text: answer, type: 'chatbot' })
      } catch (error) {
        const answer = error
        this.chat.push({ text: answer, type: 'chatbot' })
      }
    }
  }
}
</script>

<style scoped>
.chatbot {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.chat-window {
  border: 1px solid #ccc;
  width: 400px;
  height: 200px;
  overflow-y: scroll;
  padding: 10px;
}

.messages {
  display: flex;
  flex-direction: column;
}

.message {
  margin: 5px;
  padding: 10px;
  background-color: #f0f0f0;
  border-radius: 5px;
}

.message.user {
  align-self: flex-end;
  background-color: #D2051E;
  color: #fff;
}

input {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
}
</style>
</script>
