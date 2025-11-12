<template>
  <div class="sidebar" :class="{ 'sidebar-collapsed': collapsed, 'dark': darkMode }">
    <div class="sidebar-header">
      <button class="new-chat-btn" @click="$emit('new-chat')">
        <span class="plus-icon">+</span>
        <span class="btn-text">新对话</span>
      </button>
      <button class="collapse-btn" @click="collapsed = !collapsed">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <path d="M6 12L10 8L6 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>
    
    <div class="conversation-list" v-if="!collapsed">
      <div 
        v-for="conversation in conversations" 
        :key="conversation.id"
        class="conversation-item"
        :class="{ active: conversation.id === activeConversation }"
        @click="$emit('select-conversation', conversation.id)"
      >
        <div class="conversation-content">
          <span class="conversation-icon">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M14 4.66667V11.3333C14 11.7015 13.7015 12 13.3333 12H4.66667C4.29848 12 4 11.7015 4 11.3333V4.66667C4 4.29848 4.29848 4 4.66667 4H13.3333C13.7015 4 14 4.29848 14 4.66667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M2 4.66667V11.3333C2 11.7015 2.29848 12 2.66667 12H4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M4 8H14" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </span>
          <span class="conversation-title">{{ conversation.title }}</span>
        </div>
        <button 
          class="delete-btn"
          @click.stop="$emit('delete-conversation', conversation.id)"
        >
          <svg width="14" height="14" viewBox="0 0 14 14" fill="none">
            <path d="M10.3333 3.66667V2.33333C10.3333 1.59695 9.73638 1 9 1H5C4.26362 1 3.66667 1.59695 3.66667 2.33333V3.66667M1 3.66667H13M5.66667 6.33333V10.3333M8.33333 6.33333V10.3333M11.6667 3.66667V11.3333C11.6667 12.0697 11.0697 12.6667 10.3333 12.6667H3.66667C2.93029 12.6667 2.33333 12.0697 2.33333 11.3333V3.66667H11.6667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
    </div>
    
    <div class="sidebar-footer" v-if="!collapsed">
      <div class="footer-actions">
        <button class="footer-btn" @click="$emit('toggle-dark-mode')">
          <span class="footer-icon">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path d="M8 12C10.2091 12 12 10.2091 12 8C12 5.79086 10.2091 4 8 4C5.79086 4 4 5.79086 4 8C4 10.2091 5.79086 12 8 12Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M8 1V2.33333" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M8 13.6667V15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M2.8125 2.8125L3.8125 3.8125" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M12.1875 12.1875L13.1875 13.1875" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M1 8H2.33333" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M13.6667 8H15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M2.8125 13.1875L3.8125 12.1875" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              <path d="M12.1875 3.8125L13.1875 2.8125" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </span>
          <span class="footer-text">{{ darkMode ? '浅色模式' : '深色模式' }}</span>
        </button>
      </div>
      <div class="user-info">
        <div class="avatar">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M16.6667 17.5V15.8333C16.6667 14.9493 16.3155 14.1014 15.6904 13.4763C15.0653 12.8512 14.2174 12.5 13.3333 12.5H6.66667C5.78261 12.5 4.93477 12.8512 4.30964 13.4763C3.68452 14.1014 3.33333 14.9493 3.33333 15.8333V17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M10 9.16667C11.8409 9.16667 13.3333 7.67428 13.3333 5.83333C13.3333 3.99238 11.8409 2.5 10 2.5C8.15905 2.5 6.66667 3.99238 6.66667 5.83333C6.66667 7.67428 8.15905 9.16667 10 9.16667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <span class="username">用户</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    conversations: Array,
    activeConversation: Number,
    darkMode: Boolean
  },
  data() {
    return {
      collapsed: false
    }
  },
  emits: ['new-chat', 'select-conversation', 'delete-conversation', 'toggle-dark-mode']
}
</script>

<style scoped>
.sidebar {
  width: 260px;
  background: #f7f7f8;
  color: #1a1a1a;
  display: flex;
  flex-direction: column;
  transition: width 0.3s;
  height: 100vh;
  overflow: hidden;
  border-right: 1px solid #e5e5e5;
}

.sidebar.dark {
  background: #202123;
  color: #ffffff;
  border-right-color: #565869;
}

.sidebar-collapsed {
  width: 60px;
}

.sidebar-header {
  padding: 1rem;
  border-bottom: 1px solid #e5e5e5;
  position: relative;
}

.dark .sidebar-header {
  border-bottom-color: #565869;
}

.new-chat-btn {
  width: 100%;
  padding: 0.75rem;
  background: transparent;
  border: 1px solid #d1d5db;
  border-radius: 0.5rem;
  color: #1a1a1a;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.2s;
  font-size: 0.875rem;
}

.new-chat-btn:hover {
  background: #f3f4f6;
}

.dark .new-chat-btn {
  border-color: #565869;
  color: #ffffff;
}

.dark .new-chat-btn:hover {
  background: #343541;
}

.plus-icon {
  font-size: 1.125rem;
  font-weight: bold;
}

.collapse-btn {
  position: absolute;
  top: 50%;
  right: -12px;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
  background: #ffffff;
  border: 1px solid #d1d5db;
  border-radius: 50%;
  color: #6b7280;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  transition: all 0.2s;
}

.dark .collapse-btn {
  background: #40414f;
  border-color: #565869;
  color: #d1d5db;
}

.collapse-btn:hover {
  background: #f3f4f6;
  color: #1a1a1a;
}

.dark .collapse-btn:hover {
  background: #4a4b59;
  color: #ffffff;
}

.conversation-list {
  flex: 1;
  overflow-y: auto;
  padding: 0.5rem;
}

.conversation-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0.75rem;
  border-radius: 0.375rem;
  cursor: pointer;
  margin-bottom: 0.125rem;
  transition: background 0.2s;
}

.conversation-item:hover {
  background: #f3f4f6;
}

.conversation-item.active {
  background: #e5e7eb;
}

.dark .conversation-item:hover {
  background: #343541;
}

.dark .conversation-item.active {
  background: #40414f;
}

.conversation-content {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex: 1;
  overflow: hidden;
}

.conversation-icon {
  color: #6b7280;
  flex-shrink: 0;
}

.dark .conversation-icon {
  color: #d1d5db;
}

.conversation-title {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 0.875rem;
}

.delete-btn {
  opacity: 0;
  background: none;
  border: none;
  color: #6b7280;
  cursor: pointer;
  padding: 0.25rem;
  border-radius: 0.25rem;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.conversation-item:hover .delete-btn {
  opacity: 1;
}

.delete-btn:hover {
  background: #e5e7eb;
  color: #ef4444;
}

.dark .delete-btn:hover {
  background: #565869;
  color: #f87171;
}

.sidebar-footer {
  padding: 1rem;
  border-top: 1px solid #e5e5e5;
}

.dark .sidebar-footer {
  border-top-color: #565869;
}

.footer-actions {
  margin-bottom: 1rem;
}

.footer-btn {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem;
  background: transparent;
  border: none;
  border-radius: 0.375rem;
  color: #6b7280;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 0.875rem;
}

.footer-btn:hover {
  background: #f3f4f6;
  color: #1a1a1a;
}

.dark .footer-btn {
  color: #d1d5db;
}

.dark .footer-btn:hover {
  background: #343541;
  color: #ffffff;
}

.footer-icon {
  display: flex;
  align-items: center;
  justify-content: center;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem;
  border-radius: 0.375rem;
  transition: background 0.2s;
}

.user-info:hover {
  background: #f3f4f6;
}

.dark .user-info:hover {
  background: #343541;
}

.avatar {
  width: 32px;
  height: 32px;
  background: #e5e7eb;
  border-radius: 0.25rem;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #6b7280;
}

.dark .avatar {
  background: #40414f;
  color: #d1d5db;
}

.username {
  font-size: 0.875rem;
  font-weight: 500;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .sidebar {
    position: absolute;
    z-index: 100;
    transform: translateX(-100%);
  }
  
  .sidebar-collapsed {
    transform: translateX(0);
    width: 60px;
  }
  
  .sidebar:not(.sidebar-collapsed) {
    transform: translateX(0);
    width: 260px;
  }
  
  .btn-text {
    display: none;
  }
  
  .footer-text {
    display: none;
  }
  
  .username {
    display: none;
  }
}
</style>