<template>
  <div class="chat-container">
    <!-- Hamburger Menu -->
    <div class="menu-container">
      <button class="hamburger-btn" @click="toggleMenu">
        <div class="hamburger-line"></div>
        <div class="hamburger-line"></div>
        <div class="hamburger-line"></div>
      </button>
      <div v-if="isMenuOpen" class="sidebar">
        <div class="menu-item">Menu Item 1</div>
        <div class="menu-item">Menu Item 2</div>
        <div class="menu-item">Menu Item 3</div>
      </div>
    </div>

    <!-- Chat Messages -->
    <div class="chat-messages">
      <div v-for="(message, index) in messages" :key="index"
           :class="['message', message.role === 'user' ? 'user-message' : 'assistant-message']">
        {{ message.content }}
      </div>
    </div>

    <!-- Input Area -->
    <div class="input-container">
      <div class="input-toolbar">
        <input type="file" ref="fileInput" @change="handleFileUpload" hidden>
        <button class="tool-btn" @click="$refs.fileInput.click()">
          📎 {{ selectedFile ? selectedFile.name : 'Attach' }}
        </button>

        <select v-model="selectedModel" class="model-select">
          <option v-for="model in models" :key="model" :value="model">
            {{ model }}
          </option>
        </select>
      </div>

      <div class="input-area">
        <textarea v-model="inputMessage" @keyup.enter="sendMessage"
                  placeholder="Type your message..." class="message-input"></textarea>
        <button @click="sendMessage" class="send-btn" :disabled="!inputMessage">
          Send
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isMenuOpen: false,
      messages: [],
      inputMessage: '',
      selectedModel: 'deepseek-chat',
      models: ['deepseek-chat', 'gpt-4', 'claude-2', 'bard'],
      selectedFile: null,
      mockRandomResponses: [
        "Interesting, could you elaborate on that?",
        "Here's what I found: ...",
        "Let me think about that...",
        "Based on my analysis: ...",
        "Could you clarify your question?",
        "The research suggests that ...",
        "I recommend considering ...",
        "There are multiple perspectives on this...",
        "That's a great question! Here's what I know: ...",
        "The current consensus appears to be ..."
    ]
    }
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen
    },
    sendMessage() {
      if (!this.inputMessage.trim() ) return

      this.messages.push({
        role: 'user',
        content: this.inputMessage
      })

      // Simulate assistant response
      setTimeout(() => {
        // random number based on mockRandomResponses
        const randomIndex = Math.floor(Math.random() * this.mockRandomResponses.length);
        this.messages.push({
          role: 'assistant',
          content: this.mockRandomResponses[randomIndex]
        })
      }, 1000)

      this.inputMessage = ''
    },
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0]
      // Handle file upload logic here
    }
  }
}
</script>

<style scoped>
.chat-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.menu-container {
  position: fixed;
  top: 10px;
  left: 10px;
  z-index: 1000;
}

.hamburger-btn {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
}

.hamburger-line {
  width: 25px;
  height: 2px;
  background: #333;
  margin: 4px 0;
}

.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  height: 100%;
  width: 250px;
  background: white;
  box-shadow: 2px 0 5px rgba(0,0,0,0.1);
  padding: 20px;
}

.chat-messages {
  flex-grow: 1;
  padding: 80px 20px 100px;
  overflow-y: auto;
}

.message {
  max-width: 70%;
  padding: 12px 16px;
  margin: 8px;
  border-radius: 12px;
}

.user-message {
  background: #e0e0e0;
  color: #222222;
  margin-left: auto;
}

.assistant-message {
  background: #007bff;
  color: white;
  margin-right: auto;
}

.input-container {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  padding: 16px;
  box-shadow: 0 -2px 10px rgba(0,0,0,0.1);
}

.input-toolbar {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.tool-btn {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: white;
  cursor: pointer;
}

.model-select {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 8px;

}

.input-area {
  display: flex;
  gap: 10px;
}

.message-input {
  flex-grow: 1;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  resize: none;
  height: 80px;
}

.send-btn {
  padding: 8px 24px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.send-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}
</style>