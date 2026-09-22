Ansible VLESS Setup

Ansible Playbook для подготовки VPS к дальнейшему развёртыванию VLESS-сервера.

Playbook устанавливает Docker, настраивает базовое окружение сервера, создаёт рабочую директорию и открывает необходимые порты в UFW.

Проект рассчитан в первую очередь на Ubuntu и предназначен для того, чтобы не выполнять базовую настройку сервера вручную каждый раз.

Структура проекта
.
├── inventory.ini
├── vars.yml.example
├── site.yml
├── .gitignore
└── README.md


inventory.ini — список серверов, на которых будет выполняться Playbook.

vars.yml.example — пример файла с переменными проекта.

vars.yml — локальный файл с реальными значениями. В репозиторий не добавляется.

site.yml — основной Playbook.

.gitignore — список файлов, которые Git не должен отслеживать.

Требования

Для запуска потребуется:

Ansible на локальной машине;

SSH-доступ к VPS;

права root или возможность использовать sudo;

VPS с Ubuntu.

Установить Ansible на Ubuntu или Debian можно следующим образом:

sudo apt update
sudo apt install ansible


Проверить установку:

ansible --version

Настройка

Сначала укажите сервер в inventory.ini:

[vps_servers]
vps1 ansible_host=YOUR_SERVER_IP ansible_user=root ansible_port=22

[vps_servers:vars]
ansible_python_interpreter=/usr/bin/python3


Затем создайте локальный файл с переменными:

cp vars.yml.example vars.yml


После этого отредактируйте vars.yml и укажите необходимые значения.

Например:

---
working_dir: "/opt/vless-server"
domain_name: "example.com"

vless_port: 443
reality_dest: "example.com:443"

server_names:
  - "example.com"
  - "www.example.com"

user_uuid: "00000000-0000-0000-0000-000000000000"
short_id: "0123456789abcdef"


Файл vars.yml не нужно добавлять в Git, так как в нём могут находиться значения, которые не стоит публиковать.

Проверка подключения

Перед запуском Playbook можно проверить, что Ansible подключается к серверу:

ansible -i inventory.ini vps_servers -m ping


Если всё настроено правильно, Ansible вернёт:

vps1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

Запуск

После проверки подключения запустите Playbook:

ansible-playbook -i inventory.ini site.yml


Playbook выполнит следующие действия:

Обновит список пакетов.

Установит необходимые системные зависимости.

Добавит репозиторий Docker.

Установит Docker и Docker Compose.

Запустит Docker и добавит его в автозагрузку.

Создаст рабочую директорию.

Настроит UFW.

Откроет SSH-порт и порт, указанный в vless_port.

На этом этапе Playbook подготавливает сервер. Сам VLESS/Xray-сервис и его конфигурация пока отдельно не разворачиваются.

Безопасность

Не добавляйте в репозиторий реальные секреты и приватные данные.

Локальный файл vars.yml добавлен в .gitignore и должен оставаться только на машине, с которой запускается Ansible.

В дальнейшем для хранения секретов можно использовать Ansible Vault.

.gitignore

В корне проекта должен находиться файл .gitignore:

vars.yml
*.retry
.vault_password


Перед коммитом стоит проверить состояние репозитория:

git status


Убедитесь, что vars.yml не отображается среди файлов, которые Git собирается добавить.

Повторный запуск

Playbook можно запускать повторно. Ansible проверяет текущее состояние сервера и применяет только необходимые изменения.

Например:

ansible-playbook -i inventory.ini site.yml


Это позволяет использовать один и тот же Playbook для первоначальной настройки и последующего обслуживания сервера.

Что можно добавить дальше

В текущем виде проект занимается подготовкой VPS. В дальнейшем сюда можно добавить:

Docker Compose для Xray;

шаблон конфигурации VLESS/Reality;

автоматическую генерацию конфигурации;

отдельную Ansible role для Xray;

Ansible Vault для секретных переменных;

автоматический запуск и обновление контейнера.
