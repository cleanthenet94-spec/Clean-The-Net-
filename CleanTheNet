const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);
const path = require('path');

// إعداد ملفات الموقع (HTML/CSS) لتظهر للمستخدم
app.use(express.static(path.join(__dirname, '.')));

// متغير لتخزين عدد المتصلين
let connectedUsers = 0;

// التعامل مع الاتصالات اللحظية
io.on('connection', (socket) => {
    // زيادة العدد عند دخول شخص جديد
    connectedUsers++;
    io.emit('updateUserCount', connectedUsers);

    // استقبال رابط الإبلاغ من أحد المستخدمين (بدون إظهاره للعامة فوراً)
    socket.on('sendTargetLink', (link) => {
        // هنا نرسل إشارة "ساعة الصفر" للجميع
        // الرابط يتم تمريره مخفياً داخل الزر ليتم استخدامه عند الضغط فقط
        io.emit('zeroHourAlert', { target: link });
    });

    // تنقيص العدد عند خروج شخص
    socket.on('disconnect', () => {
        connectedUsers--;
        io.emit('updateUserCount', connectedUsers);
    });
});

// تشغيل السيرفر (مهم جداً لـ Render: استخدام process.env.PORT)
const PORT = process.env.PORT || 3000;
http.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
