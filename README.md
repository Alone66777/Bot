import logging
import os
import yt_dlp
from aiogram import Bot, Dispatcher, types, executor
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton  # AYNAN SHU YERDA!

# Bot tokenini atrof-muhitdan olish (Railway uchun)
API_TOKEN = os.getenv('7787773408:AAFo9F6xjZSlAteLelb6K5wxCUabS1k6KrU')

logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)

# /start komandasi
@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    kb = InlineKeyboardMarkup().add(
            InlineKeyboardButton('Kanalga obuna bo‘lish', url='https://t.me/THETEAMMY')
                )
                    await message.reply(
                            "Salom! Men Instagram va YouTube videolarini yuklab beruvchi botman. Videoning linkini yuboring.",
                                    reply_markup=kb
                                        )

                                        # Video yuklab olish
                                        @dp.message_handler()
                                        async def download_video(message: types.Message):
                                            url = message.text.strip()

                                                ydl_opts = {
                                                        'format': 'best',
                                                                'outtmpl': 'downloads/%(title)s.%(ext)s',
                                                                    }

                                                                        try:
                                                                                with yt_dlp.YoutubeDL(ydl_opts) as ydl:
                                                                                            info = ydl.extract_info(url, download=True)
                                                                                                        file_path = ydl.prepare_filename(info)

                                                                                                                await message.reply_video(open(file_path, 'rb'))

                                                                                                                        os.remove(file_path)

                                                                                                                            except Exception as e:
                                                                                                                                    await message.reply("Xatolik yuz berdi. Link to‘g‘ri ekanligini tekshiring.")
                                                                                                                                            print(e)

                                                                                                                                            # Botni ishga tushurish
                                                                                                                                            if __name__ == '__main__':
                                                                                                                                                if not os.path.exists('downloads'):
                                                                                                                                                        os.makedirs('downloads')
                                                                                                                                                            executor.start_polling(dp, skip_updates=True)