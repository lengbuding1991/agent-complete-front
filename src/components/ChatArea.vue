<template>
  <div class="chat-area" :class="{ 'dark': darkMode }">
    <div class="messages-container" ref="messagesContainer">
      <!-- 欢迎消息 -->
      <div class="welcome-message" v-if="messages.length === 0">
        <div class="welcome-content">
          <div class="welcome-avatar">
            <svg width="40" height="40" viewBox="0 0 40 40" fill="none">
              <path d="M20 0C8.954 0 0 8.954 0 20C0 31.046 8.954 40 20 40C31.046 40 40 31.046 40 20C40 8.954 31.046 0 20 0Z" fill="#1A1A1A"/>
              <path d="M25.5 15.5L20 10L14.5 15.5L20 21L25.5 15.5Z" fill="white"/>
              <path d="M14.5 24.5L20 30L25.5 24.5L20 19L14.5 24.5Z" fill="white"/>
            </svg>
          </div>
          <div class="welcome-text">
            <h3>DeepSeek</h3>
            <p>你好！我是DeepSeek，一个AI助手，很高兴为你服务！</p>
            <p>请问有什么可以帮助你的吗？</p>
          </div>
        </div>
      </div>
      
      <!-- 消息列表 -->
      <div 
        v-for="message in messages" 
        :key="message.id"
        class="message"
        :class="[message.sender, { 'dark': darkMode }]"
      >
        <div class="message-container">
          <div class="message-avatar">
            <div v-if="message.sender === 'user'" class="avatar user-avatar">
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
                <path d="M16.6667 17.5V15.8333C16.6667 14.9493 16.3155 14.1014 15.6904 13.4763C15.0653 12.8512 14.2174 12.5 13.3333 12.5H6.66667C5.78261 12.5 4.93477 12.8512 4.30964 13.4763C3.68452 14.1014 3.33333 14.9493 3.33333 15.8333V17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M10 9.16667C11.8409 9.16667 13.3333 7.67428 13.3333 5.83333C13.3333 3.99238 11.8409 2.5 10 2.5C8.15905 2.5 6.66667 3.99238 6.66667 5.83333C6.66667 7.67428 8.15905 9.16667 10 9.16667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </div>
            <div v-else class="avatar assistant-avatar">
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
                <path d="M10 0C4.477 0 0 4.477 0 10C0 15.523 4.477 20 10 20C15.523 20 20 15.523 20 10C20 4.477 15.523 0 10 0Z" fill="#1A1A1A"/>
                <path d="M12.75 7.75L10 5L7.25 7.75L10 10.5L12.75 7.75Z" fill="white"/>
                <path d="M7.25 12.25L10 15L12.75 12.25L10 9.5L7.25 12.25Z" fill="white"/>
              </svg>
            </div>
          </div>
          <div class="message-content">
            <div class="message-text" v-html="formatMessage(message.content)"></div>
            <div class="message-actions">
              <button class="action-btn" title="复制">
                <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                  <path d="M10.6667 10.6667H13.3333C13.7015 10.6667 14 10.3682 14 10V3.33333C14 2.96514 13.7015 2.66667 13.3333 2.66667H6.66667C6.29848 2.66667 6 2.96514 6 3.33333V6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                  <path d="M10 5.33333H2.66667C2.29848 5.33333 2 5.63181 2 6V12.6667C2 13.0349 2.29848 13.3333 2.66667 13.3333H9.33333C9.70152 13.3333 10 13.0349 10 12.6667V5.33333Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
              <button class="action-btn" title="点赞">
                <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                  <path d="M14 7.33333C14 8.5 13.1 9.5 12 9.5H9.5L10.3 11.1C10.5 11.5 10.3 12 9.9 12.2C9.7 12.3 9.5 12.3 9.3 12.2L6 10.6667H3.33333C2.6 10.6667 2 10.0667 2 9.33333V5.33333C2 4.6 2.6 4 3.33333 4H5.73333C5.9 4 6.06667 3.96667 6.23333 4L10.9 5.66667C11.5 5.86667 12 6.4 12 7.06667V7.33333H14Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
              <button class="action-btn" title="点踩">
                <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                  <path d="M2 8.66667C2 7.5 2.9 6.5 4 6.5H6.5L5.7 4.9C5.5 4.5 5.7 4 6.1 3.8C6.3 3.7 6.5 3.7 6.7 3.8L10 5.33333H12.6667C13.4 5.33333 14 5.93333 14 6.66667V10.6667C14 11.4 13.4 12 12.6667 12H10.2667C10.1 12 9.93333 12.0333 9.76667 12L5.1 10.3333C4.5 10.1333 4 9.6 4 8.93333V8.66667H2Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <!-- 思考指示器 -->
      <div v-if="thinking" class="message assistant" :class="{ 'dark': darkMode }">
        <div class="message-container">
          <div class="message-avatar">
            <div class="avatar assistant-avatar">
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
                <path d="M10 0C4.477 0 0 4.477 0 10C0 15.523 4.477 20 10 20C15.523 20 20 15.523 20 10C20 4.477 15.523 0 10 0Z" fill="#1A1A1A"/>
                <path d="M12.75 7.75L10 5L7.25 7.75L10 10.5L12.75 7.75Z" fill="white"/>
                <path d="M7.25 12.25L10 15L12.75 12.25L10 9.5L7.25 12.25Z" fill="white"/>
              </svg>
            </div>
          </div>
          <div class="message-content">
            <div class="thinking-indicator">
              <div class="thinking-dot"></div>
              <div class="thinking-dot"></div>
              <div class="thinking-dot"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChatArea',
  props: {
    messages: Array,
    thinking: Boolean,
    darkMode: Boolean
  },
  watch: {
    messages: {
      handler() {
        this.$nextTick(() => {
          this.scrollToBottom()
        })
      },
      deep: true
    },
    thinking() {
      this.$nextTick(() => {
        this.scrollToBottom()
      })
    }
  },
  mounted() {
    this.scrollToBottom()
  },
  methods: {
    scrollToBottom() {
      const container = this.$refs.messagesContainer
      if (container) {
        container.scrollTop = container.scrollHeight
      }
    },
    formatMessage(content) {
      // 简单的Markdown格式化
      return content
        .replace(/\n/g, '<br>')
        .replace(/```(\w+)?\n([^`]+)```/g, '<pre><code class="language-$1">$2</code></pre>')
        .replace(/`([^`]+)`/g, '<code>$1</code>')
        .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
        .replace(/\*(.*?)\*/g, '<em>$1</em>')
        .replace(/## (.*?)(<br>|$)/g, '<h2>$1</h2>')
        .replace(/# (.*?)(<br>|$)/g, '<h1>$1</h1>')
    }
  }
}
</script>

<style scoped>
.chat-area {
  flex: 1;
  overflow: hidden;
  position: relative;
  background: #ffffff;
  transition: background-color 0.3s;
  width: 100%;
}

.chat-area.dark {
  background: #343541;
}

.messages-container {
  height: 100%;
  overflow-y: auto;
  padding: 1rem;
  width: 100%;
}

.welcome-message {
  padding: 2rem 0;
  border-bottom: 1px solid #e5e5e5;
  transition: border-color 0.3s;
}

.dark .welcome-message {
  border-bottom-color: #565869;
}

.welcome-content {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  width: 100%;
}

.welcome-avatar {
  flex-shrink: 0;
}

.welcome-text h3 {
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: #1a1a1a;
  transition: color 0.3s;
}

.dark .welcome-text h3 {
  color: #ffffff;
}

.welcome-text p {
  color: #6b7280;
  margin-bottom: 0.5rem;
  transition: color 0.3s;
}

.dark .welcome-text p {
  color: #d1d5db;
}

.message {
  padding: 1.5rem 0;
  border-bottom: 1px solid #f3f4f6;
  transition: border-color 0.3s, background-color 0.3s;
}

.dark .message {
  border-bottom-color: #40414f;
}

.message.user {
  background: #f7f7f8;
  margin: 0;
  padding: 1.5rem 0;
  border-bottom: none;
}

.dark .message.user {
  background: #444654;
}

.message:last-child {
  border-bottom: none;
}

.message-container {
  display: flex;
  gap: 1rem;
  width: 100%;
  padding: 0 1rem;
}

.message-avatar {
  flex-shrink: 0;
}

.avatar {
  width: 30px;
  height: 30px;
  border-radius: 0.25rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.user-avatar {
  background: #10a37f;
  color: white;
}

.assistant-avatar {
  background: #1a1a1a;
  color: white;
}

.message-content {
  flex: 1;
  min-width: 0;
}

.message-text {
  line-height: 1.6;
  color: #1a1a1a;
  white-space: pre-wrap;
  word-wrap: break-word;
  transition: color 0.3s;
}

.dark .message-text {
  color: #d1d5db;
}

.message-text :deep(code) {
  background: #f1f1f3;
  padding: 0.125rem 0.25rem;
  border-radius: 0.25rem;
  font-family: 'Monaco', 'Menlo', 'Ubuntu Mono', monospace;
  font-size: 0.875rem;
  color: #1a1a1a;
}

.dark .message-text :deep(code) {
  background: #565869;
  color: #d1d5db;
}

.message-text :deep(pre) {
  background: #f7f7f8;
  padding: 1rem;
  border-radius: 0.5rem;
  overflow-x: auto;
  margin: 0.75rem 0;
  border: 1px solid #e5e5e5;
}

.dark .message-text :deep(pre) {
  background: #40414f;
  border-color: #565869;
}

.message-text :deep(pre code) {
  background: none;
  padding: 0;
  color: inherit;
}

.message-text :deep(h1) {
  font-size: 1.5rem;
  font-weight: 600;
  margin: 1rem 0 0.5rem;
}

.message-text :deep(h2) {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0.75rem 0 0.5rem;
}

.message-text :deep(strong) {
  font-weight: 600;
}

.message-text :deep(em) {
  font-style: italic;
}

.message-actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.75rem;
  opacity: 0;
  transition: opacity 0.2s;
}

.message:hover .message-actions {
  opacity: 1;
}

.action-btn {
  width: 28px;
  height: 28px;
  background: transparent;
  border: 1px solid #d1d5db;
  border-radius: 0.375rem;
  color: #6b7280;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.action-btn:hover {
  background: #f3f4f6;
  color: #1a1a1a;
}

.dark .action-btn {
  border-color: #565869;
  color: #d1d5db;
}

.dark .action-btn:hover {
  background: #4a4b59;
  color: #ffffff;
}

.thinking-indicator {
  display: flex;
  gap: 0.25rem;
  align-items: center;
  height: 1.5rem;
}

.thinking-dot {
  width: 8px;
  height: 8px;
  background: #c5c5d2;
  border-radius: 50%;
  animation: thinking 1.4s ease-in-out infinite both;
}

.dark .thinking-dot {
  background: #a1a1b3;
}

.thinking-dot:nth-child(1) {
  animation-delay: -0.32s;
}

.thinking-dot:nth-child(2) {
  animation-delay: -0.16s;
}

@keyframes thinking {
  0%, 80%, 100% {
    transform: scale(0.8);
    opacity: 0.5;
  }
  40% {
    transform: scale(1);
    opacity: 1;
  }
}

/* 响应式设计 */
@media (max-width: 768px) {
  .messages-container {
    padding: 0.5rem;
  }
  
  .welcome-message {
    padding: 1rem 0;
  }
  
  .message {
    padding: 1rem 0;
  }
  
  .message-container {
    padding: 0 0.5rem;
    gap: 0.75rem;
  }
  
  .avatar {
    width: 24px;
    height: 24px;
  }
  
  .message-text {
    font-size: 0.9rem;
    line-height: 1.5;
  }
  
  .message-text :deep(h1) {
    font-size: 1.25rem;
  }
  
  .message-text :deep(h2) {
    font-size: 1.1rem;
  }
  
  .message-text :deep(pre) {
    padding: 0.75rem;
    font-size: 0.8rem;
  }
}

@media (max-width: 480px) {
  .messages-container {
    padding: 0.25rem;
  }
  
  .message-container {
    padding: 0 0.25rem;
    gap: 0.5rem;
  }
  
  .avatar {
    width: 20px;
    height: 20px;
  }
  
  .message-text {
    font-size: 0.85rem;
  }
  
  .message-text :deep(pre) {
    padding: 0.5rem;
    font-size: 0.75rem;
  }
}
</style>