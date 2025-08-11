<template>
  <div class="app-container">
    <div class="sidebar">
      <button @click="createNewChat" class="new-chat-btn">
        <span class="plus-icon">+</span>
        New Chat
      </button>
      <div class="chat-tabs">
        <div
          v-for="chat in chats"
          :key="chat.id"
          :class="['chat-tab-wrapper', { active: currentChatId === chat.id }]"
        >
          <button class="chat-tab" @click="switchChat(chat.id)">
            {{ chat.title || 'New Chat' }}
          </button>
          <button
            v-if="chats.length > 1"
            class="delete-btn"
            @click="deleteChat(chat.id)"
            title="Delete chat"
          >
            ×
          </button>
        </div>
      </div>
    </div>
    <div class="main-content">
      <Chat
        v-if="currentChat"
        :key="currentChatId"
        :messages="currentChat.messages"
        :session-id="currentChat.id"
        @update:messages="updateChatMessages"
        @update:title="updateChatTitle"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Chat from './components/Chat.vue'

export default {
  name: 'App',
  components: {
    Chat,
  },
  data() {
    return {
      chats: [],
      currentChatId: null,
    }
  },
  computed: {
    currentChat() {
      return this.chats.find((chat) => chat.id === this.currentChatId)
    },
  },
  created() {
    this.createNewChat()
  },
  methods: {
    createNewChat() {
      const newChat = {
        id: 'session_' + Date.now(),
        title: 'New Chat',
        messages: [],
      }
      this.chats.push(newChat)
      this.currentChatId = newChat.id
    },
    switchChat(chatId) {
      this.currentChatId = chatId
    },
    async deleteChat(chatId) {
      try {
        await axios.delete(`https://56cba9d2-5f5f-4743-93a7-c05768acb091-00-39dzppiymdse1.sisko.replit.dev/api/chat/${chatId}`)
        const index = this.chats.findIndex((chat) => chat.id === chatId)
        if (index !== -1) {
          this.chats.splice(index, 1)
          if (this.currentChatId === chatId) {
            this.currentChatId = this.chats[Math.max(0, index - 1)]?.id
          }
          if (this.chats.length === 0) {
            this.createNewChat()
          }
        }
      } catch (error) {
        console.error('Error deleting chat:', error)
      }
    },
    updateChatMessages(messages) {
      const chat = this.chats.find((chat) => chat.id === this.currentChatId)
      if (chat) {
        chat.messages = messages
        if (messages.length === 1 && messages[0].role === 'user') {
          this.updateChatTitle(this.generateTitle(messages[0].content))
        }
      }
    },
    updateChatTitle(title) {
      const chat = this.chats.find((chat) => chat.id === this.currentChatId)
      if (chat) {
        chat.title = title
      }
    },
    generateTitle(message) {
      const maxLength = 30
      if (message.length <= maxLength) return message
      const truncated = message.substr(0, maxLength)
      const lastSpace = truncated.lastIndexOf(' ')
      return truncated.substr(0, lastSpace) + '...'
    },
  },
}
</script>

<style scoped>
/* Reset default body margins */
:root,
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.app-container {
  display: flex;
  height: 100vh;
  width: 100%;
  background-color: #1e1e1e;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.sidebar {
  width: 260px;
  min-width: 260px;
  background-color: #1f2937;
  color: white;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  height: 100%;
  overflow-y: auto;
}

.new-chat-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  width: 100%;
  padding: 0.75rem 1rem;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 0.375rem;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.2s;
}

.new-chat-btn:hover {
  background-color: #1d4ed8;
}

.plus-icon {
  font-size: 1.25rem;
  font-weight: bold;
}

.chat-tabs {
  flex-grow: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.chat-tab-wrapper {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.chat-tab {
  flex-grow: 1;
  padding: 0.75rem;
  background-color: transparent;
  color: #e5e7eb;
  border: none;
  border-radius: 0.375rem;
  text-align: left;
  cursor: pointer;
  transition: background-color 0.2s;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.chat-tab:hover {
  background-color: #374151;
}

.chat-tab-wrapper.active .chat-tab {
  background-color: #374151;
  color: white;
}

.delete-btn {
  padding: 0.25rem 0.5rem;
  background-color: transparent;
  color: #e5e7eb;
  border: none;
  border-radius: 0.25rem;
  font-size: 1.25rem;
  cursor: pointer;
  transition: background-color 0.2s;
}

.delete-btn:hover {
  background-color: #ef4444;
  color: white;
}

.main-content {
  flex: 1;
  position: relative;
  background-color: #ffffff;
  overflow: hidden;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .sidebar {
    width: 220px;
    min-width: 220px;
  }
}

@media (max-width: 768px) {
  .app-container {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    min-width: unset;
    height: auto;
    max-height: 30vh;
    padding: 0.75rem;
  }

  .main-content {
    height: 70vh;
  }

  .chat-tabs {
    max-height: 150px;
  }
}

@media (max-width: 480px) {
  .sidebar {
    padding: 0.5rem;
  }

  .new-chat-btn {
    padding: 0.5rem 0.75rem;
  }

  .chat-tab {
    padding: 0.5rem;
  }
}
</style>
