<template>
  <div id="app" :class="{ 'dark': darkMode }">
    <Sidebar 
      :conversations="conversations" 
      :activeConversation="activeConversationId"
      @new-chat="startNewChat"
      @select-conversation="selectConversation"
      @delete-conversation="deleteConversation"
      :darkMode="darkMode"
      @toggle-dark-mode="toggleDarkMode"
    />
    <div class="main-content">
      <ChatHeader 
        v-if="activeConversation"
        :conversation="activeConversation"
        :darkMode="darkMode"
      />
      <ChatArea 
        v-if="activeConversation"
        :messages="activeConversation.messages"
        :thinking="thinking"
        :darkMode="darkMode"
      />
      <MessageInput 
        @send-message="sendMessage"
        :thinking="thinking"
        :darkMode="darkMode"
      />
      
      <!-- 空状态 -->
      <div v-if="!activeConversation" class="empty-state" :class="{ 'dark': darkMode }">
        <div class="empty-content">
          <div class="logo">
            <svg width="40" height="40" viewBox="0 0 40 40" fill="none">
              <path d="M20 0C8.954 0 0 8.954 0 20C0 31.046 8.954 40 20 40C31.046 40 40 31.046 40 20C40 8.954 31.046 0 20 0Z" fill="#1A1A1A"/>
              <path d="M25.5 15.5L20 10L14.5 15.5L20 21L25.5 15.5Z" fill="white"/>
              <path d="M14.5 24.5L20 30L25.5 24.5L20 19L14.5 24.5Z" fill="white"/>
            </svg>
          </div>
          <h1>DeepSeek</h1>
          <p>你好！我是DeepSeek，一个AI助手，很高兴为你服务！</p>
          <p>请问有什么可以帮助你的吗？</p>
          
          <div class="suggestions">
            <h3>你可以尝试问我：</h3>
            <div class="suggestion-chips">
              <div class="chip" @click="sendSuggestion('帮我写一个Python函数来计算斐波那契数列')">
                <span class="chip-icon">💻</span>
                <span class="chip-text">帮我写一个Python函数来计算斐波那契数列</span>
              </div>
              <div class="chip" @click="sendSuggestion('解释一下量子计算的基本原理')">
                <span class="chip-icon">🔬</span>
                <span class="chip-text">解释一下量子计算的基本原理</span>
              </div>
              <div class="chip" @click="sendSuggestion('如何提高我的React应用性能？')">
                <span class="chip-icon">⚡</span>
                <span class="chip-text">如何提高我的React应用性能？</span>
              </div>
              <div class="chip" @click="sendSuggestion('制定一个学习机器学习的计划')">
                <span class="chip-icon">📚</span>
                <span class="chip-text">制定一个学习机器学习的计划</span>
              </div>
            </div>
          </div>
          
          <div class="capabilities">
            <h3>我的能力：</h3>
            <div class="capability-list">
              <div class="capability-item">
                <span class="capability-icon">📝</span>
                <span>文本生成与编辑</span>
              </div>
              <div class="capability-item">
                <span class="capability-icon">💡</span>
                <span>问题解答</span>
              </div>
              <div class="capability-item">
                <span class="capability-icon">🔍</span>
                <span>联网搜索（需手动开启）</span>
              </div>
              <div class="capability-item">
                <span class="capability-icon">📊</span>
                <span>数据分析</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from './components/Sidebar.vue'
import ChatHeader from './components/ChatHeader.vue'
import ChatArea from './components/ChatArea.vue'
import MessageInput from './components/MessageInput.vue'

export default {
  name: 'App',
  components: {
    Sidebar,
    ChatHeader,
    ChatArea,
    MessageInput
  },
  data() {
    return {
      conversations: [],
      activeConversationId: null,
      thinking: false,
      conversationIdCounter: 1,
      darkMode: false
    }
  },
  computed: {
    activeConversation() {
      return this.conversations.find(c => c.id === this.activeConversationId)
    }
  },
  methods: {
    startNewChat() {
      const newConversation = {
        id: this.conversationIdCounter++,
        title: '新对话',
        messages: [],
        createdAt: new Date()
      }
      this.conversations.unshift(newConversation)
      this.activeConversationId = newConversation.id
    },
    selectConversation(id) {
      this.activeConversationId = id
    },
    deleteConversation(id) {
      const index = this.conversations.findIndex(c => c.id === id)
      if (index !== -1) {
        this.conversations.splice(index, 1)
        if (this.activeConversationId === id) {
          this.activeConversationId = this.conversations.length > 0 ? this.conversations[0].id : null
        }
      }
    },
    async sendMessage(message) {
      if (!this.activeConversation) {
        this.startNewChat()
      }
      
      // 添加用户消息
      const userMessage = {
        id: Date.now(),
        content: message,
        sender: 'user',
        timestamp: new Date()
      }
      
      this.activeConversation.messages.push(userMessage)
      
      // 更新对话标题（如果是第一条消息）
      if (this.activeConversation.messages.length === 1) {
        this.activeConversation.title = message.length > 20 
          ? message.substring(0, 20) + '...' 
          : message
      }
      
      // 模拟AI思考
      this.thinking = true
      
      // 模拟AI响应
      setTimeout(() => {
        const aiMessage = {
          id: Date.now() + 1,
          content: this.generateAIResponse(message),
          sender: 'assistant',
          timestamp: new Date()
        }
        
        this.activeConversation.messages.push(aiMessage)
        this.thinking = false
      }, 1500)
    },
    sendSuggestion(suggestion) {
      this.sendMessage(suggestion)
    },
    generateAIResponse(userMessage) {
      // 根据用户消息生成更自然的回复
      if (userMessage.includes('Python') || userMessage.includes('斐波那契')) {
        return `当然，我可以帮你写一个计算斐波那契数列的Python函数。以下是几种不同的实现方式：

\`\`\`python
def fibonacci(n):
    """
    计算斐波那契数列的前n项
    
    参数:
    n -- 要计算的项数
    
    返回:
    斐波那契数列列表
    """
    if n <= 0:
        return []
    elif n == 1:
        return [0]
    elif n == 2:
        return [0, 1]
    
    fib_sequence = [0, 1]
    for i in range(2, n):
        next_fib = fib_sequence[i-1] + fib_sequence[i-2]
        fib_sequence.append(next_fib)
    
    return fib_sequence

# 使用示例
print(fibonacci(10))  # 输出: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
\`\`\`

这个函数的时间复杂度是O(n)，空间复杂度也是O(n)。如果你需要更高效的实现，我还可以提供使用生成器或动态规划的方法。`
      }
      
      if (userMessage.includes('量子计算')) {
        return `量子计算是一种基于量子力学原理的全新计算范式，它与传统计算机有着根本性的不同。让我为你详细解释一下：

## 基本概念

**量子比特 (Qubit)**：
- 传统比特只能是0或1
- 量子比特可以同时处于0和1的叠加状态
- 用数学表示为：|ψ⟩ = α|0⟩ + β|1⟩，其中α和β是复数

**量子叠加**：
- 量子系统可以同时存在于多个状态
- 这使得量子计算机能够并行处理大量计算

**量子纠缠**：
- 两个或多个量子比特可以形成纠缠态
- 改变一个量子比特的状态会立即影响另一个
- 爱因斯坦称之为"鬼魅般的超距作用"

**量子干涉**：
- 量子状态可以相互增强或抵消
- 通过精心设计的算法放大正确答案的概率

## 实际应用

量子计算机在以下领域有巨大潜力：
- 密码学（Shor算法破解RSA加密）
- 药物发现和材料科学
- 优化问题和机器学习
- 金融建模

目前量子计算还处于发展初期，但已经显示出在某些特定问题上远超经典计算机的潜力。`
      }
      
      if (userMessage.includes('React') || userMessage.includes('性能')) {
        return `提高React应用性能是一个很重要的话题！以下是一些最有效的优化策略：

## 1. 使用React.memo防止不必要的重渲染
\`\`\`jsx
const MyComponent = React.memo(({ data }) => {
  return <div>{data}</div>;
});
\`\`\`

## 2. 合理使用useCallback和useMemo
\`\`\`jsx
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);

const handleClick = useCallback(() => {
  // 处理点击
}, [dependencies]);
\`\`\`

## 3. 代码分割和懒加载
\`\`\`jsx
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function MyComponent() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
\`\`\`

## 4. 虚拟化长列表
使用react-window或react-virtualized来处理大量数据。

## 5. 优化Bundle大小
- 使用Webpack Bundle Analyzer分析包大小
- 按需引入第三方库
- 使用Tree Shaking

你目前遇到的具体性能问题是什么？我可以提供更有针对性的建议。`
      }
      
      if (userMessage.includes('机器学习') || userMessage.includes('学习计划')) {
        return `制定机器学习学习计划是个很好的开始！我为你设计了一个系统性的学习路线：

## 🗓️ 机器学习学习计划（6个月）

### 第一阶段：基础准备（1个月）
**数学基础：**
- 线性代数：矩阵运算、特征值、奇异值分解
- 概率统计：概率分布、假设检验、贝叶斯定理
- 微积分：导数、梯度、优化方法

**编程基础：**
- Python编程
- NumPy, Pandas, Matplotlib
- Jupyter Notebook使用

### 第二阶段：机器学习核心（2个月）
**监督学习：**
- 线性回归、逻辑回归
- 决策树、随机森林
- 支持向量机(SVM)
- k近邻算法

**无监督学习：**
- 聚类算法(K-means, DBSCAN)
- 降维技术(PCA, t-SNE)

**模型评估：**
- 交叉验证
- 评估指标(准确率、精确率、召回率等)

### 第三阶段：深度学习（2个月）
- 神经网络基础
- 卷积神经网络(CNN)
- 循环神经网络(RNN/LSTM)
- Transformer架构

### 第四阶段：实践项目（1个月）
- 参加Kaggle比赛
- 构建个人项目
- 学习模型部署

## 📚 推荐资源
- 课程：吴恩达机器学习、Fast.ai
- 书籍：《Python机器学习》、《深度学习》
- 实践平台：Kaggle, Colab, Hugging Face

记住，理论学习要与实践相结合，多做项目才能真正掌握！`
      }
      
      // 默认回复
      return `感谢你的提问！${userMessage}

我是一个AI助手，专门设计来帮助用户解答问题、提供信息和协助完成各种任务。我可以帮你：

✨ **内容创作**：写作、翻译、总结等
🔍 **信息查询**：回答问题、提供解释
💻 **技术支持**：编程帮助、技术指导
📊 **数据分析**：处理和分析数据
📝 **学习辅助**：制定计划、解释概念

请告诉我更多关于你的具体需求，这样我可以提供更精准的帮助。`
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode
    }
  },
  mounted() {
    // 初始化一个空对话
    this.startNewChat()
  }
}
</script>

<style scoped>
.main-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    width: 100%;
    overflow: hidden;
    position: relative;
    min-width: 0; /* 防止flex项目溢出 */
  }

  .empty-state {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #ffffff;
    transition: background-color 0.3s;
    min-height: 100vh; /* 确保在移动端有足够高度 */
  }

  .empty-state.dark {
    background: #343541;
  }

  .empty-content {
    text-align: center;
    max-width: 600px;
    padding: 2rem;
    width: 100%;
  }

.logo {
  margin-bottom: 1.5rem;
}

.empty-content h1 {
  font-size: 2rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #1a1a1a;
  transition: color 0.3s;
}

.dark .empty-content h1 {
  color: #ffffff;
}

.empty-content p {
  font-size: 1.125rem;
  color: #6b7280;
  margin-bottom: 0.5rem;
  transition: color 0.3s;
}

.dark .empty-content p {
  color: #d1d5db;
}

.suggestions {
  margin: 3rem 0;
}

.suggestions h3 {
  font-size: 1.125rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #1a1a1a;
  transition: color 0.3s;
}

.dark .suggestions h3 {
  color: #ffffff;
}

.suggestion-chips {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.75rem;
  max-width: 600px;
  margin: 0 auto;
}

.chip {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1rem;
  background: #f9fafb;
  border: 1px solid #e5e7eb;
  border-radius: 0.75rem;
  cursor: pointer;
  transition: all 0.2s;
  text-align: left;
}

.chip:hover {
  background: #f3f4f6;
  border-color: #d1d5db;
}

.dark .chip {
  background: #40414f;
  border-color: #565869;
  color: #ffffff;
}

.dark .chip:hover {
  background: #4a4b59;
}

.chip-icon {
  font-size: 1.125rem;
}

.chip-text {
  font-size: 0.875rem;
  line-height: 1.25;
}

.capabilities {
  margin-top: 2rem;
}

.capabilities h3 {
  font-size: 1.125rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: #1a1a1a;
  transition: color 0.3s;
}

.dark .capabilities h3 {
  color: #ffffff;
}

.capability-list {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
}

.capability-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #6b7280;
  transition: color 0.3s;
}

.dark .capability-item {
  color: #d1d5db;
}

.capability-icon {
  font-size: 1rem;
}

/* 平板设备 (769px - 1024px) */
@media (max-width: 1024px) and (min-width: 769px) {
  .suggestion-chips {
    grid-template-columns: repeat(2, 1fr);
    gap: 1rem;
  }
  
  .empty-content {
    max-width: 90%;
  }
  
  .capability-item {
    padding: 1rem;
  }
  
  .main-content {
    width: calc(100vw - 60px); /* 侧边栏折叠时的宽度 */
  }
}

/* 移动设备 (≤768px) */
@media (max-width: 768px) {
  #app {
    flex-direction: column;
    height: auto;
    min-height: 100vh;
  }
  
  .main-content {
    width: 100vw;
    min-height: calc(100vh - 60px); /* 减去侧边栏高度 */
  }
  
  .sidebar {
    width: 100vw;
    height: 60px;
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 1000;
    border-right: none;
    border-top: 1px solid #e5e5e5;
  }
  
  .sidebar.dark {
    border-top-color: #565869;
  }
  
  .sidebar-collapsed {
    width: 100vw;
  }
  
  .suggestion-chips {
    grid-template-columns: 1fr;
    gap: 0.75rem;
  }
  
  .capability-list {
    grid-template-columns: 1fr;
    gap: 0.75rem;
  }
  
  .empty-content h1 {
    font-size: 1.75rem;
  }
  
  .empty-content p {
    font-size: 1rem;
  }
  
  .empty-state {
    padding-bottom: 80px; /* 为底部导航栏留出空间 */
  }
}

/* 小屏手机 (≤480px) */
@media (max-width: 480px) {
  .empty-state {
    padding: 1rem;
    padding-bottom: 80px;
  }
  
  .empty-content {
    padding: 1.5rem 1rem;
  }
  
  .empty-content h1 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }
  
  .empty-content p {
    font-size: 0.9rem;
    margin-bottom: 0.75rem;
  }
  
  .suggestion-chips {
    gap: 0.5rem;
  }
  
  .chip {
    padding: 0.75rem;
    font-size: 0.9rem;
  }
  
  .chip-icon {
    font-size: 1rem;
  }
  
  .capability-list {
    gap: 0.5rem;
  }
  
  .capability-item {
    padding: 0.75rem;
    font-size: 0.9rem;
  }
  
  .capability-icon {
    font-size: 1.25rem;
  }
  
  .logo svg {
    width: 32px;
    height: 32px;
  }
}
</style>