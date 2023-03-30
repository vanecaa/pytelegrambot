import config
import logging
import asyncio
from aiogram import Bot, Dispatcher, Router

from datetime import datetime

logging.basicConfig(level=logging.INFO)
router = Router()

bot = Bot(token=config.TOKEN)

# Словарь со списком дней рождений участников
birthdays = {
    'dimarapira': datetime(1995, 3, 11),
    'Aza': datetime(1995, 12, 25),
    'Ad': datetime(1995, 4, 22),
    'Alim': datetime(1995, 8, 11),
    'Iluha': datetime(1995, 9, 10),
    'Toha': datetime(1995, 9, 28),
    'Temur': datetime(1995, 11, 10),
    'Andruha': datetime(1995, 6, 17),
    'Max': datetime(1995, 7, 20)
}

# Функция для отправки уведомления о дне рождении
async def send_birthday_notification(name: str) -> None:
    chat_id = -1001629510378
    await bot.send_message(chat_id, f"С днем рождения {name}! 🎉🎂🎁") 

# Проверяем дни рождений каждый день в полночь
async def check_birthdays() -> None:
    while True:
        try:
            await asyncio.sleep(1)
            today = datetime.now().date()
            for name, birthday in birthdays.items():
                if birthday.date().day == today.day and birthday.date().month == today.month:
                    await send_birthday_notification(name)
        except Exception as e:
            logging.exception(f"Error in check_birthdays: {e}")


# Запускаем проверку дней рождений
check_birthdays_task = None

async def on_startup(dp) -> None:
    print("Start")
    global check_birthdays_task
    chat_id = -1001629510378
    await bot.send_message(chat_id, "Бот запущен")
    check_birthdays_task = asyncio.create_task(check_birthdays())  # Start the check_birthdays coroutine

async def on_shutdown(dp) -> None:
    global check_birthdays_task
    if check_birthdays_task is not None:
        check_birthdays_task.cancel()
        await check_birthdays_task
    await bot.close()


    # Эхо бот
''''@router.message()
async def echo(message: types.Message):
    print(message.text)
    await message.answer(message.text)'''


async def main():
    dp = Dispatcher()
    dp.include_router(router)
    await dp.start_polling(bot)


if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    loop.create_task(check_birthdays())
    loop.create_task(main())
    loop.run_forever()
