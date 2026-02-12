import telebot
import requests
import json
TOKEN = "8543162857:AAH_j-tJM95g9QPNveQALy-TDcpWPQj-MkA"
bot = telebot.TeleBot("8543162857:AAH_j-tJM95g9QPNveQALy-TDcpWPQj-MkA")
API_KEY = "3481dd9ec04e380ea6511dcd"
@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, "Привет! Напиши валюту (например USD, EUR, RUB)")

@bot.message_handler(func=lambda message: True)
def handle_message(message):
    base_currency = message.text.upper()

    url = f"https://v6.exchangerate-api.com/v6/{API_KEY}/latest/{base_currency}"
    response = requests.get(url)
    data = response.json()

    if data['result'] == 'success':
        rates = data['conversion_rates']
        reply = f"Курсы валют для {base_currency}:\n\n"

        #только несколько валют
        for currency in ["KZT", "USD", "EUR", "RUB"]:
            reply += f"{currency}: {rates[currency]}\n"

        bot.reply_to(message, reply)
    else:
        bot.reply_to(message, "Неверная валюта или ошибка API.")

bot.polling(none_stop=True) 
