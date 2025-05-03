# Thingboard-LINE_MESSAGE_API

ตัวอย่างขั้นตอนและเนื้อหาสำหรับเขียน README หรือ Wiki บน GitHub เพื่ออธิบายวิธีสร้างระบบแจ้งเตือนอุณหภูมิสูงผ่าน LINE ด้วย ThingsBoard

🚨 IoT Temperature Alert to LINE with ThingsBoard
1. เตรียม LINE Messaging API
สร้าง LINE Official Account และเปิดใช้งาน Messaging API

สร้าง Channel Access Token สำหรับใช้ส่งข้อความผ่าน API

จดจำ USER_ID หรือ groupId/roomId ที่ต้องการส่งข้อความ

2. ตั้งค่า ThingsBoard Rule Chain
2.1 สร้าง Script Transform Node
เพิ่ม Node ประเภท Script Transformation ใน Rule Chain

ใส่โค้ดตัวอย่างนี้:

javascript
if (msg.temperature > 30) {
    var newMsg = {};
    newMsg.to = "USER_ID"; // เปลี่ยนเป็น userId/groupId/roomId ที่ต้องการ
    newMsg.messages = [{
      "type": "text",
      "text": "แจ้งเตือน: อุณหภูมิสูงเกิน 30°C (" + msg.temperature + "°C)"
    }];
    return {msg: newMsg, metadata: metadata, msgType: msgType};
} else {
    return null; // ไม่เข้าเงื่อนไขจะหยุด flow
}
2.2 เพิ่ม REST API Call Node
ต่อ Node ถัดไปเป็น REST API Call

ตั้งค่าดังนี้:

URL: https://api.line.me/v2/bot/message/push

Method: POST

Headers:

Content-Type: application/json

Authorization: Bearer {LINE_CHANNEL_ACCESS_TOKEN} (แทนที่ด้วย token ของคุณ)

Body:
ใช้ ${msg} (หรือ ${msg.msg} ถ้าโครงสร้าง message อยู่ใน msg.msg)

3. ตัวอย่าง Flow
อุปกรณ์ส่งค่า temperature มาที่ ThingsBoard

Script Node ตรวจสอบค่า ถ้าเกิน 30°C จะสร้างข้อความแจ้งเตือน

REST API Call Node ส่งข้อความแจ้งเตือนผ่าน LINE Messaging API

4. หมายเหตุ
ต้องมี LINE Official Account และเปิดใช้งาน Messaging API

ตรวจสอบว่า Channel Access Token และ USER_ID ถูกต้อง

สามารถขยาย logic ใน Script Node สำหรับเงื่อนไขอื่น ๆ ได้

5. อ้างอิง
[LINE Messaging API Getting Started]

[LINE Messaging API Reference]

[ThingsBoard Rule Engine Guide]
