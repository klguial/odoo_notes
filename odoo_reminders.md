### Odoo installation 
Softawares:  
- VMWare Workstation 14 Player
- Ubuntu Odoo Image File
- Navicat for Postgres
- Pentaho Report Designer
run Ubuntu Odoo Image file on VMWare.  
`cd` to `/opt/files/`  
download odoo `wget toolkt.com/files/odoo/odoo10.sh` or `odoo9.sh` or `odoo.sh` (odoov8)  
configure settings. comment out lines for postgresql installation  
run bash file `./odoo10.sh`  
go to **192.168.10.100:1069** to open odoo  

### Odoo service reminders
ODOO DB PASS: `P@SSW0RD`  
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

/opt/odoo9/server/openerp-server -c /etc/odoo9-server.conf -u <module_name> -d <db_name>
/opt/odoo9/server/openerp-server -c /etc/odoo9-server.conf --addons-path=/opt/odoo9/custom/addons,/opt/odoo9/custom/
```

### Pentaho Reports for OpenERP Installation
1. add pentaho-reports folder in the same directory as the new module
2. install manually
3. check if requiremnt file `/var/lib/tomcat7/webapps/perntaho-reports-for-openerp.war` exists
4. install at destination using `wget http://toolkt.com/files/pentaho-reports-for-openerp.war`

### Pentaho Reports Parameter Usage
type | value
--- | ---
name | ids
label | ids
value typye | object

### Navicat db connection config
**odoo**  
name: odoo9  
host: (virtual ip) 192.168.192.128  
post: 5432  
initial database: postgres  
username: odoomgr  
pwd: odoomgr  
  
**localhost**  
name: odoo9  
host: localhost  
post: 5432  
initial database: postgres  
username: postgres  
pwd: postgres  

### restore a zip file database
```bash
pg_restore -U postgres -d dvdrental
```

### pentaho: configure multiple selected records, different report per id
Put the whole form inside the group.  
Configure group settings to be grouped by `id`  
Set `pagebreak-after` (or `pagebreak-before`) to `true` in the style settings for group

### pentaho forms page formatting
**orientation:** portrait  
**width:** 612  
**height:** 504  
**top-bottom margin:** 30  
**left-right margin:** 40  




