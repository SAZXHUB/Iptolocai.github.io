
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Geolocation จาก IP Address</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f0f0f0;
        }
        h1 {
            color: #333;
        }
        input, button, select {
            padding: 10px;
            margin: 10px 0;
            font-size: 16px;
        }
        #result {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1 id="title">Geolocation จาก IP Address</h1>
    <select id="language" onchange="changeLanguage()">
        <option value="th">ภาษาไทย</option>
        <option value="en">English</option>
        <option value="zh">中文 (Chinese)</option>
        <option value="ja">日本語 (Japanese)</option>
        <option value="ko">한국어 (Korean)</option>
        <option value="hi">हिन्दी (Hindi)</option>
        <option value="ar">العربية (Arabic)</option>
        <option value="ms">Bahasa Melayu (Malay)</option>
        <option value="vi">Tiếng Việt (Vietnamese)</option>
        <option value="id">Bahasa Indonesia (Indonesian)</option>
        <option value="bn">বাংলা (Bengali)</option>
    </select>
    <input type="text" id="ipAddress" placeholder="โปรดป้อน IP Address">
    <button onclick="getLocation()">ค้นหาตำแหน่ง</button>
    <div id="result"></div>

    <script>
        const translations = {
            th: {
                title: "Geolocation จาก IP Address",
                placeholder: "โปรดป้อน IP Address",
                button: "ค้นหาตำแหน่ง",
                error: "เกิดข้อผิดพลาด: ",
                notFound: "ไม่สามารถหาตำแหน่งที่อยู่ได้สำหรับ IP Address ที่ระบุ",
                location: "ตำแหน่งที่อยู่: ",
                city: "เมือง: ",
                country: "ประเทศ: "
            },
            en: {
                title: "Geolocation from IP Address",
                placeholder: "Please enter IP Address",
                button: "Find Location",
                error: "Error: ",
                notFound: "Could not find location for the specified IP Address",
                location: "Location: ",
                city: "City: ",
                country: "Country: "
            },
            zh: {
                title: "从 IP 地址获取地理位置",
                placeholder: "请输入 IP 地址",
                button: "查找位置",
                error: "错误: ",
                notFound: "找不到指定 IP 地址的位置",
                location: "位置: ",
                city: "城市: ",
                country: "国家: "
            },
            ja: {
                title: "IPアドレスから位置情報を取得",
                placeholder: "IPアドレスを入力してください",
                button: "位置を検索",
                error: "エラー: ",
                notFound: "指定されたIPアドレスの位置を見つけることができませんでした",
                location: "位置: ",
                city: "都市: ",
                country: "国: "
            },
            ko: {
                title: "IP 주소로부터 위치 정보",
                placeholder: "IP 주소를 입력하세요",
                button: "위치 찾기",
                error: "오류: ",
                notFound: "지정된 IP 주소의 위치를 찾을 수 없습니다",
                location: "위치: ",
                city: "도시: ",
                country: "국가: "
            },
            hi: {
                title: "IP पते से स्थान प्राप्त करें",
                placeholder: "कृपया IP पता दर्ज करें",
                button: "स्थान खोजें",
                error: "त्रुटि: ",
                notFound: "निर्दिष्ट IP पते के लिए स्थान नहीं मिल सका",
                location: "स्थान: ",
                city: "शहर: ",
                country: "देश: "
            },
            ar: {
                title: "الموقع الجغرافي من عنوان IP",
                placeholder: "الرجاء إدخال عنوان IP",
                button: "البحث عن الموقع",
                error: "خطأ: ",
                notFound: "تعذر العثور على الموقع لعنوان IP المحدد",
                location: "الموقع: ",
                city: "المدينة: ",
                country: "البلد: "
            },
            ms: {
                title: "Geolokasi daripada Alamat IP",
                placeholder: "Sila masukkan Alamat IP",
                button: "Cari Lokasi",
                error: "Ralat: ",
                notFound: "Tidak dapat mencari lokasi untuk Alamat IP yang dinyatakan",
                location: "Lokasi: ",
                city: "Bandar: ",
                country: "Negara: "
            },
            vi: {
                title: "Định vị địa lý từ địa chỉ IP",
                placeholder: "Vui lòng nhập địa chỉ IP",
                button: "Tìm vị trí",
                error: "Lỗi: ",
                notFound: "Không thể tìm thấy vị trí cho địa chỉ IP đã chỉ định",
                location: "Vị trí: ",
                city: "Thành phố: ",
                country: "Quốc gia: "
            },
            id: {
                title: "Geolokasi dari Alamat IP",
                placeholder: "Silakan masukkan Alamat IP",
                button: "Cari Lokasi",
                error: "Kesalahan: ",
                notFound: "Tidak dapat menemukan lokasi untuk Alamat IP yang ditentukan",
                location: "Lokasi: ",
                city: "Kota: ",
                country: "Negara: "
            },
            bn: {
                title: "আইপি ঠিকানা থেকে ভূ-অবস্থান",
                placeholder: "অনুগ্রহ করে আইপি ঠিকানা লিখুন",
                button: "অবস্থান খুঁজুন",
                error: "ত্রুটি: ",
                notFound: "নির্দিষ্ট আইপি ঠিকানার জন্য অবস্থান পাওয়া যায়নি",
                location: "অবস্থান: ",
                city: "শহর: ",
                country: "দেশ: "
            }
        };

        function changeLanguage() {
            const lang = document.getElementById('language').value;
            document.getElementById('title').innerText = translations[lang].title;
            document.getElementById('ipAddress').placeholder = translations[lang].placeholder;
            document.querySelector('button').innerText = translations[lang].button;
        }

        async function getLocation() {
            const ip = document.getElementById('ipAddress').value;
            const lang = document.getElementById('language').value;
            try {
                const response = await fetch(`https://ipapi.co/${ip}/json/`);
                const data = await response.json();

                if (data.error) {
                    throw new Error(data.reason);
                }

                if (data.latitude && data.longitude) {
                    const latStr = convertToDegreesMinutes(data.latitude, data.longitude);
                    document.getElementById('result').innerText = `${translations[lang].location} ${latStr}, ${translations[lang].city} ${data.city}, ${translations[lang].country} ${data.country_name}`;
                } else {
                    document.getElementById('result').innerText = translations[lang].notFound;
                }
            } catch (error) {
                document.getElementById('result').innerText = `${translations[lang].error} ${error.message}`;
            }
        }

        function convertToDegreesMinutes(latitude, longitude) {
            const latDeg = Math.floor(latitude);
            const latMin = (latitude - latDeg) * 60;
            const lonDeg = Math.floor(longitude);
            const lonMin = (longitude - lonDeg) * 60;

            const latDir = latDeg >= 0 ? 'N' : 'S';
            const lonDir = lonDeg >= 0 ? 'E' : 'W';

            const latStr = `${Math.abs(latDeg)}°${Math.floor(latMin)}'${(latMin % 1 * 60).toFixed(2)}"${latDir}`;
            const lonStr = `${Math.abs(lonDeg)}°${Math.floor(lonMin)}'${(lonMin % 1 * 60).toFixed(2)}"${lonDir}`;

            return `${latStr} ${lonStr}`;
        }

        // Set default language
        changeLanguage();
        
        let allLogs = [];

async function getLocation() {
    const ip = document.getElementById('ipAddress').value;
    const lang = document.getElementById('language').value;
    try {
        const response = await fetch(`https://ipapi.co/${ip}/json/`);
        const data = await response.json();

        if (data.error) {
            throw new Error(data.reason);
        }

        const logMessage = {
            timestamp: new Date().toLocaleString(),
            ipAddress: ip,
            language: lang,
            latitude: data.latitude,
            longitude: data.longitude,
            city: data.city,
            country: data.country_name
        };

        allLogs.push(logMessage);

        if (data.latitude && data.longitude) {
            const latStr = convertToDegreesMinutes(data.latitude, data.longitude);
            document.getElementById('result').innerText = `${translations[lang].location} ${latStr}, ${translations[lang].city} ${data.city}, ${translations[lang].country} ${data.country_name}`;
        } else {
            document.getElementById('result').innerText = translations[lang].notFound;
        }
    } catch (error) {
        document.getElementById('result').innerText = `${translations[lang].error} ${error.message}`;
    }
}
    </script>
</body>
</html>
