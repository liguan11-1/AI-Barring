<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI调酒师</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
      background-color: #121212;
      overflow: hidden;
      position: relative;
      height: 100vh;
    }
    
    .floating-text {
      position: absolute;
      color: rgba(255, 255, 255, 0.9);
      font-size: 1.5rem;
      font-weight: bold;
      text-shadow: 0 0 10px rgba(52, 152, 219, 0.9), 0 0 20px rgba(46, 204, 113, 0.7);
      animation: float 15s linear infinite;
      white-space: nowrap;
      opacity: 0;
      z-index: 10;
      letter-spacing: 1px;
      top: 5%; /* 让文字靠上显示 */
    }
    
    @keyframes float {
      0% {
        transform: translateY(0) translateX(0);
        opacity: 0;
      }
      5% {
        opacity: 1;
      }
      25% { /* 保持显示3秒左右 */
        opacity: 1;
      }
      30% {
        opacity: 0;
      }
      100% {
        transform: translateY(50px) translateX(100px);
        opacity: 0;
      }
    }
    
    .mobile-container {
      max-width: 480px;
      margin: 0 auto;
      background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
      height: 100vh;
      box-shadow: 0 0 30px rgba(52, 152, 219, 0.5);
      position: relative;
      z-index: 5;
      overflow: hidden;
      border-radius: 20px;
      animation: container-float 6s ease-in-out infinite;
      display: flex;
      flex-direction: column;
    }
    
    @keyframes container-float {
      0%, 100% {
        transform: translateY(0);
      }
      50% {
        transform: translateY(-10px);
      }
    }
    
    .app-header {
      text-align: center;
      padding: 15px 0;  /* 减小头部padding */
      color: white;
      font-size: 1.8rem;
      font-weight: bold;
      text-shadow: 0 0 10px rgba(52, 152, 219, 0.9), 0 0 20px rgba(46, 204, 113, 0.7);
      background: rgba(0, 0, 0, 0.3);
      border-bottom: 1px solid rgba(52, 152, 219, 0.5);
    }
    
    .phone-frame {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      background: linear-gradient(90deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 20%, rgba(255,255,255,0) 80%, rgba(255,255,255,0.1) 100%);
      z-index: 30;
      border-radius: 20px;
      box-shadow: inset 0 0 15px rgba(52, 152, 219, 0.3);
    }
    
    .phone-notch {
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 150px;
      height: 25px;
      background-color: #000;
      border-bottom-left-radius: 15px;
      border-bottom-right-radius: 15px;
      z-index: 31;
    }
    
    .bubbles {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 4;
      pointer-events: none;
    }
    
    .bubble {
      position: absolute;
      bottom: -50px;
      background-color: rgba(255, 255, 255, 0.1);
      border-radius: 50%;
      animation: bubble-rise linear infinite;
    }
    
    @keyframes bubble-rise {
      0% {
        transform: translateY(0) translateX(0);
        opacity: 0;
      }
      10% {
        opacity: 0.8;
      }
      90% {
        opacity: 0.4;
      }
      100% {
        transform: translateY(-100vh) translateX(20px);
        opacity: 0;
      }
    }
    
    .input-group {
      margin: 8px auto;  /* 减小输入框组的外边距 */
      width: 85%;
    }
    
    textarea {
      width: 100%;
      height: 65px;  /* 稍微减小文本框高度 */
      padding: 8px;  /* 减小内边距 */
      border: 2px solid rgba(52, 152, 219, 0.5);
      border-radius: 8px;
      resize: none;
      font-size: 15px;
      transition: all 0.3s ease;
      background: rgba(255, 255, 255, 0.9);
      font-family: 'Microsoft YaHei', 'PingFang SC', sans-serif;
    }
    
    textarea:focus {
      outline: none;
      border-color: #3498db;
      box-shadow: 0 0 15px rgba(52, 152, 219, 0.4);
    }
    
    .action-button {
      background: linear-gradient(45deg, #3498db, #2ecc71);
      color: white;
      padding: 8px 25px;  /* 减小按钮内边距 */
      border: none;
      border-radius: 20px;
      cursor: pointer;
      font-size: 16px;
      font-weight: bold;
      transition: all 0.3s ease;
      display: block;
      margin: 8px auto;  /* 减小按钮外边距 */
      box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    }
    
    .action-button:hover {
      transform: translateY(-3px) scale(1.05);
      box-shadow: 0 6px 20px rgba(52, 152, 219, 0.5);
    }
    
    .loading {
      display: none;
      text-align: center;
      margin: 8px 0;  /* 减小加载区域的间距 */
    }
    
    .loading-spinner {
      width: 35px;
      height: 35px;
      border: 3px solid rgba(255, 255, 255, 0.3);
      border-top: 3px solid #3498db;
      border-radius: 50%;
      animation: spin 1s linear infinite;
      margin: 8px auto;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    .response-content {
      background: rgba(255, 255, 255, 0.9);
      padding: 8px;  /* 减小内边距 */
      border-radius: 8px;
      margin: 8px auto;  /* 减小外边距 */
      width: 85%;
      display: flex;
      flex-direction: column;
      align-items: center;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }
    
    .cocktail-image {
      width: 100%;
      max-width: 280px;
      border-radius: 8px;
      margin: 0 auto 6px;  /* 减小底部间距 */
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
      object-fit: contain;
    }
    
    .download-button {
      display: inline-block;
      padding: 6px 20px;  /* 减小按钮内边距 */
      background: linear-gradient(45deg, #3498db, #2ecc71);
      color: #fff;
      border-radius: 20px;
      text-decoration: none;
      font-weight: bold;
      transition: all 0.3s ease;
      margin-top: 4px;  /* 减小顶部间距 */
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
      font-size: 14px;
    }
    
    .download-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 15px rgba(0,0,0,0.3);
    }
    
    @media (max-width: 480px) {
      .mobile-container {
        width: 100%;
        border-radius: 0;
        animation: none;
      }
      
      .phone-frame, .phone-notch {
        display: none;
      }
      
      .floating-text {
        font-size: 1.2rem;
      }
    }
    
    #coze-container {
      width: 100%;
      position: relative;
      z-index: 20;
      padding-top: 0;  /* 移除顶部padding */
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 25vh;  /* 减小顶部边距，从33vh改为25vh */
    }
    
    .subtitle {
      text-align: center;
      color: rgba(255, 255, 255, 0.8);
      font-size: 1rem;
      margin: 0 auto;
      line-height: 1.4;
      width: 85%;
      position: absolute;
      top: 80px;  /* 减小与顶部的距离 */
      left: 50%;
      transform: translateX(-50%);
    }
    
    .spacer {
      flex-grow: 1;
      min-height: 20px;
    }
  </style>
</head>
<body>
  <div class="mobile-container">
    <div class="phone-notch"></div>
    <div class="phone-frame"></div>
    <div class="app-header">AI调酒师</div>
    <div class="subtitle">你的私人调酒师，只需输入任何一句话，我将为您推荐最合适酒！</div>
    
    <div id="coze-container">
      <div class="input-group">
        <textarea id="input" placeholder="告诉我你的心情，或者描述你想要的鸡尾酒..."></textarea>
      </div>
      <button class="action-button" onclick="callCozeAPI()">开始调酒</button>
      
      <div class="loading" id="loading">
        <div class="loading-spinner"></div>
        <p style="color: white;">正在为您调制专属定制噢！...</p>
      </div>
      
      <div id="response"></div>
    </div>
    
    <div class="bubbles" id="bubbles"></div>
  </div>
  
  <script src="https://lf-cdn.coze.cn/obj/unpkg/flow-platform/chat-app-sdk/1.2.0-beta.10/libs/cn/index.js"></script>
  <script>
    document.addEventListener('DOMContentLoaded', function() {
      // 酒文化语录和网络热梗
      const wineQuotes = [
        "早 C 晚 A，不如早啤晚白",
        "微醺，才是生活顶配",
        "敬过往，干了这杯酒",
        "酒是生活的调味剂",
        "以酒为墨，写人生稿",
        "酒精，当代快乐开关",
        "酒杯一端，故事开讲",
        "小酌怡情，大酌也灵",
        "今夜，与酒坦诚相见",
        "用酒，敬生活的难",
        "醉翁之意，全在酒里",
        "把心事，泡进酒杯里",
        "喝酒，是灵魂的 SPA",
        "酒逢知己，千杯不腻",
        "有酒，平凡也有诗意",
        "举杯，敬这滚烫人间",
        "酒桌风云，情谊先行",
        "借酒，与自己和解",
        "美酒下肚，烦恼让路",
        "让酒，治愈疲惫日常"
      ];
      
      // 创建飘屏文字
      function createFloatingText() {
        const text = document.createElement('div');
        text.className = 'floating-text';
        text.textContent = wineQuotes[Math.floor(Math.random() * wineQuotes.length)];
        
        // 随机位置和动画时间
        const startX = Math.random() * window.innerWidth;
        
        text.style.left = `${startX}px`;
        text.style.animationDuration = `8s`; // 缩短动画时间以适应3秒显示
        
        // 随机颜色
        const hue = Math.floor(Math.random() * 360);
        text.style.color = `hsla(${hue}, 100%, 80%, 0.9)`;
        text.style.textShadow = `0 0 10px hsla(${hue}, 100%, 60%, 0.8), 0 0 20px hsla(${hue}, 100%, 40%, 0.6)`;
        
        document.body.appendChild(text);
        
        // 3秒后开始淡出
        setTimeout(() => {
          text.style.opacity = '0';
          text.style.transition = 'opacity 1s ease-out';
        }, 3000);
        
        // 动画结束后移除元素
        setTimeout(() => {
          document.body.removeChild(text);
        }, 8000);
      }
      
      // 创建气泡效果
      function createBubbles() {
        const bubblesContainer = document.getElementById('bubbles');
        
        for (let i = 0; i < 40; i++) {
          const bubble = document.createElement('div');
          bubble.className = 'bubble';
          
          // 随机大小和位置
          const size = 5 + Math.random() * 25;
          const left = Math.random() * 100;
          const duration = 8 + Math.random() * 12;
          const delay = Math.random() * 15;
          
          bubble.style.width = `${size}px`;
          bubble.style.height = `${size}px`;
          bubble.style.left = `${left}%`;
          bubble.style.animationDuration = `${duration}s`;
          bubble.style.animationDelay = `${delay}s`;
          
          // 随机透明度和颜色
          const opacity = 0.1 + Math.random() * 0.2;
          const hue = Math.floor(Math.random() * 360);
          bubble.style.backgroundColor = `hsla(${hue}, 100%, 70%, ${opacity})`;
          
          bubblesContainer.appendChild(bubble);
        }
      }
      
      // 每隔一段时间创建新的飘屏文字
      setInterval(createFloatingText, 2000); // 调整间隔以适应3秒显示
      
      // 初始创建几个飘屏文字
      for (let i = 0; i < 5; i++) {
        setTimeout(createFloatingText, i * 1000);
      }
      
      // 创建气泡效果
      createBubbles();
    });
    
    async function callCozeAPI() {
      const input = document.getElementById('input').value;
      const loading = document.getElementById('loading');
      const response = document.getElementById('response');

      if (!input) {
          alert('请告诉我，你想要什么样的鸡尾酒！');
          return;
      }

      loading.style.display = 'block';
      response.innerHTML = '';

      try {
          const apiResponse = await fetch('https://api.coze.cn/v1/workflow/run', {
              method: 'POST',
              headers: {
                  'Authorization': 'Bearer pat_K6QPZiAxDVh2CJHYxatf0RDzNt14VGC6886r71lO8cbK3jEN1XTtUZ8f6qkvanJF',
                  'Content-Type': 'application/json'
              },
              body: JSON.stringify({
                  parameters: {
                      input: input
                  },
                  workflow_id: "7500239957405073420"
              })
          });

          const data = await apiResponse.json();
          
          // 只处理data字段，提取图片URL
          let imgUrl = null;
          try {
              // data.data 可能是字符串，需要解析
              let inner = data.data;
              if (typeof inner === 'string') {
                  inner = JSON.parse(inner);
              }
              // 尝试从 inner.data 中提取图片URL
              if (inner && inner.data) {
                  // 匹配URL
                  const urlMatch = inner.data.match(/https?:\/\/[^\s"']+/);
                  if (urlMatch) {
                      imgUrl = urlMatch[0];
                  }
              }
          } catch (e) {}
          
          if (imgUrl) {
              // 只展示图片和下载按钮
              document.getElementById('response').innerHTML = `
                  <div class="response-content">
                      <img src="${imgUrl}" class="cocktail-image" alt="AI 调酒图片">
                      <a href="${imgUrl}" download="专属调酒.png" class="download-button">下载图片</a>
                  </div>
              `;
          } else {
              document.getElementById('response').innerHTML = `<div class="response-content" style="color:#333;">未获取到图片链接</div>`;
          }
      } catch (error) {
          document.getElementById('response').innerHTML = `<div class="response-content" style="color:#333;">错误: ${error.message}</div>`;
      } finally {
          loading.style.display = 'none';
      }
    }
  </script>
</body>
</html> 
