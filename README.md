# ssh-in-tor
🧠 Концепция (что мы сделали)

Когда обычный SSH ломают на уровне протокола:
	•	ping работает
	•	TCP 22 открыт
	•	но соединение рвётся

👉 значит фильтрация DPI

Решение:

👉 спрятать SSH внутри Tor
👉 использовать .onion hidden service
🏗 Архитектура
[ ТЫ ]
   ↓ (Tor SOCKS)
127.0.0.1:9050
   ↓
[ Tor сеть ]
   ↓
.onion адрес
   ↓
[ SSH сервер ]
👉 SSH больше не виден провайдеру вообще
👉 выглядит как обычный Tor-трафик

⸻

⚙️ Часть 1 — сервер (самое важное)

1. Установка Tor
sudo apt install tor -y
2. Настройка hidden service
sudo nano /etc/tor/torrc
Добавить:
HiddenServiceDir /var/lib/tor/ssh_hidden/
HiddenServicePort 22 127.0.0.1:22
3. Перезапуск
sudo systemctl restart tor
4. Получение onion-адреса
sudo cat /var/lib/tor/ssh_hidden/hostname
👉 получаешь:
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.onion
⚠️ Важный момент
	•	это НЕ DNS
	•	это НЕ IP
	•	это криптографический адрес

👉 его нельзя “угадать” или отсканировать
