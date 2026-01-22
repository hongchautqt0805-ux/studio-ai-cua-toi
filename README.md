<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Creator Pro - Fixed 100%</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-[#05070a] text-white p-6 min-h-screen flex items-center justify-center">
    <div class="max-w-md w-full bg-[#111827] rounded-3xl p-8 border border-gray-800 shadow-2xl">
        <h1 class="text-2xl font-black text-center mb-6 text-blue-400 italic italic">AI CREATOR PRO</h1>
        
        <div class="space-y-4">
            <textarea id="prompt" rows="4" class="w-full bg-black/40 border border-gray-700 rounded-xl p-4 text-sm outline-none focus:border-blue-500" placeholder="Nhập ý tưởng video..."></textarea>
            
            <button onclick="runAI()" id="btn" class="w-full py-4 bg-blue-600 rounded-xl font-bold hover:bg-blue-500 transition-all shadow-lg shadow-blue-900/40">BẮT ĐẦU SẢN XUẤT</button>

            <div id="loading" class="hidden text-center text-[10px] text-blue-400 animate-pulse uppercase">Đang kết nối siêu máy tính Gemini...</div>

            <div id="result" class="hidden mt-6 p-4 bg-black/60 rounded-xl border border-blue-900/30">
                <p class="text-[10px] text-blue-500 font-bold mb-2 uppercase">Kịch bản triệu view:</p>
                <div id="outputText" class="text-sm text-gray-300 leading-relaxed whitespace-pre-wrap"></div>
            </div>
        </div>
    </div>

    <script>
        // API Key của bạn từ ảnh Google AI Studio
        const API_KEY = "AIzaSyCHOETmHolQhihQJS7DLGM5-qRLOVxLvgQ";

        async function runAI() {
            const input = document.getElementById('prompt').value;
            if(!input) return alert("Vui lòng nhập ý tưởng!");

            document.getElementById('loading').classList.remove('hidden');
            document.getElementById('result').classList.add('hidden');

            try {
                // SỬA LỖI: Dùng đúng endpoint v1 và model 'gemini-pro' cho gói Free
                const url = `https://generativelanguage.googleapis.com/v1/models/gemini-pro:generateContent?key=${API_KEY}`;
                
                const response = await fetch(url, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: "Hãy viết kịch bản chi tiết cho video TikTok về: " + input }] }]
                    })
                });

                const data = await response.json();
                
                if(data.error) {
                    alert("Lỗi từ Google: " + data.error.message);
                } else if (data.candidates && data.candidates[0].content.parts[0].text) {
                    document.getElementById('outputText').innerText = data.candidates[0].content.parts[0].text;
                    document.getElementById('result').classList.remove('hidden');
                }
            } catch (e) {
                alert("Lỗi kết nối: " + e.message);
            } finally {
                document.getElementById('loading').classList.add('hidden');
            }
        }
    </script>
</body>
</html>
