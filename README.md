<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Content Studio Pro - Fixed</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body { background: #05070a; color: white; font-family: sans-serif; }
        .glass { background: rgba(17, 24, 39, 0.9); backdrop-filter: blur(10px); border: 1px solid rgba(255,255,255,0.1); }
    </style>
</head>
<body class="p-4 flex items-center justify-center min-h-screen">
    <div class="max-w-md w-full glass rounded-3xl p-8 shadow-2xl">
        <h1 class="text-3xl font-black text-center mb-8 bg-gradient-to-r from-blue-400 to-purple-500 bg-clip-text text-transparent italic">
            AI CREATOR PRO
        </h1>

        <div class="space-y-6">
            <textarea id="userPrompt" rows="4" class="w-full bg-black/50 border border-gray-700 rounded-2xl p-4 text-sm outline-none focus:border-blue-500" placeholder="Nhập ý tưởng của bạn..."></textarea>
            
            <button onclick="startAI()" id="btnGenerate" class="w-full py-4 bg-blue-600 rounded-2xl font-bold text-white uppercase tracking-widest hover:bg-blue-500 transition-all">
                Bắt đầu sản xuất
            </button>

            <div id="status" class="hidden text-center text-blue-400 text-xs animate-pulse">
                Đang kết nối siêu máy tính Gemini 3...
            </div>

            <div id="resultContainer" class="hidden mt-6 p-4 border border-gray-800 rounded-2xl">
                <p class="text-xs text-gray-500 mb-2 uppercase">Kết quả kịch bản tối ưu:</p>
                <div id="optimizedResult" class="text-sm text-gray-300 italic"></div>
            </div>
        </div>
    </div>

    <script>
        // Key của bạn đã được gắn cố định vào đây
        const API_KEY = "AIzaSyCHOETmHolQhihQJS7DLGM5-qRLOVxLvgQ"; 

        async function startAI() {
            const prompt = document.getElementById('userPrompt').value;
            const status = document.getElementById('status');
            const resultBox = document.getElementById('resultContainer');
            const resultText = document.getElementById('optimizedResult');

            if(!prompt) return alert("Hãy nhập ý tưởng!");

            status.classList.remove('hidden');
            resultBox.classList.add('hidden');

            try {
                // SỬA LỖI Ở ĐÂY: Dùng đúng endpoint và model Gemini 3 Pro mới nhất
                const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro:generateContent?key=${API_KEY}`;
                
                const response = await fetch(url, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({
                        contents: [{ 
                            parts: [{ 
                                text: `Bạn là chuyên gia kịch bản. Hãy dịch và mở rộng ý tưởng này thành prompt tiếng Anh chuyên nghiệp để tạo video: ${prompt}` 
                            }] 
                        }]
                    })
                });

                const data = await response.json();

                if (data.error) {
                    alert("Lỗi từ Google: " + data.error.message);
                } else {
                    const output = data.candidates[0].content.parts[0].text;
                    resultText.innerText = output;
                    resultBox.classList.remove('hidden');
                    alert("Gemini đã xử lý xong ý tưởng của bạn!");
                }
            } catch (error) {
                alert("Lỗi kết nối mạng hoặc API. Hãy thử lại!");
            } finally {
                status.classList.add('hidden');
            }
        }
    </script>
</body>
</html>
