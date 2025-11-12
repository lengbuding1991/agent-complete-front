<template>
  <div class="sidebar" :class="{ 'dark': darkMode }">
    <div class="sidebar-header">
      <!-- 桌面端显示 -->
      <button class="new-chat-btn" @click="$emit('new-chat')">
        <span class="plus-icon">+</span>
        <span class="btn-text">新对话</span>
      </button>
      
      <!-- 移动端对话切换按钮 -->
      <button class="conversation-switch-btn" @click="showMobileConversations = !showMobileConversations">
        <span class="conversation-switch-icon">💬</span>
        <span class="conversation-switch-text">对话</span>
      </button>
    </div>
    
    <div class="conversation-list">
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
    
    <div class="sidebar-footer">
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
      <div class="user-info" @click="showLoginModal = true">
        <div class="avatar">
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M16.6667 17.5V15.8333C16.6667 14.9493 16.3155 14.1014 15.6904 13.4763C15.0653 12.8512 14.2174 12.5 13.3333 12.5H6.66667C5.78261 12.5 4.93477 12.8512 4.30964 13.4763C3.68452 14.1014 3.33333 14.9493 3.33333 15.8333V17.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M10 9.16667C11.8409 9.16667 13.3333 7.67428 13.3333 5.83333C13.3333 3.99238 11.8409 2.5 10 2.5C8.15905 2.5 6.66667 3.99238 6.66667 5.83333C6.66667 7.67428 8.15905 9.16667 10 9.16667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <span class="username">登录</span>
      </div>
    </div>
    
    <!-- 移动端对话列表弹窗 -->
    <div v-if="isMobile" class="mobile-conversation-modal" :class="{ active: showMobileConversations }" @click="showMobileConversations = false">
      <div class="mobile-conversation-list" @click.stop>
        <div class="mobile-conversation-header">
          <h3 class="mobile-conversation-title">对话列表</h3>
          <button class="mobile-close-btn" @click="showMobileConversations = false">×</button>
        </div>
        
        <div 
          v-for="conversation in conversations" 
          :key="conversation.id"
          class="mobile-conversation-item"
          :class="{ active: conversation.id === activeConversation }"
          @click="$emit('select-conversation', conversation.id); showMobileConversations = false"
        >
          <div class="mobile-conversation-content">
            <span class="mobile-conversation-icon">
              <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
                <path d="M14 4.66667V11.3333C14 11.7015 13.7015 12 13.3333 12H4.66667C4.29848 12 4 11.7015 4 11.3333V4.66667C4 4.29848 4.29848 4 4.66667 4H13.3333C13.7015 4 14 4.29848 14 4.66667Z" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M2 4.66667V11.3333C2 11.7015 2.29848 12 2.66667 12H4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M4 8H14" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </span>
            <span class="mobile-conversation-title-text">{{ conversation.title }}</span>
          </div>
        </div>
        
        <!-- 如果没有对话，显示提示 -->
        <div v-if="conversations.length === 0" class="mobile-conversation-item">
          <div class="mobile-conversation-content">
            <span class="mobile-conversation-title-text">暂无对话</span>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 登录/注册弹窗 -->
    <div v-if="showLoginModal" class="login-modal" @click="showLoginModal = false">
      <div class="login-modal-content" @click.stop>
        <div class="login-modal-header">
          <h3 class="login-modal-title">{{ isLogin ? '登录' : '注册' }}</h3>
          <button class="login-close-btn" @click="showLoginModal = false">×</button>
        </div>
        
        <div class="login-modal-body">
          <!-- 登录表单 -->
          <form v-if="isLogin" @submit.prevent="handleLogin" class="login-form">
            <div class="form-group">
              <label for="email">邮箱</label>
              <input 
                id="email" 
                type="email" 
                v-model="loginForm.email" 
                placeholder="请输入邮箱"
                required
              >
            </div>
            <div class="form-group">
              <label for="password">密码</label>
              <input 
                id="password" 
                type="password" 
                v-model="loginForm.password" 
                placeholder="请输入密码"
                required
              >
            </div>
            <button type="submit" class="login-btn">登录</button>
            <p class="switch-text">
              还没有账号？
              <span class="switch-link" @click="isLogin = false">立即注册</span>
            </p>
          </form>
          
          <!-- 注册表单 -->
          <form v-else @submit.prevent="handleRegister" class="register-form">
            <div class="form-group">
              <label for="username">用户名</label>
              <input 
                id="username" 
                type="text" 
                v-model="registerForm.username" 
                placeholder="请输入用户名"
                required
              >
            </div>
            <div class="form-group">
              <label for="reg-email">邮箱</label>
              <input 
                id="reg-email" 
                type="email" 
                v-model="registerForm.email" 
                placeholder="请输入邮箱"
                required
              >
            </div>
            <div class="form-group">
              <label for="reg-password">密码</label>
              <input 
                id="reg-password" 
                type="password" 
                v-model="registerForm.password" 
                placeholder="请输入密码（至少6位）"
                required
                minlength="6"
              >
            </div>
            <div class="form-group">
              <label for="confirm-password">确认密码</label>
              <input 
                id="confirm-password" 
                type="password" 
                v-model="registerForm.confirmPassword" 
                placeholder="请再次输入密码"
                required
              >
            </div>
            <button type="submit" class="register-btn">注册</button>
            <p class="switch-text">
              已有账号？
              <span class="switch-link" @click="isLogin = true">立即登录</span>
            </p>
          </form>
        </div>
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
      showMobileConversations: false,
      windowWidth: window.innerWidth,
      showLoginModal: false,
      isLogin: true,
      loginForm: {
        email: '',
        password: ''
      },
      registerForm: {
        username: '',
        email: '',
        password: '',
        confirmPassword: ''
      }
    }
  },
  computed: {
    isMobile() {
      return this.windowWidth <= 768;
    }
  },
  mounted() {
    window.addEventListener('resize', this.handleResize);
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.handleResize);
  },
  methods: {
    handleResize() {
      this.windowWidth = window.innerWidth;
    },
    handleLogin() {
      // 模拟登录逻辑
      if (this.loginForm.email && this.loginForm.password) {
        console.log('登录成功:', this.loginForm.email);
        this.showLoginModal = false;
        this.$emit('user-login', { email: this.loginForm.email });
        // 清空表单
        this.loginForm = { email: '', password: '' };
      }
    },
    handleRegister() {
      // 验证密码是否匹配
      if (this.registerForm.password !== this.registerForm.confirmPassword) {
        alert('密码不匹配，请重新输入');
        return;
      }
      
      // 验证密码长度
      if (this.registerForm.password.length < 6) {
        alert('密码长度至少6位');
        return;
      }
      
      // 模拟注册逻辑
      if (this.registerForm.username && this.registerForm.email && this.registerForm.password) {
        console.log('注册成功:', this.registerForm);
        alert('注册成功！请登录');
        this.isLogin = true;
        // 清空表单
        this.registerForm = { username: '', email: '', password: '', confirmPassword: '' };
      }
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

/* 电脑端隐藏对话切换按钮 */
.conversation-switch-btn {
  display: none;
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

/* 移动设备 (≤768px) - 底部导航栏模式 */
@media (max-width: 768px) {
  .sidebar {
    width: 100vw;
    height: 60px;
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 1000;
    border-right: none;
    border-top: 1px solid #e5e5e5;
    padding: 0;
  }
  
  .sidebar.dark {
    border-top-color: #565869;
  }
  
  .sidebar-collapsed {
    width: 100vw;
  }
  
  .sidebar-header {
    padding: 0;
    border-bottom: none;
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 100%;
    flex: 1;
  }
  
  .new-chat-btn {
    width: 48px;
    height: 48px;
    padding: 0;
    border: none;
    background: transparent;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 2px;
  }
  
  .plus-icon {
    font-size: 1.5rem;
    font-weight: bold;
  }
  
  .btn-text {
    display: block;
    font-size: 0.7rem;
    line-height: 1;
  }
  
  .collapse-btn {
    display: none;
  }
  
  .conversation-list {
    display: none;
  }
  
  .sidebar-footer {
    display: none;
  }
  
  .footer-text {
    display: none;
  }
  
  .username {
    display: none;
  }
  
  /* 移动端底部导航栏样式 */
  .sidebar-header::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 100%;
    background: inherit;
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }
  
  .new-chat-btn {
    position: relative;
    z-index: 1;
  }
  
  /* 移动端对话切换按钮 */
  .conversation-switch-btn {
    width: 48px;
    height: 48px;
    padding: 0;
    border: none;
    background: transparent;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    gap: 2px;
    position: relative;
    z-index: 1;
    cursor: pointer;
  }
  
  .new-chat-btn .btn-text {
    display: none; /* 移动端隐藏新对话按钮文字 */
  }
  
  /* 移动端显示对话切换按钮 */
  .conversation-switch-btn {
    display: flex;
  }
  
  .conversation-switch-icon {
    font-size: 1.25rem;
    font-weight: bold;
  }
  
  .conversation-switch-text {
    display: block;
    font-size: 0.7rem;
    line-height: 1;
  }
  
  /* 移动端对话列表弹窗 */
  .mobile-conversation-modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 60px;
    background: rgba(0, 0, 0, 0.5);
    z-index: 999;
    display: none;
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
  }
  
  .mobile-conversation-modal.active {
    display: block;
  }
  
  .mobile-conversation-list {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    max-height: 60vh;
    background: #ffffff;
    border-radius: 1rem 1rem 0 0;
    overflow-y: auto;
    padding: 1rem;
    transform: translateY(100%);
    transition: transform 0.3s ease;
  }
  
  .dark .mobile-conversation-list {
    background: #202123;
  }
  
  .mobile-conversation-modal.active .mobile-conversation-list {
    transform: translateY(0);
  }
  
  .mobile-conversation-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1rem;
    padding-bottom: 0.5rem;
    border-bottom: 1px solid #e5e5e5;
  }
  
  .dark .mobile-conversation-header {
    border-bottom-color: #565869;
  }
  
  .mobile-conversation-title {
    font-size: 1.1rem;
    font-weight: 600;
  }
  
  .mobile-close-btn {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: #6b7280;
  }
  
  .mobile-conversation-item {
    padding: 0.75rem;
    border-radius: 0.5rem;
    margin-bottom: 0.5rem;
    cursor: pointer;
    transition: background 0.2s;
    border: 1px solid transparent;
  }
  
  .mobile-conversation-item:hover {
    background: #f3f4f6;
  }
  
  .dark .mobile-conversation-item:hover {
    background: #343541;
  }
  
  .mobile-conversation-item.active {
    background: #e5e7eb;
    border-color: #d1d5db;
  }
  
  .dark .mobile-conversation-item.active {
    background: #40414f;
    border-color: #565869;
  }
  
  .mobile-conversation-content {
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }
  
  .mobile-conversation-icon {
    flex-shrink: 0;
  }
  
  .mobile-conversation-title-text {
    font-size: 0.9rem;
    font-weight: 500;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}

/* 小屏手机 (≤480px) */
@media (max-width: 480px) {
  .sidebar {
    height: 56px;
  }
  
  .new-chat-btn {
    width: 44px;
    height: 44px;
  }
  
  .plus-icon {
    font-size: 1.25rem;
  }
  
  .btn-text {
    font-size: 0.65rem;
  }
}

/* 登录/注册弹窗样式 */
.login-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
}

.login-modal-content {
  background: #ffffff;
  border-radius: 1rem;
  width: 90%;
  max-width: 400px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  animation: modalSlideIn 0.3s ease;
}

.dark .login-modal-content {
  background: #202123;
  color: #ffffff;
}

@keyframes modalSlideIn {
  from {
    opacity: 0;
    transform: translateY(-20px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.login-modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem 1.5rem 1rem;
  border-bottom: 1px solid #e5e5e5;
}

.dark .login-modal-header {
  border-bottom-color: #565869;
}

.login-modal-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0;
}

.login-close-btn {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: #6b7280;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
}

.login-close-btn:hover {
  background: #f3f4f6;
}

.dark .login-close-btn:hover {
  background: #343541;
}

.login-modal-body {
  padding: 1.5rem;
}

.form-group {
  margin-bottom: 1rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
  color: #374151;
}

.dark .form-group label {
  color: #d1d5db;
}

.form-group input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #d1d5db;
  border-radius: 0.5rem;
  font-size: 0.875rem;
  transition: border-color 0.2s, box-shadow 0.2s;
  background: #ffffff;
  color: #1a1a1a;
}

.form-group input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.dark .form-group input {
  background: #343541;
  border-color: #565869;
  color: #ffffff;
}

.dark .form-group input:focus {
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.2);
}

.login-btn, .register-btn {
  width: 100%;
  padding: 0.75rem;
  background: #3b82f6;
  color: #ffffff;
  border: none;
  border-radius: 0.5rem;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s;
  margin-bottom: 1rem;
}

.login-btn:hover, .register-btn:hover {
  background: #2563eb;
}

.switch-text {
  text-align: center;
  font-size: 0.875rem;
  color: #6b7280;
  margin: 0;
}

.dark .switch-text {
  color: #d1d5db;
}

.switch-link {
  color: #3b82f6;
  cursor: pointer;
  text-decoration: underline;
  transition: color 0.2s;
}

.switch-link:hover {
  color: #2563eb;
}

/* 移动端适配 */
@media (max-width: 768px) {
  .login-modal-content {
    width: 95%;
    margin: 1rem;
  }
  
  .login-modal-header {
    padding: 1rem 1rem 0.75rem;
  }
  
  .login-modal-body {
    padding: 1rem;
  }
}
</style>