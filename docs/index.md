# 🏠 Home

---

> [!CAUTION]
> **Please don't use this book blindly or for bad stuff.**
>
> This book is under [MIT License](https://github.com/ky4meru/ky4meru.github.io/blob/main/LICENSE). The content within this book is provided on an 'as is' basis, and the author makes no representations or warranties of any kind, express or implied, about the completeness, accuracy, reliability, suitability, or availability of the information, products, services, or related graphics contained within this book. Any reliance you place on such information is therefore strictly at your own risk.
>
> The author shall in no event be liable for any loss or damage, including without limitation, indirect or consequential loss or damage, or any loss or damage whatsoever arising from loss of data or profits arising out of, or in connection with, the use of this book.
>
> The tactics and techniques described in this book should not be used for any illegal or malicious activities. The author does not condone or support any illegal or unethical activities, and any use of the information contained within this book is at the user's own risk and discretion.

---

#### 👋 Welcome

This book centralizes all cybersecurity offensive tactics and techniques I discovered from challenges, trainings, certifications and real life engagements. It is my personal notebook I use daily, but I decided to turn it public to give back to the InfoSec community what I learnt from it.

I built this book as a mix between a wiki and a mind-map. Simply [follow links](/docs/index.html#-lets-start) depending on what you want to achieve.

Enjoy your reading!

#### 🛑 Before you start

Tactics and techniques described in this book leverage dozens of open-source tools. Despite that I try to link each tool to its official source every time, it could be painful to manually download each tool one by one.

That's why I propose you to deploy my [Kalice](https://github.com/ky4meru/Kalice) Ansible playbook, which automatically install everything you will need on you Kali Linux environment!

```shell
git clone https://github.com/ky4meru/Kalice.git
sudo apt install -y ansible
ansible-playbook main.yml
```

#### ⬇️ Let's start

Go on the page corresponding to your **target**:

- [Active Directory domain](/docs/active-directory/index.html) like `target.local`
- [Azure tenant](/azure/) like `target.onmicrosoft.com`
- [Domain name](/network/) like `*.target.com`
- [Linux host](/docs/linux/index.html) like `target`
- [Network range](/network/) like `10.0.0.0/8`
- [People](/osint/) like `John Doe from Target & Co.`
- [Web application](/webapps/) like `https://app.target.com/`
- [Windows host](/docs/windows/index.html) like `TARGET`

#### 🙏 Acknowledgements

The idea of this book is definitely inspired from following ones. They constitute an awesome knowledge base, many thanks to them. If you're looking for an information that is not here, it is for sure there:

- [CSbyGB](https://csbygb.gitbook.io/)
- [HackTricks](https://book.hacktricks.xyz/)
- [IRed.Team](https://www.ired.team/)
- [Pentester's Promiscuous Notebook](https://ppn.snovvcrash.rocks/)
- [The Hacker Recipes](https://www.thehacker.recipes/)