<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 微信分享配置（必填，否则分享时显示异常） -->
    <meta property="og:title" content="2026跨年倒计时">
    <meta property="og:description" content="小韩祝大家2026新年快乐，一起倒数迎接新年！">
    <meta property="og:image" content="https://picsum.photos/id/237/400/300"> <!-- 分享缩略图（可替换为自己的图片） -->
    <meta property="og:type" content="website">
    <title>2026跨年倒计时 | 小韩祝大家新年快乐</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "Microsoft YaHei", "SimHei", Arial, sans-serif;
        }
        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%);
            color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-around;
            padding: 20px;
        }
        /* 顶部日期样式 */
        .date-container {
            font-size: 18px;
            color: #f0f0f0;
            text-align: center;
        }
        /* 中间倒计时样式（响应式字体） */
        .countdown-container {
            font-size: clamp(4rem, 15vw, 10rem); /* 手机小、电脑大 */
            font-weight: bold;
            color: #00c8ff; /* 科技蓝 */
            text-shadow: 0 0 20px rgba(0, 200, 255, 0.5);
            margin: 40px 0;
        }
        /* 底部祝福区域 */
        .blessing-container {
            text-align: center;
        }
        .blessing {
            font-size: clamp(1.5rem, 5vw, 2rem);
            color: #ff3c3c; /* 喜庆红 */
            font-weight: bold;
            margin-bottom: 15px;
        }
        .tip {
            font-size: clamp(1.2rem, 4vw, 1.8rem);
            color: #ffc864; /* 暖黄色 */
        }
        /* 适配小屏幕 */
        @media (max-width: 480px) {
            .countdown-container {
                margin: 20px 0;
            }
            .blessing {
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <!-- 顶部：公历+农历日期 -->
    <div class="date-container" id="date"></div>

    <!-- 中间：核心倒计时 -->
    <div class="countdown-container" id="countdown">00:00:00</div>

    <!-- 底部：祝福+提示语 -->
    <div class="blessing-container">
        <div class="blessing" id="blessing">小韩祝大家2026新年快乐</div>
        <div class="tip" id="tip">此刻 来年 都要幸福</div>
    </div>

    <script>
        // 目标时间：2026年1月1日 00:00:00
        const targetTime = new Date('2026-01-01T00:00:00').getTime();

        // 2025年12月 公历→农历映射（和Java版一致）
        const lunarMap = [
            "", "冬月初一", "冬月初二", "冬月初三", "冬月初四", "冬月初五", "冬月初六", "冬月初七", "冬月初八", "冬月初九", "冬月初十",
            "冬月十一", "冬月十二", "冬月十三", "冬月十四", "冬月十五", "冬月十六", "冬月十七", "冬月十八", "冬月十九", "冬月二十",
            "冬月廿一", "冬月廿二", "冬月廿三", "冬月廿四", "冬月廿五", "冬月廿六", "冬月廿七", "冬月廿八", "冬月廿九", "冬月三十", "腊月初一"
        ];

        // 星期映射
        const weekMap = ["周日", "周一", "周二", "周三", "周四", "周五", "周六"];

        // 获取当前公历+农历日期
        function getCurrentDate() {
            const now = new Date();
            const year = now.getFullYear();
            const month = now.getMonth() + 1; // 月份从0开始
            const day = now.getDate();
            const week = weekMap[now.getDay()];
            // 农历（仅2025年12月有效）
            const lunarDay = lunarMap[day] || "腊月初一";
            return `${year}年${padZero(month)}月${padZero(day)}日 ${week} ${lunarDay}`;
        }

        // 数字补零（如1→01）
        function padZero(num) {
            return num < 10 ? `0${num}` : num;
        }

        // 更新倒计时
        function updateCountdown() {
            const now = new Date().getTime();
            const diff = targetTime - now;

            // 跨年之后的处理
            if (diff <= 0) {
                document.getElementById('countdown').textContent = "00:00:00";
                document.getElementById('blessing').textContent = "小韩祝大家2026新年快乐 🎉";
                document.getElementById('tip').textContent = "新的一年 万事顺意！";
                return;
            }

            // 计算时、分、秒
            const hours = Math.floor(diff / (1000 * 60 * 60));
            const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((diff % (1000 * 60)) / 1000);

            // 格式化并显示
            const countdownText = `${padZero(hours)}:${padZero(minutes)}:${padZero(seconds)}`;
            document.getElementById('countdown').textContent = countdownText;
        }

        // 初始化并每秒更新
        document.getElementById('date').textContent = getCurrentDate();
        updateCountdown();
        setInterval(() => {
            document.getElementById('date').textContent = getCurrentDate();
            updateCountdown();
        }, 1000);
    </script>
</body>
</html>
