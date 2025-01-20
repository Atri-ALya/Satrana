import os
import sys
import time
import threading
import subprocess
import pygetwindow as gw
import keyboard
import speech_recognition as sr
import tkinter as tk  # Tkinter для GUI
from collections import deque
from queue import Queue
import getpass
import win32com.client  # Для создания ярлыков в Windows


# Глобальные переменные
blocked_sites = [
    "youtube - google chrome",
    "twitch - google chrome",
    "vimeo - google chrome",
    "dailymotion - google chrome",
    "netflix - google chrome",
    "hulu - google chrome",
    "disneyplus - google chrome",
    "primevideo - google chrome",
    "youtube.com - firefox",
    "twitch.tv - firefox",
    "vimeo.com - firefox",
    "dailymotion.com - firefox",
    "netflix.com - firefox",
    "hulu.com - firefox",
    "disneyplus.com - firefox",
    "primevideo.com - firefox",
    "youtube.ru - google chrome",
    "twitch.tv - google chrome",
    "vimeo.com - google chrome",
    "dailymotion.com - google chrome",
    "netflix.com - google chrome",
    "hulu.com - google chrome",
    "youtube.com - opera",
    "twitch.tv - opera",
    "vimeo.com - opera",
    "dailymotion.com - opera",
    "netflix.com - opera",
    "hulu.com - opera",
    "youtube.com - tor",
    "twitch.tv - tor",
    "vimeo.com - tor",
    "dailymotion.com - tor",
    "netflix.com - tor",
    "hulu.com - tor",
    "peertube - brave",
    "d-tube - brave",
    "bitchute - brave",
    "odnoklassniki - brave",
    "rutube - brave",
    "streamable - brave",
    "vube - brave",
    "youtube.com - edge",
    "twitch.tv - edge",
    "vimeo.com - edge",
    "dailymotion.com - edge",
    "netflix.com - edge",
    "hulu.com - edge",
    "peertube - edge",
    "d-tube - edge",
    "bitchute - edge",
    "odnoklassniki - edge",
    "rutube - edge",
    "streamable - edge",
    "vube - edge",
    "youtube.com - safari",
    "twitch.tv - safari",
    "vimeo.com - safari",
    "dailymotion.com - safari",
    "netflix.com - safari",
    "hulu.com - safari",
    "peertube - safari",
    "d-tube - safari",
    "bitchute - safari",
    "odnoklassniki - safari",
    "rutube - safari",
    "streamable - safari",
    "vube - safari",
    "youtube.com - vivaldi",
    "twitch.tv - vivaldi",
    "vimeo.com - vivaldi",
    "dailymotion.com - vivaldi",
    "netflix.com - vivaldi",
    "hulu.com - vivaldi",
    "peertube - vivaldi",
    "d-tube - vivaldi",
    "bitchute - vivaldi",
    "odnoklassniki - vivaldi",
    "rutube - vivaldi",
    "streamable - vivaldi",
    "vube - vivaldi",
    "youtube.com - maxthon",
    "twitch.tv - maxthon",
    "vimeo.com - maxthon",
    "dailymotion.com - maxthon",
    "netflix.com - maxthon",
    "hulu.com - maxthon",
    "peertube - maxthon",
    "d-tube - maxthon",
    "bitchute - maxthon",
    "odnoklassniki - maxthon",
    "rutube - maxthon",
    "streamable - maxthon",
    "vube - maxthon",
    "youtube.com - waterfox",
    "twitch.tv - waterfox",
    "vimeo.com - waterfox",
    "dailymotion.com - waterfox",
    "netflix.com - waterfox",
    "hulu.com - waterfox",
    "peertube - waterfox",
    "d-tube - waterfox",
    "bitchute - waterfox",
    "odnoklassniki - waterfox",
    "rutube - waterfox",
    "streamable - waterfox",
    "vube - waterfox",
    "youtube.com - epic browser",
    "twitch.tv - epic browser",
    "vimeo.com - epic browser",
    "dailymotion.com - epic browser",
    "netflix.com - epic browser",
    "hulu.com - epic browser",
    "peertube - epic browser",
    "d-tube - epic browser",
    "bitchute - epic browser",
    "odnoklassniki - epic browser",
    "rutube - epic browser",
    "streamable - epic browser",
    "vube - epic browser",
    "youtube.com - uc browser",
    "twitch.tv - uc browser",
    "vimeo.com - uc browser",
    "dailymotion.com - uc browser",
    "netflix.com - uc browser",
    "hulu.com - uc browser",
    "peertube - uc browser",
    "d-tube - uc browser",
    "bitchute - uc browser",
    "odnoklassniki - uc browser",
    "rutube - uc browser",
    "streamable - uc browser",
    "vube - uc browser",
    "youtube.com - qutebrowser",
    "twitch.tv - qutebrowser",
    "vimeo.com - qutebrowser",
    "dailymotion.com - qutebrowser",
    "netflix.com - qutebrowser",
    "hulu.com - qutebrowser",
    "peertube - qutebrowser",
    "d-tube - qutebrowser",
    "bitchute - qutebrowser",
    "odnoklassniki - qutebrowser",
    "rutube - qutebrowser",
    "streamable - qutebrowser",
    "vube - qutebrowser",
    "youtube.com - pale moon",
    "twitch.tv - pale moon",
    "vimeo.com - pale moon",
    "dailymotion.com - pale moon",
    "netflix.com - pale moon",
    "hulu.com - pale moon",
    "peertube - pale moon",
    "d-tube - pale moon",
    "bitchute - pale moon",
    "odnoklassniki - pale moon",
    "rutube - pale moon",
    "streamable - pale moon",
    "vube - pale moon",
    "steam - google chrome",
    "epicgames - google chrome",
    "riotgames - google chrome",
    "origin - google chrome",
    "battle.net - google chrome",
    "roblox - google chrome",
    "minecraft.net - google chrome",
    "playstation.com - google chrome",
    "xbox.com - google chrome",
    "steamcommunity - google chrome",
    "epicgames.com - firefox",
    "riotgames.com - firefox",
    "origin.com - firefox",
    "battle.net - firefox",
    "roblox.com - firefox",
    "minecraft.net - firefox",
    "playstation.com - firefox",
    "xbox.com - firefox",
    "steam - maxthon",
    "epicgames - maxthon",
    "riotgames - maxthon",
    "origin - maxthon",
    "battle.net - maxthon",
    "roblox - maxthon",
    "minecraft.net - maxthon",
    "playstation.com - maxthon",
    "xbox.com - maxthon",
    "steamcommunity - maxthon",
    "steam - waterfox",
    "epicgames - waterfox",
    "riotgames - waterfox",
    "origin - waterfox",
    "battle.net - waterfox",
    "roblox - waterfox",
    "minecraft.net - waterfox",
    "playstation.com - waterfox",
    "xbox.com - waterfox",
    "steamcommunity - waterfox",
    "steam - epic browser",
    "epicgames - epic browser",
    "riotgames - epic browser",
    "origin - epic browser",
    "battle.net - epic browser",
    "roblox - epic browser",
    "minecraft.net - epic browser",
    "playstation.com - epic browser",
    "xbox.com - epic browser",
    "steamcommunity - epic browser",
    "steam - uc browser",
    "epicgames - uc browser",
    "riotgames - uc browser",
    "origin - uc browser",
    "battle.net - uc browser",
    "roblox - uc browser",
    "minecraft.net - uc browser",
    "playstation.com - uc browser",
    "xbox.com - uc browser",
    "steamcommunity - uc browser",
    "steam - qutebrowser",
    "epicgames - qutebrowser",
    "riotgames - qutebrowser",
    "origin - qutebrowser",
    "battle.net - qutebrowser",
    "roblox - qutebrowser",
    "minecraft.net - qutebrowser",
    "playstation.com - qutebrowser",
    "xbox.com - qutebrowser",
    "steamcommunity - qutebrowser",
    "steam - pale moon",
    "epicgames - pale moon",
    "riotgames - pale moon",
    "origin - pale moon",
    "battle.net - pale moon",
    "roblox - pale moon",
    "minecraft.net - pale moon",
    "playstation.com - pale moon",
    "xbox.com - pale moon",
    "steamcommunity - pale moon",
    "facebook - google chrome",
    "instagram - google chrome",
    "twitter - google chrome",
    "tiktok - google chrome",
    "snapchat - google chrome",
    "reddit - google chrome",
    "pinterest - google chrome",
    "discord - google chrome",
    "facebook.com - firefox",
    "instagram.com - firefox",
    "twitter.com - firefox",
    "tiktok.com - firefox",
    "snapchat.com - firefox",
    "reddit.com - firefox",
    "pinterest.com - firefox",
    "discord.com - firefox",
    "facebook.com - maxthon",
    "instagram.com - maxthon",
    "twitter.com - maxthon",
    "tiktok.com - maxthon",
    "snapchat.com - maxthon",
    "reddit.com - maxthon",
    "pinterest.com - maxthon",
    "discord.com - maxthon",
    "facebook.com - waterfox",
    "instagram.com - waterfox",
    "twitter.com - waterfox",
    "tiktok.com - waterfox",
    "snapchat.com - waterfox",
    "reddit.com - waterfox",
    "pinterest.com - waterfox",
    "discord.com - waterfox",
    "facebook.com - epic browser",
    "instagram.com - epic browser",
    "twitter.com - epic browser",
    "tiktok.com - epic browser",
    "snapchat.com - epic browser",
    "reddit.com - epic browser",
    "pinterest.com - epic browser",
    "discord.com - epic browser",
    "facebook.com - uc browser",
    "instagram.com - uc browser",
    "twitter.com - uc browser",
    "tiktok.com - uc browser",
    "snapchat.com - uc browser",
    "reddit.com - uc browser",
    "pinterest.com - uc browser",
    "discord.com - uc browser",
    "facebook.com - qutebrowser",
    "instagram.com - qutebrowser",
    "twitter.com - qutebrowser",
    "tiktok.com - qutebrowser",
    "snapchat.com - qutebrowser",
    "reddit.com - qutebrowser",
    "pinterest.com - qutebrowser",
    "discord.com - qutebrowser",
    "facebook.com - pale moon",
    "instagram.com - pale moon",
    "twitter.com - pale moon",
    "tiktok.com - pale moon",
    "snapchat.com - pale moon",
    "reddit.com - pale moon",
    "pinterest.com - pale moon",
    "discord.com - pale moon",
    "vk.com - google chrome",
    "rutube.ru - google chrome",
    "ok.ru - google chrome",
    "mail.ru - google chrome",
    "yandex.ru - google chrome",
    "dzen.ru - google chrome",
    "vk.com - firefox",
    "rutube.ru - firefox",
    "ok.ru - firefox",
    "mail.ru - firefox",
    "yandex.ru - firefox",
    "dzen.ru - firefox",
    "vk.com - maxthon",
    "rutube.ru - maxthon",
    "ok.ru - maxthon",
    "mail.ru - maxthon",
    "yandex.ru - maxthon",
    "dzen.ru - maxthon",
    "vk.com - waterfox",
    "rutube.ru - waterfox",
    "ok.ru - waterfox",
    "mail.ru - waterfox",
    "yandex.ru - waterfox",
    "dzen.ru - waterfox",
    "vk.com - epic browser",
    "rutube.ru - epic browser",
    "ok.ru - epic browser",
    "mail.ru - epic browser",
    "yandex.ru - epic browser",
    "dzen.ru - epic browser",
    "vk.com - uc browser",
    "rutube.ru - uc browser",
    "ok.ru - uc browser",
    "mail.ru - uc browser",
    "yandex.ru - uc browser",
    "dzen.ru - uc browser",
    "vk.com - qutebrowser",
    "rutube.ru - qutebrowser",
    "ok.ru - qutebrowser",
    "mail.ru - qutebrowser",
    "yandex.ru - qutebrowser",
    "dzen.ru - qutebrowser",
    "vk.com - pale moon",
    "rutube.ru - pale moon",
    "ok.ru - pale moon",
    "mail.ru - pale moon",
    "yandex.ru - pale moon",
    "dzen.ru - pale moon",
    "newgrounds - google chrome",
    "kongregate - google chrome",
    "addictinggames - google chrome",
    "armor games - google chrome",
    "miniclip - google chrome",
    "addictinggames.com - firefox",
    "kongregate.com - firefox",
    "armor games.com - firefox",
    "miniclip.com - firefox",
    "newgrounds.com - firefox",
    "kongregate.ru - maxthon",
    "addictinggames.ru - maxthon",
    "armor games.ru - maxthon",
    "miniclip.ru - maxthon",
    "newgrounds.com - maxthon",
    "newgrounds.ru - waterfox",
    "kongregate.ru - waterfox",
    "addictinggames.ru - waterfox",
    "armor games.ru - waterfox",
    "miniclip.ru - waterfox",
    "newgrounds.com - epic browser",
    "kongregate.com - epic browser",
    "addictinggames.com - epic browser",
    "armor games.com - epic browser",
    "miniclip.com - epic browser",
    "newgrounds.com - uc browser",
    "kongregate.com - uc browser",
    "addictinggames.com - uc browser",
    "armor games.com - uc browser",
    "miniclip.com - uc browser",
    "newgrounds.com - qutebrowser",
    "kongregate.com - qutebrowser",
    "addictinggames.com - qutebrowser",
    "armor games.com - qutebrowser",
    "miniclip.com - qutebrowser",
    "newgrounds.com - pale moon",
    "kongregate.com - pale moon",
    "addictinggames.com - pale moon",
    "armor games.com - pale moon",
    "miniclip.com - pale moon",
    "youtube.com - tor",
    "twitch.tv - tor",
    "vimeo.com - tor",
    "dailymotion.com - tor",
    "netflix.com - tor",
    "hulu.com - tor",
    "peertube - tor",
    "d-tube - tor",
    "bitchute - tor",
    "odnoklassniki - tor",
    "rutube - tor",
    "streamable - tor",
    "vube - tor",
    "steam - tor",
    "epicgames - tor",
    "riotgames - tor",
    "origin - tor",
    "battle.net - tor",
    "roblox - tor",
    "minecraft.net - tor",
    "playstation.com - tor",
    "xbox.com - tor",
    "steamcommunity - tor",
    "facebook - tor",
    "instagram - tor",
    "twitter - tor",
    "tiktok - tor",
    "snapchat - tor",
    "reddit - tor",
    "pinterest - tor",
    "discord - tor",
    "vk.com - tor",
    "rutube.ru - tor",
    "ok.ru - tor",
    "mail.ru - tor",
    "yandex.ru - tor",
    "dzen.ru - tor",
    "newgrounds - tor",
    "kongregate - tor",
    "addictinggames - tor",
    "armor games - tor",
    "miniclip - tor",
    "Главная – смотреть онлайн в высоком качестве на RUTUBE - Opera",
    "Блогеры: Главная – смотреть онлайн в высоком качестве на RUTUBE - Opera"
]

stop_threads = threading.Event()
cancel_shutdown = threading.Event()
command_queue = Queue()  # Очередь для передачи команды в основной поток

def check_microphone():
    print("Проверяем доступность микрофона...")
    mic_list = sr.Microphone.list_microphone_names()
    if not mic_list:
        print("Микрофоны не найдены. Убедитесь, что устройство подключено.")
        return False

    print("Доступные микрофоны:")
    for i, mic in enumerate(mic_list):
        print(f"{i}: {mic}")

    return True

def recognize_speech_from_mic():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        recognizer.adjust_for_ambient_noise(source)
        print("Слушаю...")
        audio = recognizer.listen(source)
        try:
            return recognizer.recognize_google(audio, language="ru-RU")
        except sr.UnknownValueError:
            print("Не удалось распознать речь")
            return None
        except sr.RequestError:
            print("Ошибка связи с сервисом распознавания")
            return None


def monitor_microphone(keywords):
    """Прослушивание микрофона и выполнение действия по голосовой команде."""
    while not stop_threads.is_set():
        text = recognize_speech_from_mic()
        if text:
            print(f"Распознанный текст: {text}")  # Добавляем вывод для отладки
            if any(keyword in text.lower() for keyword in keywords):  # Проверяем на совпадение с любым из ключевых слов
                print("Команда для выключения распознана.")
                command_queue.put("shutdown")  # Отправляем команду в основной поток
        time.sleep(1)



def get_active_window():
    """Получение названия активного окна"""
    try:
        active_window = gw.getActiveWindow()
        return active_window.title if active_window else None
    except Exception as e:
        print(f"Ошибка при получении активного окна: {e}")
        return None

def load_blocked_sites(file_path="blocked_sites.txt"):
    """Загрузка списка запрещённых сайтов из файла"""
    try:
        with open(file_path, "r", encoding="utf-8") as file:
            return [line.strip().lower() for line in file if line.strip()]
    except FileNotFoundError:
        print(f"Файл {file_path} не найден. Создаём пустой список.")
        return []

def save_blocked_sites(blocked_sites, file_path="blocked_sites.txt"):
    """Сохранение списка запрещённых сайтов"""
    try:
        with open(file_path, "w", encoding="utf-8") as file:
            file.write("\n".join(blocked_sites))
    except Exception as e:
        print(f"Ошибка при сохранении списка сайтов: {e}")

def show_cancel_window(cancel_keyword):
    """Окно отмены выключения"""
    cancel_shutdown.clear()

    def on_submit(event=None):
        if entry.get().strip().lower() == cancel_keyword.lower():
            cancel_shutdown.set()
            window.destroy()

    def countdown():
        for i in range(5, 0, -1):
            timer_label.config(text=f"Секунд до отмены: {i}")
            window.update()
            time.sleep(1)
        if not cancel_shutdown.is_set():
            subprocess.run(["shutdown", "/s", "/t", "10"], check=True)
        window.destroy()

    window = tk.Tk()
    window.title("Отмена выключения")

    # Метка
    tk.Label(window, text="Введите секретное слово:").pack(pady=10)

    # Поле ввода
    entry = tk.Entry(window)
    entry.pack(pady=10)

    # Кнопка отправки
    submit_button = tk.Button(window, text="Отменить", command=on_submit)
    submit_button.pack(pady=10)

    # Таймер
    timer_label = tk.Label(window, text="Секунд до отмены: 5")
    timer_label.pack(pady=10)

    # Запуск таймера
    threading.Thread(target=countdown, daemon=True).start()

    window.mainloop()


def monitor_websites(cancel_keyword):
    """Мониторинг активного окна на запрещённые сайты"""
    while not stop_threads.is_set():
        active_window_title = get_active_window()
        print(f"Текущее активное окно: {active_window_title}")  # Отладочный вывод
        if active_window_title:
            for site in blocked_sites:
                if site.lower() in active_window_title.lower():  # Приводим оба текста к нижнему регистру
                    print(f"Обнаружен запрещённый сайт: {site}")
                    if not show_cancel_window(cancel_keyword):
                        subprocess.run(["shutdown", "/s", "/t", "10"], check=True)
                    else:
                        subprocess.run(["shutdown", "/a"], check=True)
        time.sleep(1)


def monitor_keys(cancel_keyword):
    """Отслеживание нажатий клавиш с возможностью отмены"""
    key_combinations = {'w', 'a', 's', 'd', 'ц', 'ф', 'ы', 'в', 'right', 'left', 'up', 'down', 'tab', 'space'}
    key_press_times = deque()

    def on_key_press(event):
        current_time = time.time()
        key = event.name.lower()
        print(f"Нажата клавиша: {key}")  # Отладочный вывод
        if key in key_combinations:
            key_press_times.append(current_time)
            while key_press_times and key_press_times[0] < current_time - 10:
                key_press_times.popleft()
            if len(key_press_times) >= 15:
                print("Множественные нажатия клавиш. Подготовка к выключению...")
                if not show_cancel_window(cancel_keyword):
                    subprocess.run(["shutdown", "/s", "/t", "10"], check=True)
                else:
                    subprocess.run(["shutdown", "/a"], check=True)

    keyboard.on_press(on_key_press)
    while not stop_threads.is_set():
        time.sleep(1)

def add_keyword(keywords, new_keyword):
    """Добавление нового слова в список ключевых слов"""
    new_keyword = new_keyword.lower()
    if new_keyword not in keywords:
        keywords.append(new_keyword)
        print(f"Слово '{new_keyword}' добавлено в список запрещенных слов.")
    else:
        print(f"Слово '{new_keyword}' уже есть в списке.")

def remove_keyword(keywords, keyword_to_remove):
    """Удаление слова из списка ключевых слов"""
    keyword_to_remove = keyword_to_remove.lower()
    if keyword_to_remove in keywords:
        keywords.remove(keyword_to_remove)
        print(f"Слово '{keyword_to_remove}' удалено из списка запрещенных слов.")
    else:
        print(f"Слово '{keyword_to_remove}' не найдено в списке.")

def show_keywords(keywords):
    """Вывод списка ключевых слов"""
    print("Список запрещенных слов:")
    for i, keyword in enumerate(keywords, start=1):
        print(f"{i}. {keyword}")


def main():
    global blocked_sites

    keywords = [
        "выключи компьютер", "выключение", "пора спать", "код",
        "код в группе класса", "яйца", "заходи ко мне", "поставь другой ник",
        "крыса", "блять", "сасасть", "соси", "роблокс", "игры", "лох"
    ]

    # Пример использования
    add_keyword(keywords, "Зачем я это добавил? Да сам не знаю... Помогите мне!!")  # Добавляет "новое слово" в список
    remove_keyword(keywords, "Может когда-то здесь что-то будет?")  # Удаляет слово "яйца" из списка
    show_keywords(keywords)  # Выводит обновленный список
    
    cancel_keyword = "Атри"
    blocked_sites = load_blocked_sites()

    # Запуск потоков для мониторинга сайтов и клавиш
    website_monitor_thread = threading.Thread(target=monitor_websites, args=(cancel_keyword,), daemon=True)
    website_monitor_thread.start()

    key_monitor_thread = threading.Thread(target=monitor_keys, args=(cancel_keyword,), daemon=True)
    key_monitor_thread.start()

    # Запуск потока для прослушивания микрофона
    microphone_thread = threading.Thread(target=monitor_microphone, args=(keywords,), daemon=True)
    microphone_thread.start()

    try:
        while True:
            # Если в очереди есть команда, обрабатываем ее
            if not command_queue.empty():
                command = command_queue.get()
                if command == "shutdown":
                    show_cancel_window(cancel_keyword)
            time.sleep(1)
    except KeyboardInterrupt:
        print("Завершение программы...")
        stop_threads.set()


if __name__ == "__main__":
    main()
