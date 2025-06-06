<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI调酒师</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Press Start 2P', 'Microsoft YaHei', 'PingFang SC', sans-serif;
      background-color: #121212;
      overflow: hidden;
      position: relative;
      height: 100vh;
    }
    
    .floating-text {
      position: absolute;
      color: rgba(255, 255, 255, 0.9);
      font-size: 1.2rem;
      font-weight: normal;
      font-family: 'Press Start 2P', monospace;
      text-shadow: 2px 2px 0 rgba(52, 152, 219, 0.9);
      animation: float 15s steps(30) infinite;
      white-space: nowrap;
      opacity: 0;
      z-index: 10;
      letter-spacing: 1px;
      top: 5%;
      image-rendering: pixelated;
    }
    
    @keyframes float {
      0% {
        transform: translateY(0) translateX(0);
        opacity: 0;
      }
      5% {
        opacity: 1;
      }
      25% {
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
      border-radius: 0;
      display: flex;
      flex-direction: column;
    }
    
    .app-header {
      text-align: center;
      padding: 15px 0;
      margin-bottom: 10px;
    }
    
    .app-title {
      background-color: #2196F3;
      color: white;
      font-size: 1.5rem;
      font-weight: normal;
      padding: 8px 25px;
      border-radius: 0;
      display: inline-block;
      box-shadow: 4px 4px 0 rgba(33, 150, 243, 0.5);
      margin-bottom: 10px;
      font-family: 'Press Start 2P', monospace;
      text-transform: uppercase;
      image-rendering: pixelated;
    }
    
    .app-subtitle {
      display: flex;
      justify-content: center;
      margin-top: 10px;
    }
    
    .subtitle-pill {
      background-color: #2196F3;
      color: white;
      font-size: 0.8rem;
      font-weight: normal;
      padding: 5px 15px;
      border-radius: 0;
      margin: 0 5px;
      box-shadow: 2px 2px 0 rgba(33, 150, 243, 0.4);
      font-family: 'Press Start 2P', monospace;
      image-rendering: pixelated;
    }
    
    .cocktail-image {
      width: 156px;
      height: 195px;
      margin: 20px auto;
      background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="%23FFFFFF"><path d="M7.5,7L5.5,5H18.5L16.5,7M11,13V19H6V21H18V19H13V13L21,5V3H3V5L11,13Z"/></svg>');
      background-size: contain;
      background-repeat: no-repeat;
      background-position: center;
      position: relative;
      image-rendering: pixelated;
    }
    
    .cocktail-glass {
      width: 100%;
      height: 100%;
      background: linear-gradient(to bottom, #4CAF50 0%, #FF9800 30%, #F44336 100%);
      -webkit-mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M7.5,7L5.5,5H18.5L16.5,7M11,13V19H6V21H18V19H13V13L21,5V3H3V5L11,13Z"/></svg>');
      mask-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="M7.5,7L5.5,5H18.5L16.5,7M11,13V19H6V21H18V19H13V13L21,5V3H3V5L11,13Z"/></svg>');
      -webkit-mask-size: contain;
      -webkit-mask-repeat: no-repeat;
      -webkit-mask-position: center;
      mask-size: contain;
      mask-repeat: no-repeat;
      mask-position: center;
      image-rendering: pixelated;
    }
    
    .lime-slice {
      width: 39px;
      height: 39px;
      background-color: #8BC34A;
      border-radius: 50%;
      position: absolute;
      top: 13px;
      right: 33px;
      transform: rotate(-20deg);
      box-shadow: 0 0 5px rgba(139, 195, 74, 0.7);
      image-rendering: pixelated;
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
      border-radius: 0;
      animation: bubble-rise steps(20) infinite;
      border: 1px solid rgba(255, 255, 255, 0.2);
      image-rendering: pixelated;
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
    
    .input-container {
      width: 85%;
      margin: 0 auto;
      background-color: rgba(255, 255, 255, 0.9);
      border-radius: 0;
      padding: 10px;
      box-shadow: 4px 4px 0 rgba(0, 0, 0, 0.2);
      border: 2px solid #000;
      image-rendering: pixelated;
    }
    
    textarea {
      width: 100%;
      height: 65px;
      padding: 8px;
      border: 2px solid #000;
      border-radius: 0;
      resize: none;
      font-size: 14px;
      background: transparent;
      font-family: 'Press Start 2P', monospace;
      image-rendering: pixelated;
    }
    
    textarea:focus {
      outline: none;
    }
    
    .action-button {
      background: #00BFA5;
      color: white;
      padding: 12px 30px;
      border: 2px solid #008975;
      border-radius: 0;
      cursor: pointer;
      font-size: 16px;
      font-weight: normal;
      font-family: 'Press Start 2P', monospace;
      transition: all 0.2s ease;
      display: block;
      margin: 20px auto;
      box-shadow: 4px 4px 0 #008975;
      image-rendering: pixelated;
    }
    
    .action-button:hover {
      transform: translate(2px, 2px);
      box-shadow: 2px 2px 0 #008975;
    }
    
    .loading {
      display: none;
      text-align: center;
      margin: 15px 0;
    }
    
    .loading-spinner {
      width: 40px;
      height: 40px;
      border: 4px solid rgba(255, 255, 255, 0.3);
      border-top: 4px solid #00BFA5;
      border-radius: 0;
      animation: spin 1s steps(8) infinite;
      margin: 10px auto;
      image-rendering: pixelated;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    .response-content {
      background: rgba(255, 255, 255, 0.9);
      padding: 10px;
      border-radius: 0;
      margin: 15px auto;
      width: 85%;
      display: flex;
      flex-direction: column;
      align-items: center;
      box-shadow: 4px 4px 0 rgba(0, 0, 0, 0.2);
      border: 2px solid #000;
      image-rendering: pixelated;
    }
    
    .result-cocktail-image {
      width: 100%;
      max-width: 280px;
      border-radius: 0;
      margin: 0 auto 10px;
      box-shadow: 4px 4px 0 rgba(0, 0, 0, 0.2);
      border: 2px solid #000;
      object-fit: contain;
      image-rendering: pixelated;
    }
    
    .download-button {
      display: inline-block;
      padding: 8px 25px;
      background: #00BFA5;
      color: #fff;
      border: 2px solid #008975;
      border-radius: 0;
      text-decoration: none;
      font-weight: normal;
      font-family: 'Press Start 2P', monospace;
      transition: all 0.2s ease;
      margin-top: 8px;
      box-shadow: 4px 4px 0 #008975;
      font-size: 12px;
      image-rendering: pixelated;
    }
    
    .download-button:hover {
      transform: translate(2px, 2px);
      box-shadow: 2px 2px 0 #008975;
    }
    
    @media (max-width: 480px) {
      .mobile-container {
        width: 100%;
      }
      
      .floating-text {
        font-size: 1.2rem;
      }
    }
    
    #main-content {
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="mobile-container">
    <div class="app-header">
      <div class="app-title">AI调酒师</div>
      <div class="app-subtitle">
        <div class="subtitle-pill">私人</div>
        <div class="subtitle-pill">定制</div>
        <div class="subtitle-pill">鸡尾酒</div>
        <div class="subtitle-pill">推荐</div>
      </div>
    </div>
    
    <div class="cocktail-image">
      <div class="cocktail-glass"></div>
      <div class="lime-slice"></div>
    </div>
    
    <div id="main-content">
      <div class="input-container">
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
          alert('请告诉我你想要什么样的鸡尾酒！');
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
                      <img src="${imgUrl}" class="result-cocktail-image" alt="AI 调酒图片">
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
