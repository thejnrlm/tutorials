# tutorials

## Samba

Entre no modo de superuser:
`su -`

Crie 2 diretórios, um para o "rh" e o outro para o "financeiro", mas antes vou criar uma pasta chamada “server” para centralizar os diretórios do “rh”e “financeiro”:
```
mkdir /server
mkdir /server/rh /server/financeiro
```

Altere as permissões dos diretórios:
```
chmod 777 -R /server/rh/
chmod 777 -R /server/financeiro/
```

Instale Samba e suas dependências:
`apt install samba samba-common`

Crie uma cópia do arquivo “smb.conf”:
`cp /etc/samba/smb.conf /etc/samba/smb.conf.bkp`

Realize as alterações abaixo no server samba:
`nano /etc/samba/smb.conf`

```
[global]
   workgroup = WORKGROUP 
   netbios name = Servidor

####### Authentication #######
   security = user

[RH]
   path = /server/rh/
   comment = Arquivos RH
   browseable = yes
   public = no
   writable = no
   guest ok = no
   read list = administrador,rh
   write list = administrador,rh
   admin users = administrador,rh
   valid users = administrador,rh
   create mask = 0777
   directory mask = 0777

[FINANCEIRO]
   path = /server/financeiro/
   comment = Arquivos Financeiro
   browseable = yes
   public = no
   writable = no
   guest ok = no
   read list = administrador,financeiro
   write list = administrador,financeiro
   admin users = administrador,financeiro
   valid users = administrador,financeiro
   create mask = 0777
   directory mask = 0777
```

Reiniciar o serviço de Samba:
`systemctl restart smbd.service nmbd.service`

Criar novo usuário no Linux:
```
adduser rh
adduser financeiro
```

Criar novo usuário no servidor Samba:
```
smbpasswd -a administrador
smbpasswd -a rh
smbpasswd -a financeiro
```
