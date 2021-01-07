
|app|IP|Hostname|user|pwd|
|-------|-------|--------------|-----------|------------|
|nextcloud |**192.168.2.12** |	pi8gb.home |  pi | Nw2xdihr|
|mattermost|**192.168.2.18** |	ubuntu.home | ubuntu |Nw2xdirh|       
|gitlab|**192.168.2.56** |	gitlab.example.com |  pi | Nw2xdihr|

|app|IP|Hostname|user|pwd|
|-------|-------|--------------|-----------|------------|
|homeassistant|192.168.2.55 |	homeassistant.home | pi |  Nw2xdihr|

 alias mm='ssh ubuntu@192.168.2.18'    # pi2GB  32GB mattermost boscp08 ThisIsCool_2020


alias pi5='ssh pi@192.168.2.55'
alias mntha55='sudo mount -t cifs -o user=homeassistant,password=homeassistant //192.168.2.55/config /mnt/nfsshare'


