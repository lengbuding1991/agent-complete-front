<template>
  <div class="chat-header" :class="{ 'dark': darkMode }">
    <div class="header-content">
      <div class="header-left">
        <button class="menu-btn" @click="$emit('toggle-sidebar')">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M2.5 5H17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M2.5 10H17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M2.5 15H17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
        </button>
        <div class="conversation-info">
          <h2 class="conversation-title">{{ conversation.title }}</h2>
          <div class="conversation-stats">
            <span class="message-count">{{ conversation.messages.length }} 条消息</span>
            <span class="separator">•</span>
            <span class="conversation-date">{{ formatDate(conversation.createdAt) }}</span>
          </div>
        </div>
      </div>
      <div class="header-actions">
        <button class="action-btn" title="共享对话">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M15 6.66667L10 1.66667L5 6.66667" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M10 1.66667V12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M17.5 12.5V17.5C17.5 17.7761 17.2761 18 17 18H3C2.72386 18 2.5 17.7761 2.5 17.5V12.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
        <button class="action-btn" title="设置">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M10 12.5C11.3807 12.5 12.5 11.3807 12.5 10C12.5 8.61929 11.3807 7.5 10 7.5C8.61929 7.5 7.5 8.61929 7.5 10C7.5 11.3807 8.61929 12.5 10 12.5Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M15.606 6.115C15.739 6.315 15.844 6.533 15.919 6.762L16.719 9.157C16.825 9.481 16.825 9.833 16.719 10.157L15.919 12.552C15.844 12.781 15.739 12.999 15.606 13.199L14.849 14.345C14.396 15.039 13.582 15.45 12.719 15.45H10.281C9.418 15.45 8.604 15.039 8.151 14.345L7.394 13.199C7.261 12.999 7.156 12.781 7.081 12.552L6.281 10.157C6.175 9.833 6.175 9.481 6.281 9.157L7.081 6.762C7.156 6.533 7.261 6.315 7.394 6.115L8.151 4.969C8.604 4.275 9.418 3.864 10.281 3.864H12.719C13.582 3.864 14.396 4.275 14.849 4.969L15.606 6.115Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChatHeader',
  props: {
    conversation: Object,
    darkMode: Boolean
  },
  emits: ['toggle-sidebar'],
  methods: {
    formatDate(date) {
      const now = new Date();
      const convDate = new Date(date);
      const diffTime = Math.abs(now - convDate);
      const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
      
      if (diffDays === 1) {
        return '今天';
      } else if (diffDays === 2) {
        return '昨天';
      } else if (diffDays <= 7) {
        return `${diffDays - 1} 天前`;
      } else {
        return convDate.toLocaleDateString('zh-CN');
      }
    }
  }
}
</script>

<style scoped>
.chat-header {
  background: #ffffff;
  border-bottom: 1px solid #e5e5e5;
  padding: 0.75rem 1rem;
  transition: background-color 0.3s, border-color 0.3s;
}

.chat-header.dark {
  background: #343541;
  border-bottom-color: #565869;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: none;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.menu-btn {
  width: 40px;
  height: 40px;
  border: none;
  background: transparent;
  color: #6b7280;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 0.375rem;
  transition: all 0.2s;
}

.menu-btn:hover {
  background: #f3f4f6;
  color: #1a1a1a;
}

.dark .menu-btn {
  color: #d1d5db;
}

.dark .menu-btn:hover {
  background: #40414f;
  color: #ffffff;
}

.conversation-info {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.conversation-title {
  font-size: 1.125rem;
  font-weight: 600;
  color: #1a1a1a;
  transition: color 0.3s;
}

.dark .conversation-title {
  color: #ffffff;
}

.conversation-stats {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #6b7280;
  transition: color 0.3s;
}

.dark .conversation-stats {
  color: #d1d5db;
}

.separator {
  font-size: 0.75rem;
}

.header-actions {
  display: flex;
  gap: 0.5rem;
}

.action-btn {
  width: 40px;
  height: 40px;
  border: 1px solid #e5e7eb;
  border-radius: 0.375rem;
  background: white;
  color: #6b7280;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.action-btn:hover {
  background: #f9fafb;
  color: #1a1a1a;
  border-color: #d1d5db;
}

.dark .action-btn {
  background: #40414f;
  border-color: #565869;
  color: #d1d5db;
}

.dark .action-btn:hover {
  background: #4a4b59;
  color: #ffffff;
  border-color: #6e6e80;
}

/* 平板设备 (769px - 1024px) */
@media (max-width: 1024px) and (min-width: 769px) {
  .chat-header {
    padding: 0.75rem 1rem;
  }
  
  .header-content {
    padding: 0;
  }
  
  .conversation-title {
    font-size: 1rem;
  }
  
  .conversation-stats {
    font-size: 0.8rem;
  }
  
  .action-btn {
    width: 36px;
    height: 36px;
  }
}

/* 移动设备 (≤768px) */
@media (max-width: 768px) {
  .chat-header {
    padding: 0.75rem 1rem;
    position: sticky;
    top: 0;
    z-index: 900;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }
  
  .chat-header.dark {
    background: rgba(52, 53, 65, 0.9);
  }
  
  .header-content {
    padding: 0;
  }
  
  .conversation-stats {
    display: none;
  }
  
  .header-actions {
    gap: 0.5rem;
  }
  
  .action-btn {
    width: 32px;
    height: 32px;
  }
  
  .menu-btn {
    width: 36px;
    height: 36px;
  }
}

/* 小屏手机 (≤480px) */
@media (max-width: 480px) {
  .chat-header {
    padding: 0.5rem 0.75rem;
  }
  
  .conversation-title {
    font-size: 0.9rem;
  }
  
  .header-actions {
    gap: 0.25rem;
  }
  
  .action-btn {
    width: 28px;
    height: 28px;
  }
  
  .menu-btn {
    width: 32px;
    height: 32px;
  }
  
  .action-btn svg,
  .menu-btn svg {
    width: 16px;
    height: 16px;
  }
}
 </style>