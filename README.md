import os
import telebot
import requests

BOT_TOKEN = os.environ['BOT_TOKEN']
GROQ_KEY = os.environ['GROQ_KEY']

bot = telebot.TeleBot(BOT_TOKEN)


@bot.message_handler(commands=['start'])
def start(message): bot.send_message(message.chat.id, 'Привет! Я AI-студия. Напиши мне что-нибудь — придумаю идею!')


@bot.message_handler(func=lambda m: True)def reply(message):r =requests.post(
        'https://api.groq.com/openai/v1/chat/completions',
headers={'Authorization': 'Bearer ' + GROQ_KEY},
json={
'model': 'openai/gpt-oss-20b',
'messages': [{'role': 'user', 'content': message.text}]
        }
    )
answer = r.json()['choices'][['message']['content']
bot.send_message(message.chat.id, answer)


bot.polling()# -
Creative-bot 
