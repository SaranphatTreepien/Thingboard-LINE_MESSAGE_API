# Thingboard-LINE_MESSAGE_API

# Rule-Chain
1.SetCode - JAVA SCRIPT
if (msg.temperature > 30) {
    var newMsg = {};
    newMsg.to = "USER_ID"; // หรือ groupId, roomId ตามที่ต้องการ
    newMsg.messages = [{
      "type": "text",
      "text": "แจ้งเตือน: อุณหภูมิสูงเกิน 30°C (" + msg.temperature + "°C)"
    }];
    return {msg: newMsg, metadata: metadata, msgType: msgType};
} else {
    // ถ้าไม่เข้าเงื่อนไข ให้ return null เพื่อหยุด flow
    return null;
}
