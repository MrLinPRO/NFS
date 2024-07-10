
# NFS
# **ДЗ**
* Создали дефолтные машины серв, клиент)
* Настройка сервера NFS
* Настройка клиента NFS

# **Выполнение**

### Настройка сервера NFS

Установка сервера
```
apt install nfs-kernel-server
```

```
Проверяем наличие слушающих портов 2049/udp, 2049/tcp,111/udp, 111/tcp (screen "ports")
ss -tnplu 
```
Создаём и настраиваем директорию, которая будет экспортирована в будущем
```
mkdir -p /srv/share/upload
chown -R nfsnobody:nfsnobody /srv/share
chmod 0777 /srv/share/upload
cat << EOF > /etc/exports
/srv/share 192.168.50.11/32(rw,sync,root_squash)
EOF
exportfs -r
```

Проверка экспорта каталога
```
[root@nfss ~]# exportfs -s
/srv/share  192.168.50.11/32(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
```

### Настройка клиента NFS

Установка пакета с NFS-клиентом
```
apt install nfs-common
```

Добавляем в /etc/fstab строку 
root@nfsc:~# echo "192.168.50.10:/srv/share/ /mnt nfs vers=3,noauto,x-systemd.automount 0 0" >> /etc/fstab
```

Проверка монтирования
```
screen "mount"
```
Результат
Выполненные при конфигурировании сервера и клиента команды внесены в скрипты для автоматического конфигурирования машин.
