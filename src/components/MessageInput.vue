<template>
  <div class="message-input-container" :class="{ 'dark': darkMode }">
    <div class="input-wrapper">
      <div class="input-inner" :class="{ 'focused': isFocused }">
        <textarea
          ref="textarea"
          v-model="message"
          @keydown="handleKeydown"
          @input="autoResize"
          @focus="isFocused = true"
          @blur="isFocused = false"
          placeholder="输入消息...（输入 / 获取帮助）"
          rows="1"
          class="message-textarea"
          :disabled="thinking"
        ></textarea>
        <div class="input-actions">
          <button class="action-btn" title="上传文件" :disabled="thinking">
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
              <path d="M17.5 12.5V15.8333C17.5 16.2754 17.3244 16.6993 17.0118 17.0118C16.6993 17.3244 16.2754 17.5 15.8333 17.5H4.16667C3.72464 17.5 3.30072 17.3244 2.98816 17.0118C2.67559 16.6993 2.5 16.2754 2.5 15.8333V12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M14.1667 6.66667L10 2.5L5.83333 6.66667" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M10 2.5V12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </button>
          <button 
            class="send-button"
            :disabled="!message.trim() || thinking"
            @click="sendMessage"
            :class="{ 'disabled': !message.trim() || thinking }"
          >
            <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
              <path d="M2.5 5.83333L17.5 10L2.5 14.1667V5.83333Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </button>
        </div>
      </div>
      <div class="input-footer">
        <div class="footer-text">
          <span class="footer-icon">🔍</span>
          DeepSeek可以联网，请告知它打开联网搜索
        </div>
        <div class="model-info">
          模型: DeepSeek Chat
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MessageInput',
  props: {
    thinking: Boolean,
    darkMode: Boolean
  },
  data() {
    return {
      message: '',
      isFocused: false
    }
  },
  methods: {
    sendMessage() {
      if (this.message.trim() && !this.thinking) {
        this.$emit('send-message', this.message.trim())
        this.message = ''
        this.$nextTick(() => {
          this.autoResize()
        })
      }
    },
    handleKeydown(event) {
      if (event.key === 'Enter' && !event.shiftKey) {
        event.preventDefault()
        this.sendMessage()
      }
    },
    autoResize() {
      const textarea = this.$refs.textarea
      textarea.style.height = 'auto'
      textarea.style.height = Math.min(textarea.scrollHeight, 120) + 'px'
    },
    focus() {
      this.$refs.textarea.focus()
    }
  },
  emits: ['send-message']
}
</script>

<style scoped>
.message-input-container {
  padding: 1rem;
  background: #ffffff;
  border-top: 1px solid #e5e5e5;
  transition: background-color 0.3s, border-color 0.3s;
  width: 100%;
}

.message-input-container.dark {
  background: #343541;
  border-top-color: #565869;
}

.input-wrapper {
  width: 100%;
  max-width: none;
  position: relative;
}

.input-inner {
  position: relative;
  display: flex;
  align-items: flex-end;
  background: #ffffff;
  border: 1px solid #d1d5db;
  border-radius: 0.75rem;
  padding: 0.75rem 1rem;
  transition: all 0.2s;
}

.input-inner.focused {
  border-color: #10a37f;
  box-shadow: 0 0 0 2px rgba(16, 163, 127, 0.1);
}

.dark .input-inner {
  background: #40414f;
  border-color: #565869;
}

.dark .input-inner.focused {
  border-color: #10a37f;
  box-shadow: 0 0 0 2px rgba(16, 163, 127, 0.2);
}

.message-textarea {
  flex: 1;
  border: none;
  outline: none;
  resize: none;
  font-family: inherit;
  font-size: 1rem;
  line-height: 1.5;
  color: #1a1a1a;
  background: transparent;
  max-height: 120px;
  min-height: 24px;
  transition: color 0.3s;
}

.message-textarea::placeholder {
  color: #9ca3af;
}

.dark .message-textarea {
  color: #d1d5db;
}

.dark .message-textarea::placeholder {
  color: #9ca3af;
}

.message-textarea:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.input-actions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-left: 0.75rem;
}

.action-btn {
  width: 32px;
  height: 32px;
  background: transparent;
  border: none;
  color: #6b7280;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 0.375rem;
  transition: all 0.2s;
}

.action-btn:hover:not(:disabled) {
  background: #f3f4f6;
  color: #1a1a1a;
}

.action-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.dark .action-btn:hover:not(:disabled) {
  background: #4a4b59;
  color: #ffffff;
}

.send-button {
  width: 32px;
  height: 32px;
  background: #10a37f;
  border: none;
  border-radius: 0.375rem;
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.send-button:hover:not(.disabled) {
  background: #0d8c6c;
}

.send-button.disabled {
  background: #f3f4f6;
  color: #9ca3af;
  cursor: not-allowed;
}

.dark .send-button.disabled {
  background: #565869;
  color: #6b7280;
}

.input-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 0.5rem;
}

.footer-text {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.75rem;
  color: #6b7280;
  transition: color 0.3s;
}

.dark .footer-text {
  color: #9ca3af;
}

.footer-icon {
  font-size: 0.875rem;
}

.model-info {
  font-size: 0.75rem;
  color: #6b7280;
  transition: color 0.3s;
}

.dark .model-info {
  color: #9ca3af;
}

/* 平板设备 (769px - 1024px) */
@media (max-width: 1024px) and (min-width: 769px) {
  .input-wrapper {
    padding: 0.75rem 1rem;
  }
  
  .message-input-container {
    padding: 0.75rem;
  }
  
  .message-textarea {
    font-size: 0.95rem;
    padding: 0.75rem 1rem;
  }
  
  .send-button {
    width: 36px;
    height: 36px;
  }
  
  .action-btn {
    width: 34px;
    height: 34px;
  }
  
  .input-footer {
    font-size: 0.9rem;
  }
}

/* 移动设备 (≤768px) */
@media (max-width: 768px) {
  .message-input-container {
    padding-bottom: 80px; /* 为底部导航栏留出空间 */
  }
  
  .input-wrapper {
    padding: 0.75rem 1rem;
  }
  
  .message-input-container {
    padding: 0.75rem;
  }
  
  .message-textarea {
    font-size: 0.9rem;
    padding: 0.75rem 1rem;
  }
  
  .send-button {
    width: 36px;
    height: 36px;
  }
  
  .action-btn {
    width: 32px;
    height: 32px;
  }
  
  .input-footer {
    font-size: 0.85rem;
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }
  
  .footer-text {
    order: 2;
  }
  
  .model-info {
    order: 1;
  }
}

/* 小屏手机 (≤480px) */
@media (max-width: 480px) {
  .message-input-container {
    padding-bottom: 70px; /* 为底部导航栏留出空间 */
  }
  
  .input-wrapper {
    padding: 0.5rem 0.75rem;
  }
  
  .message-input-container {
    padding: 0.5rem;
  }
  
  .message-textarea {
    font-size: 0.85rem;
    padding: 0.5rem 0.75rem;
  }
  
  .send-button {
    width: 32px;
    height: 32px;
  }
  
  .input-actions {
    gap: 0.5rem;
  }
  
  .action-btn {
    width: 32px;
    height: 32px;
  }
  
  .input-footer {
    font-size: 0.8rem;
    gap: 0.25rem;
  }
  
  .footer-icon {
    font-size: 0.9rem;
  }
}
</style>