<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Content Studio - LIVE</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-[#05070a] text-white p-4 min-h-screen flex items-center justify-center">
    <div class="max-w-xl w-full bg-gray-900/80 border border-white/10 rounded-3xl p-8 shadow-2xl">
        <h1 class="text-2xl font-black text-center mb-6 bg-gradient-to-r from-blue-400 to-purple-500 bg-clip-text text-transparent italic">AI CREATOR PRO</h1>
        
        <div class="space-y-4">
            <textarea id="userInput" rows="4" class="w-full bg-black/50 border border-gray-700 rounded-xl p-4 text-sm outline-none focus:border-blue-500" placeholder="Nhập ý tưởng video của bạn..."></textarea>
            <button onclick="startAI()" id="btn" class="w-full py-4 bg-blue-600 rounded-xl font-bold hover:bg-blue-500 transition-all transform active:scale-95">BẮT ĐẦU SẢN XUẤT</button>

            <div id="loading" class="hidden text-center py-4">
                <p class="text-blue-400 animate-pulse text-xs">ĐANG KẾT NỐI GOOGLE AI STUDIO...</p>
            </div>

            <div id="result" class="hidden p-4 bg-blue-900/20 border border-blue-500/30 rounded-xl">
                <h3 class="text-xs font-bold text-blue-400 mb-2">KỊCH BẢN CHI TIẾT:</h3>
                <p id="textContent" class="text-sm leading-relaxed text-gray-200"></p>
            </div>
        </div>
    </div>

    <script>
        const API_KEY = "AIzaSyCHOETmHolQhihQJS7DLGM5-qRLOVxLvgQ"; 

        async function startAI() {
            const input = document.getElementById('userInput').value;
            if(!input) return alert("Vui lòng nhập ý tưởng!");

            document.getElementById('loading').classList.remove('hidden');
            document.getElementById('result').classList.add('hidden');
            document.getElementById('btn').disabled = true;

            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`, {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: "Hãy viết kịch bản TikTok 15 giây hấp dẫn cho ý tưởng này: " + input }] }]
                    })
                });

                const data = await response.json();
                if (data.candidates) {
                    document.getElementById('result').classList.remove('hidden');
                    document.getElementById('textContent').innerText = data.candidates[0].content.parts[0].text;
                } else {
                    alert("Lỗi API: " + (data.error ? data.error.message : "Không xác định"));
                }
            } catch (error) {
                alert("Lỗi kết nối! Hãy kiểm tra internet.");
            } finally {
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('btn').disabled = false;
            }
        }
    </script>
</body>
</html>
