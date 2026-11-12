import os
import threading
import tls_client
from colorama import Fore, init

# تهيئة ألوان الشاشة
init(autoreset=True)

# طباعة شعار الأداة
def banner():
    print(Fore.CYAN + """
    =========================================
       Discord Token Joiner / Mass Joiner
           Developed for Sentero
    =========================================
    """)

# دالة انضمام التوكن للسيرفر
def join_server(token, invite_code):
    # استخدام tls-client لتجاوز حماية دسكورد (Cloudflare)
    session = tls_client.Session(
        client_identifier="chrome_112",
        random_tls_extension_order=True
    )
    
    # رابط الانضمام عبر واجهة دسكورد البرمجية
    url = f"https://discord.com/api/v9/invites/{invite_code}"
    
    headers = {
        "Authorization": token.strip(),
        "Content-Type": "application/json",
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36"
    }

    try:
        response = session.post(url, headers=headers)
        if response.status_code == 200:
            print(Fore.GREEN + f"[+] Success: Token joined successfully!")
        elif response.status_code == 429:
            print(Fore.YELLOW + f"[!] Rate Limited: Too many requests, slowing down.")
        else:
            print(Fore.RED + f"[-] Failed: Status code {response.status_code} | Response: {response.text}")
    except Exception as e:
        print(Fore.RED + f"[-] Error: {e}")

def main():
    banner()
    
    # طلب رابط الدعوة من المستخدم
    invite_code = input(Fore.YELLOW + "[?] Enter Discord Invite Code (or full link): ").strip()
    if "discord.gg/" in invite_code:
        invite_code = invite_code.split("discord.gg/")[1]
    elif "invite/" in invite_code:
        invite_code = invite_code.split("invite/")[1]

    # التأكد من وجود ملف التوكنات
    if not os.path.exists("tokens.txt"):
        print(Fore.RED + "[-] Error: 'tokens.txt' not found! Please create it and add your tokens.")
        return

    with open("tokens.txt", "r") as f:
        tokens = [line.strip() for line in f if line.strip()]

    print(Fore.CYAN + f"[*] Loaded {len(tokens)} tokens. Starting process...\n")

    threads = []
    for token in tokens:
        t = threading.Thread(target=join_server, args=(token, invite_code))
        threads.append(t)
        t.start()
        # تأسيط بسيط بين الخيوط لمنع الحظر السريع
        # time.sleep(0.5)

    for t in threads:
        t.join()

    print(Fore.GREEN + "\n[+] Process completed!")

if __name__ == "__main__":
    main()
