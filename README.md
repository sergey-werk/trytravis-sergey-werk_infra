# sergey-werk_infra
sergey-werk Infra repository

## Connect with ProxyJump

Подключиться к хосту внутренней сети можно воспользовавшись функцией ProxyJump стандартного ssh-клиента:

		ssh -J appuser@bastion appuser@internalhost

Для устаревших версий ssh:

    ssh -o ProxyCommand='ssh -q -W %h:%p appuser@bastion' appuser@internalhost

Также можно сохранить конфигурацию в файле `~/.ssh/config`:
```
Host internalhost
        User appuser
        HostName 10.130.0.9
        ProxyJump appuser@bastion
        IdentityFile ~/.ssh/appuser
```

## YC testapp

```
testapp_IP = 84.201.135.50
testapp_port = 9292
```

#### Run machine with bootstrap.sh:

```
yc compute instance create \
   --name reddit-app2 \
   --hostname reddit-app2 \
   --memory=4 \
   --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1604-lts,size=4GB,type=network-ssd \
   --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4, \
   --metadata serial-port-enable=1 \
   --metadata-from-file user-data=./bootstrap-metadata.yaml
```

#### Packer

```
packer verify immutable.json
packer build -var-file=variables.json immutable.json
```


#### Terraform with YC

[Docs](https://www.terraform.io/docs/providers/yandex/index.html)


Почему-то не работает с ssh-agent (не получается использовать запароленный ключ).
```
Error: Failed to parse ssh private key: ssh: this private key is passphrase protected
```


Шпаргалка по командам:
```
yc config list

terraform show
terraform refresh
terraform output
terraform taint yandex_compute_instance.app

```
