import urllib.request as req
from bs4 import BeautifulSoup
import telegram

TOKEN = ""
CHAT_ID = ""

def get_exchange_rate():
    try:
        code = req.urlopen("https://finance.naver.com/marketindex/exchangeList.naver")
        soup = BeautifulSoup(code, "html.parser")
        row = soup.select_one("body > div.tbl_area > table > tbody > tr:nth-child(37)")
        columns = row.find_all("td")

        return {
            "country": columns[0].text.strip(),
            "exchange_rate": columns[1].text.strip(),
            "buying_rate": columns[2].text.strip()
        }
    except Exception as e:
        print(f"Error during web scraping: {e}")
        return None

def create_message(exchange_rate_info, price):
    buying_rate_num = float(exchange_rate_info["buying_rate"])
    total_price = "{:,}".format(buying_rate_num * price)

    if buying_rate_num <= 25.1:
        message_status = "숙소값 결제!!!!!!!!!!!!!!!"
    elif 25.1 < buying_rate_num <= 25.5:
        message_status = "지켜보자...."
    else:
        message_status = "현재 환율 상태"

    message = f'({message_status})\n\n 현재 {exchange_rate_info["country"]}의 환율 정보입니다. \n 현찰 살 때 : {exchange_rate_info["buying_rate"]} / 매매기준율 : {exchange_rate_info["exchange_rate"]} \n\n 총 결제 금액: {total_price}입니다.'
    return message

def send_telegram_message(message):
    bot = telegram.Bot(TOKEN)
    bot.send_message(CHAT_ID, message)

def main():
    exchange_rate_info = get_exchange_rate()
    if exchange_rate_info:
        message = create_message(exchange_rate_info, 79400)
        send_telegram_message(message)

if __name__ == "__main__":
    main()
