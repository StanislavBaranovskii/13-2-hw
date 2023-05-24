# Домашнее задание к занятию 13.2 «`Защита хоста`» - `Станислав Барановский`

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

## Задание 1

1. Установите **eCryptfs**.
2. Добавьте пользователя cryptouser.
3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.


*В качестве ответа  пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.*  

```bash
sudo apt update
sudo apt install -y ecryptfs-utils

sudo adduser --encrypt-home usertest
su - usertest
touch readme
exit

sudo ls /home/usertest/

#Миграция домашнего каталога пользователя:
sudo ecryptfs-migrate-home -u user
#Шифрование раздела swap:
sudo ecryptfs-setup-swap
#Информация для восстановления:
ecryptfs-unwrap-passphrase
```

---

## Задание 2

1. Установите поддержку **LUKS**.
2. Создайте небольшой раздел, например, 100 Мб.
3. Зашифруйте созданный раздел с помощью LUKS.

*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*

```bash
sudo apt install gparted
#Установка LUKS (должна быть установлено по умолчанию):
sudo apt-get install cryptsetup
#Проверка установки:
cryptsetup --version
#Подготовка раздела (luksFormat):
sudo cryptsetup -y -v --type luks2 luksFormat /dev/sdb1
#Монтирование раздела:
sudo cryptsetup luksOpen /dev/sdb1 disk
ls /dev/mapper/disk
#Форматирование раздела:
sudo dd if=/dev/zero of=/dev/mapper/disk
sudo mkfs.ext4 /dev/mapper/disk
#Монтирование «открытого» раздела:
mkdir .secret
sudo mount /dev/mapper/disk .secret/
#Завершение работы:
sudo umount .secret
sudo cryptsetup luksClose disk
```

---

## Задание 3 *

1. Установите **apparmor**.
2. Повторите эксперимент, указанный в лекции.
3. Отключите (удалите) apparmor.


*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*
