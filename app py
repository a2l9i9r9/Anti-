cat > app.py << 'EOF'
import requests
import time
import os
from flask import Flask
from threading import Thread

app = Flask(__name__)

TOKEN = "8399356480:AAFShivOk9pi2iv1N-7FbKSJoVoKK6jgqh0"
BAD = ["فردین", "فردینپور", "فردین پور"]

last_update_id = 0

def send_message(chat_id, text):
    url = f"https://api.telegram.org/bot{TOKEN}/sendMessage"
    data = {"chat_id": chat_id, "text": text}
    requests.post(url, data=data)

def delete_message(chat_id, message_id):
    url = f"https://api.telegram.org/bot{TOKEN}/deleteMessage"
    data = {"chat_id": chat_id, "message_id": message_id}
    requests.post(url, data=data)

def get_updates():
    global last_update_id
    url = f"https://api.telegram.org/bot{TOKEN}/getUpdates"
    params = {"offset": last_update_id + 1, "timeout": 30}
    try:
        response = requests.get(url, params=params, timeout=35)
        return response.json().get("result", [])
    except:
        return []

def run_bot():
    print("بات روشن شد!")
    while True:
        try:
            updates = get_updates()
            for update in updates:
                last_update_id = update["update_id"]
                msg = update.get("message")
                if msg:
                    text = msg.get("text", "")
                    chat_id = msg["chat"]["id"]
                    message_id = msg["message_id"]
                    
                    for w in BAD:
                        if w in text:
                            delete_message(chat_id, message_id)
                            send_message(chat_id, "خودت روش کراش داری روت نمیشه منو میندازی وسط")
                            break
            time.sleep(1)
        except Exception as e:
            print(f"Error: {e}")
            time.sleep(3)

@app.route('/')
def home():
    return "بات روشنه!"

@app.route('/health')
def health():
    return "OK"

if __name__ == "__main__":
    # اجرای بات توی یه نخ جداگانه
    bot_thread = Thread(target=run_bot)
    bot_thread.start()
    # اجرای فلاسک برای اینکه Render پورت رو ببینه [citation:5]
    port = int(os.environ.get("PORT", 5000))
    app.run(host='0.0.0.0', port=port)
EOF
