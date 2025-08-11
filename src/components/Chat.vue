<template>
  <div class="chat-container">
    <div class="chat-messages" ref="messagesContainer">
      <div v-if="messages.length === 0" class="welcome-message">
        <h2>Welcome to SolveMate AI</h2>
        <p>How can I help you with your studies today?</p>
      </div>
      <div v-for="(message, index) in messages" :key="index" :class="['message', message.role]">
        <div class="message-content" v-html="formatMessageContent(message.content)"></div>
      </div>
      <div v-if="isLoading" class="message ai">
        <div class="message-content loading">
          <div class="typing-indicator">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>
    </div>

    <div class="chat-input">
      <form @submit.prevent="sendMessage">
        <input
          v-model="newMessage"
          type="text"
          placeholder="Type your message..."
          :disabled="isLoading"
        />
        <button type="submit" :disabled="isLoading || !newMessage.trim()">Send</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Chat',
  props: {
    messages: {
      type: Array,
      required: true,
    },
    sessionId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      newMessage: '',
      isLoading: false,
    }
  },
  methods: {
    formatMessageContent(content) {
      if (!content) return ''
      return content.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
    },
    async sendMessage() {
      if (!this.newMessage.trim() || this.isLoading) return

      const updatedMessages = [
        ...this.messages,
        {
          role: 'user',
          content: this.newMessage,
        },
      ]
      this.$emit('update:messages', updatedMessages)

      const messageToSend = this.newMessage
      this.newMessage = ''
      this.isLoading = true

      try {
        const response = await axios.post('https://56cba9d2-5f5f-4743-93a7-c05768acb091-00-39dzppiymdse1.sisko.replit.dev/api/chat', {
          message: messageToSend,
          session_id: this.sessionId,
        })

        if (response.data.status === 'success') {
          updatedMessages.push({
            role: 'ai',
            content: response.data.message,
          })
          this.$emit('update:messages', updatedMessages)
        } else {
          throw new Error(response.data.message)
        }
      } catch (error) {
        updatedMessages.push({
          role: 'ai',
          content: 'Sorry, I encountered an error. Please try again.',
        })
        this.$emit('update:messages', updatedMessages)
        console.error('Error:', error)
      } finally {
        this.isLoading = false
        this.$nextTick(() => {
          this.scrollToBottom()
        })
      }
    },
    scrollToBottom() {
      const container = this.$refs.messagesContainer
      container.scrollTop = container.scrollHeight
    },
  },
  watch: {
    messages: {
      handler() {
        this.$nextTick(() => {
          this.scrollToBottom()
        })
      },
      deep: true,
    },
  },
}
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  background-color: #f7f7f8;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 1rem;
  scroll-behavior: smooth;
}

.welcome-message {
  text-align: center;
  color: #374151;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.welcome-message h2 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.message {
  max-width: 80%;
  margin: 1rem 0;
  padding: 1rem;
  border-radius: 0.5rem;
  animation: fadeIn 0.3s ease-in-out;
  word-wrap: break-word;
}

.user {
  margin-left: auto;
  background-color: #2563eb;
  color: white;
}

.ai {
  margin-right: auto;
  background-color: white;
  color: #1f2937;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.message-content {
  line-height: 1.5;
  white-space: pre-wrap;
}

.chat-input {
  padding: 1rem;
  background-color: white;
  border-top: 1px solid #e5e7eb;
  width: 100%;
  box-sizing: border-box;
}

.chat-input form {
  display: flex;
  gap: 0.5rem;
  max-width: 100%;
}

.chat-input input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 1px solid #e5e7eb;
  border-radius: 0.375rem;
  font-size: 1rem;
  transition: border-color 0.2s;
  min-width: 0;
}

.chat-input input:focus {
  outline: none;
  border-color: #2563eb;
}

.chat-input button {
  padding: 0.75rem 1.5rem;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 0.375rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
  white-space: nowrap;
}

.chat-input button:disabled {
  background-color: #93c5fd;
  cursor: not-allowed;
}

.chat-input button:hover:not(:disabled) {
  background-color: #1d4ed8;
}

/* Loading Animation */
.typing-indicator {
  display: flex;
  gap: 0.5rem;
  padding: 0.5rem 0;
}

.typing-indicator span {
  width: 0.5rem;
  height: 0.5rem;
  background-color: #94a3b8;
  border-radius: 50%;
  animation: bounce 1.4s infinite ease-in-out;
}

.typing-indicator span:nth-child(1) {
  animation-delay: 0s;
}
.typing-indicator span:nth-child(2) {
  animation-delay: 0.2s;
}
.typing-indicator span:nth-child(3) {
  animation-delay: 0.4s;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {
  0%,
  80%,
  100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-6px);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .message {
    max-width: 90%;
  }

  .chat-input {
    padding: 0.75rem;
  }

  .chat-input input {
    padding: 0.5rem 0.75rem;
  }

  .chat-input button {
    padding: 0.5rem 1rem;
  }
}

@media (max-width: 480px) {
  .message {
    max-width: 95%;
    padding: 0.75rem;
  }

  .welcome-message h2 {
    font-size: 1.25rem;
  }
}
</style>
