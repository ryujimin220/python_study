# python_study
## Weather Bot
```sh
import requests
from telegram import Bot
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

# OpenWeatherMap API 키와 도시 정보 설정
api_key = "fe05faa5e76c2edf42f9997b1e26d23d"
city = "SEOUL"

# 텔레그램 봇 토큰 설정
telegram_token = "6374512768:AAHoBzQON-TeRw0OapwxjAExh-tAcz5awIM"

# 텔레그램 봇 초기화
bot = Bot(token=telegram_token)

# 날씨 정보를 가져오는 함수
def get_weather():
    base_url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": city,
        "appid": api_key,
        "units": "metric"
    }

    response = requests.get(base_url, params=params)

    if response.status_code == 200:
        data = response.json()
        weather_description = data["weather"][0]["description"]
        temperature = data["main"]["temp"]
        return f"날씨: {weather_description}, 온도: {temperature}°C"
    else:
        return "날씨 정보를 가져올 수 없습니다."

# /weather 명령어를 처리하는 함수
def send_weather(update: Update, context: CallbackContext):
    chat_id = update.effective_chat.id
    weather_info = get_weather()
    bot.send_message(chat_id=chat_id, text=weather_info)

# 메인 함수
def main():
    updater = Updater(token=telegram_token, use_context=True)
    dp = updater.dispatcher

    bot.sendMessage(chat_id="932259636", text=get_weather())

if __name__ == "__main__":
    main()
```

## Use crontab to run the above script every day at 9:02am KST
```sh
## Weather Bot
2 0 * * * root python3 /root/bin/crontab_weatherbot.sh >> /root/bin/crontab_weatherbot.log 2>&1
```
