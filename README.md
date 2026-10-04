# Сессионный проект 2026 — Безопасность GNU/Linux

Настройка базовых средств защиты ОС **РЕД ОС 8** (сертификат ФСТЭК №4060) без графического интерфейса: установка системы, настройка веб-сервера `nginx`, разграничение доступа (DAC + MAC), управление привилегиями через capabilities, политики аутентификации.

Выполнил: **Кобецкий Андрей**  

---

## Таблица выполнения заданий

| Раздел | Ключевые действия | Команды / артефакт |
|---|---|---|
| **1. Установка дистрибутива** | DHCP, hostname `mephi-2026.domain.local`, ping 8.8.8.8 | `hostnamectl`, `ping.out` |
| **2. Управление ПО** | `dnf update`, установка `nginx` + `libcap-ng-utils`, локальный RPM `tcpdump` | `dnf.out`, `history.out` |
| **3. Файловые системы** | Второй диск `/dev/sdb` → `ext4` с меткой `MEPHI_WEB`, автомонтирование через `fstab` | `/etc/fstab`, `stat.out` |
| **4. Сервисы** | `nginx` в автозагрузке + сбор журнала за текущую загрузку | `journalctl.out`, `systemctl is-enabled nginx` |
| **5.1 DAC** | Группа `developers` (5501–5503), `curators` (4444), ACL + setgid на `/data/mephi-2026` | `stat.out`, `getfacl /data/mephi-2026` |
| **5.2 Capabilities** | Снят set-UID у `tcpdump`, выдан `cap_net_raw,cap_net_admin=eip` | `getcap.out` |
| **5.3 MAC (SELinux)** | Режим `Enforcing`, контекст `httpd_sys_content_t` на `/mephi-web` | `getenforce.out`, `ls -Z /mephi-web` |
| **6.1 Ограничение входа** | Кураторам `curator1/curator2` shell = `/sbin/nologin` | `/etc/passwd` |
| **6.2 Управление паролями** | `PASS_MAX_DAYS 90`, `minlen = 12`| `/etc/login.defs`, `pwquality.conf`, `/etc/shadow` |
| **7. Тестирование** | `/mephi-web/index.html` отдаётся через nginx | `curl.out`, `mephi-screenshot.png` |
| **8. Публикация** | Публичный репозиторий на GitHub со всеми артефактами | этот файл |

---

## Содержимое репозитория

| Файл | Что подтверждает |
|---|---|
| `mephi-screenshot.png` | Визуальное подтверждение: веб-сервер отдаёт `Hello from Student: 359123` |
| `history.out` | Полная история выполненных команд |
| `ping.out` | Сетевая связность с `8.8.8.8` |
| `dnf.out` | История транзакций менеджера пакетов |
| `stat.out` | Права и SELinux-контексты директорий `/data/mephi-2026` и `/mephi-web` |
| `journalctl.out` | Лог запуска `nginx` за текущую загрузку |
| `getcap.out` | Capabilities бинарника `tcpdump` |
| `getenforce.out` | Режим работы SELinux (`Enforcing`) |
| `curl.out` | Результат проверки веб-сервера |
| `fstab` | Автомонтирование `/mephi-web` по метке `MEPHI_WEB` |
| `passwd` | Пользователи: `user1/2/3`, `curator1/2` (с `/sbin/nologin`) |
| `group` | Группы `developers` и `curators` |
| `shadow` | Сроки жизни паролей |
| `pwquality.conf` | Политика сложности пароля |

---
