import re
from telethon import TelegramClient, events
from binance.client import Client

api_id = ********
api_hash = "********"
SESSION = "session"

TARGET_ROOM = -******** 

client = TelegramClient("session", api_id, api_hash)

def parse_signal(msg):
    msg = msg.lower()

    if "롱" in msg:
        return "LONG"
    if "숏" in msg:
        return "SHORT"
    if "반익" in msg:
        return "PARTIAL"
    if "정리" in msg or "익자유" in msg:
        return "CLOSE"

    return None

client.on(events.NewMessage(chats=TARGET_ROOM))
async def handler(event):
    msg = event.text

    signal = parse_signal(msg)

    if signal:
        print(" 시그널 감지:", signal)
    else:
        print("무시:", msg)

client.start()
client.run_until_disconnected()
