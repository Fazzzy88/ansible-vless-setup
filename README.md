# Ansible VLESS Setup & Server Deployment 🚀

Автоматическое развертывание и подготовка инфраструктуры на VPS с использованием Ansible и Docker.

## Структура проекта

- `inventory.ini` — Список целевых серверов для управления.
- `vars.yml.example` — Шаблон конфигурационных переменных (порты, пути, параметры).
- `site.yml` — Основной Playbook автоматизации.

## Требования

- Установленный `ansible` на управляющей машине (ПК или управленческий сервер):
  ```bash
  sudo apt install ansible
- SSH-доступ с правами root или sudo к целевому VPS.

## Быстрый старт
1. Клонируйте репозиторий:
    ```bash
    git clone [https://github.com/Fazzzy/ansible-vless-setup.git](https://github.com/Fazzzy/ansible-vless-setup.git)
    cd ansible-vless-setup
    ```

2. Создайте файл конфигурации из шаблона:
    ```bash
    cp vars.yml.example vars.yml
    ```

3. Отредактируйте vars.yml и inventory.ini, указав актуальные IP-адреса и параметры.

Запустите Playbook:
    ```bash
    ansible-playbook -i inventory.ini site.yml
    ```
