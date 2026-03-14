# sdbot.php
<?php
// توكن بوت صانع البوتات
$botToken = "8713555145:AAFK5oyjqJVJ_STsSDRE8D2uGViFtlTnUyw";
$website = "https://api.telegram.org/bot".$botToken;

// قراءة الرسائل الواردة
$update = json_decode(file_get_contents("php://input"), TRUE);
$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];

// أوامر البوت
if ($message == "/start") {
    $reply = "أهلاً بك في بوت صانع البوتات 🔹\nأرسل اسم البوت الجديد لإنشاء ملف بوت PHP جاهز.";
} else {
    // اسم البوت الجديد من المستخدم (يسمح أحرف وأرقام و _ فقط)
    $botName = preg_replace("/[^a-zA-Z0-9_]/", "", $message);
    $fileName = $botName . ".php";

    // كود بوت PHP جديد جاهز
    $botCode = "<?php
// بوت جديد: $botName
\$botToken = 'PUT_NEW_BOT_TOKEN_HERE';
\$website = 'https://api.telegram.org/bot'.\$botToken;
\$update = json_decode(file_get_contents('php://input'), TRUE);
\$chatId = \$update['message']['chat']['id'];
\$message = \$update['message']['text'];
\$reply = 'بوت $botName جاهز! ارسل /start.';
file_get_contents(\$website.'/sendMessage?chat_id='.\$chatId.'&text='.urlencode(\$reply));
?>";

    // حفظ الملف
    file_put_contents($fileName, $botCode);

    $reply = "تم إنشاء بوت جديد باسم `$fileName`.\nيمكنك تعديل التوكن داخل الملف.";
}

// إرسال الرد للمستخدم
file_get_contents($website."/sendMessage?chat_id=".$chatId."&text=".urlencode($reply));
?>
