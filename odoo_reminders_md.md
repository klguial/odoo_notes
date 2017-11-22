# Odoo installation 
Softawres:  
- VMWare Workstation 14 Player
- Ubuntu Odoo Image File
- Navicat for Postgres
- Pentaho Report Designer

# Odoo service reminders
ODOO DB PASS  
P@SSW0RD  
  
```bash
ssh tk@192.168.10.100
tk
sudo su

ifconfig
su odoo9

service odoo9-server stop
pkill -f odoo9
ps aux | grep odoo9

service postgresql restart
sudo netstat -anp | grep 9069

telinit 0 #shutdown
telinit 6 #restart
```
