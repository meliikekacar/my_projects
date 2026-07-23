# n8n_no_code_automation
Purpose: 
The goal of this project to develop an n8n workflow with no code automation that sends messages to Telegram in specific hour of the day based on current weather conditions.
The workflow checks real time weather data via Open-Meteo API and then sends a Telegram notification depending on weather conditions. 
If weather code indicates rain or snow (code ≥ 60), the user receives a reminder to take an umbrella. Otherwise, a message confirming clear weather is sent.

 <img width="454" height="232" alt="Resim1" src="https://github.com/user-attachments/assets/9d08f919-c129-4f14-bb3f-69c132a16592" />

Scenario:
Every morning at 7:00 AM, the workflow automatically checks Istanbul’s weather (latitude 41, longitude 29).
If precipitation is expected (weather code ≥ 60):
-	Telegram message: “⚠️ *GÜNLÜK HAVA UYARISI* ⚠️\n\nŞemsiyenizi yanınıza alın! “
Otherwise:
-	Telegram message: 🌞GÜNÜN HAVA DURUMU*\n\nHava bugün güneşli! Şemsiyeye gerek yok, iyi bir gün sizi bekliyor.

Step-by-Step Implementation:
Step 1 – Schedule Trigger Node
Node Name: Schedule Trigger1
Purpose: Automatically start the workflow every morning.
Configuration:
Trigger Interval: 1 Day
Trigger Time: 7:00 AM
Function: Runs the weather check daily without manual action.

<img width="1485" height="754" alt="Resim2" src="https://github.com/user-attachments/assets/ea47578f-edbf-4146-9470-cbbf3995237a" />
Step 2 – HTTP Request Node (Weather API)
Node Name: HTTP Request1
Purpose: Retrieve real-time weather forecast data.
API Used: Open-Meteo API
Configuration:
Method: GET
URL: https://api.open-meteo.com/v1/forecast?latitude=41.0082&longitude=28.9784&current_weather=true
Authentication: None
Output:

<img width="709" height="286" alt="Ekran Resmi 2026-07-23 18 21 03" src="https://github.com/user-attachments/assets/5ea159d4-7e91-489c-a6b8-78e2dc191306" />

<img width="941" height="788" alt="Resim3" src="https://github.com/user-attachments/assets/d69621bf-016e-4ef9-88a5-f5a6061cf214" />

Step 3 – If Node
Node Name: If
Purpose: Check whether the weather code indicates rain or snow.
Condition:
Expression: weathercode ≥ 60
If True → Rain/Snow expected
If False → Clear weather
Flow:
True Branch → Sends umbrella reminder.
False Branch → Sends sunny-weather message.	

If node ensures the workflow reacts to the real weather conditions coming from website. It uses numerical codes rather than strings.

<img width="889" height="849" alt="Resim4" src="https://github.com/user-attachments/assets/064b4778-ab40-4f89-92cc-7eeeb93c958c" />

Step 4 – Connecting Telegram True Statement
The workflow uses http requests node to Telegram Bot API for accesibility. This http request only works for true statement. If weather is rainy if node connects to the true statement and in telegram this message will be sent to the user. To Access telegram, telegram bot is giving chat_id and Token numbers. We use token number in Url and chat_id in the body part. Without these it is not possible to access and send messages through Telegram.
Node Name: HTTP Request 2 (True branch)
Method: POST
URL: https://api.telegram.org/bot8366231172:AAF1NW8Ylv4Bs2_dm8wEtQGgbaH4CjeEiBs/sendMessage
Body name: chat_id
Value: <My chat id from telegram>
Body name: text
Value: ⚠️ *GÜNLÜK HAVA UYARISI*/n/⚠️Şemsiyenizi yanınıza alın!

<img width="944" height="788" alt="Resim5" src="https://github.com/user-attachments/assets/c81f3333-eb78-4732-a8ce-96e3bf8b549c" />

<img width="950" height="779" alt="Resim6" src="https://github.com/user-attachments/assets/d8ecddec-264b-45ec-9e62-e2c636c3893a" />

Step 5 – Connecting Telegram False Statement
The same procedur applies in this step too. To send a message we should connect telegram to http request node. This statement works only for false statement. If weather is smaller than the indicated value (60) then false statement works and inform the user that the weather is sunny and there is no need for umbrella.
Node Name: HTTP Request 3 (False branch)
Method: POST
URL: https://api.telegram.org/bot8366231172:AAF1NW8Ylv4Bs2_dm8wEtQGgbaH4CjeEiBs/sendMessage
Body name: chat_id
Value: <My chat id from telegram>
Body name: text
Value: *🌞GÜNÜN HAVA DURUMU*/n/Hava bugün güneşli! Şemsiyeye gerek yok, iyi bir gün sizi bekliyor.

<img width="928" height="751" alt="Resim7" src="https://github.com/user-attachments/assets/b9d0223f-9fd1-489c-bf89-163360334da6" />
<img width="913" height="766" alt="Resim8" src="https://github.com/user-attachments/assets/f1f86baa-5bcd-44b4-a091-dabe9d21339e" />

    
Step 5 – Executing and Testing
The workflow unfourtunately only tested in clear weather conditions due to the weather was always sunny this week.
•	When weathercode < 60 if node evaluated false and then sending the sunny weather message.
Even though the workflow can only be tested in clear weather conditions there has not seen any mistake in true statement and it will be working in rainy days.

Results and Past Mistakes:
-	The automation runs Daily and successfully on time through Telegram.
-	The if condition uses weathercode ≥ 60 correctly and differentiates rainy and sunny days.

<img width="286" height="364" alt="Resim9" src="https://github.com/user-attachments/assets/85d5535e-1ad8-489a-b22a-36751ad7a33e" />


 Reflection
Through this project I learn no code automations and integrate API’s with n8n. I worked with nodes and i used numerical weather codes. With http request I can design flexible automations and i understood data driven desicion flows in no code.

Past Mistakes:
At first I decided to make a program that sends meme (internet funny images) to people. Fort hat i needed a meme service’s API address. But unfourtunately the website had access issues. Then i thought about other projects too such as horoscope comment apps or an app that shows the tweet of the day but all of them had issues about APIs. Then I came up with this idea and luckily its API had worked. 
