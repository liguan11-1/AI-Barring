<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI调酒师</title>
    <style>
        body {
            font-family: 'Microsoft YaHei', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(135deg, #1a1a1a 0%, #2c3e50 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .container {
            background: rgba(255, 255, 255, 0.95);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            width: 90%;
            max-width: 800px;
            backdrop-filter: blur(10px);
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            font-family: 'Microsoft YaHei', 'PingFang SC', 'Source Han Sans SC', 'Helvetica Neue', Arial, sans-serif;
            font-size: 2.6em;
            font-weight: bold;
            margin-bottom: 10px;
            letter-spacing: 2px;
            text-shadow: 1px 1px 3px rgba(0,0,0,0.06);
        }
        .subtitle {
            text-align: center;
            color: #666;
            font-size: 1.2em;
            margin-bottom: 30px;
        }
        .input-group {
            margin-bottom: 25px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        textarea {
            width: 80%;
            height: 120px;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            resize: vertical;
            font-size: 16px;
            transition: all 0.3s ease;
            background: rgba(255, 255, 255, 0.9);
        }
        textarea:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 10px rgba(52, 152, 219, 0.2);
        }
        button {
            background: linear-gradient(45deg, #3498db, #2ecc71);
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            transition: all 0.3s ease;
            display: block;
            margin: 0 auto;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            margin-top: 10px;
            margin-bottom: 10px;
        }
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.3);
        }
        #response {
            margin-top: 30px;
            padding: 20px;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.9);
            white-space: pre-wrap;
            font-size: 16px;
            line-height: 1.6;
        }
        .loading {
            display: none;
            text-align: center;
            margin: 20px 0;
        }
        .loading-spinner {
            width: 50px;
            height: 50px;
            border: 5px solid #f3f3f3;
            border-top: 5px solid #3498db;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .cocktail-image {
            width: 100%;
            height: auto;
            max-width: 500px;
            border-radius: 10px;
            margin: 0;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            object-fit: contain;
        }
        .response-content {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>AI调酒师</h1>
        <div class="subtitle">你的私人调酒师，只需输入任何一句话，我将为您推荐最合适酒！</div>
        <div class="input-group">
            <textarea id="input" placeholder="告诉我你的心情，或者描述你想要的鸡尾酒..."></textarea>
        </div>
        <button onclick="callCozeAPI()">开始调酒</button>
        <div class="loading" id="loading">
            <div class="loading-spinner"></div>
            <p>正在为您调制完美的鸡尾酒...</p>
        </div>
        <div id="response"></div>
    </div>

    <script>
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
                const response = await fetch('https://api.coze.cn/v1/workflow/run', {
                    method: 'POST',
                    headers: {
                        'Authorization': 'Bearer pat_UsQXqoiWxqLD0E1YrYSVA5ufeehiIkocr0F3o3VZt9KgydM5Byr0zS0xgidSOCW2',
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        parameters: {
                            input: input
                        },
                        workflow_id: "7500239957405073420"
                    })
                });

                const data = await response.json();
                
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
                        <div class="response-content" style="text-align:center;">
                            <img src="${imgUrl}" class="cocktail-image" alt="鸡尾酒图片" style="max-width:320px;display:block;margin:0 auto 16px;">
                            <a href="${imgUrl}" download="鸡尾酒.png" style="display:inline-block;padding:10px 24px;background:linear-gradient(45deg,#3498db,#2ecc71);color:#fff;border-radius:8px;text-decoration:none;font-weight:bold;">下载图片</a>
                        </div>
                    `;
                } else {
                    document.getElementById('response').innerHTML = `<div class="response-content">未获取到图片链接</div>`;
                }
            } catch (error) {
                document.getElementById('response').innerHTML = `<div class="response-content">错误: ${error.message}</div>`;
            } finally {
                loading.style.display = 'none';
            }
        }
    </script>
</body>
</html> 
