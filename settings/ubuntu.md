## softwares

* 办公：wps
* 对比：meld
* 截图：flameshot
* 文本：

## smb config

* install smb
  sudo apt install samba
  sudo apt install smbclient

* /etc/samba/smb.conf 
  [share]
  comment = share folder
  browseable = yes
  path = /home/atom/share
  create mask = 0777
  directory mask = 0777
  valid users = atom
  force user = atom
  force group = atom
  public = yes
  available = yes
  read only = no
  writable = yes

* sudo smbpasswd -a atom

* sudo service smbd restart