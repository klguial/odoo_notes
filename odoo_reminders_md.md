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

/opt/odoo9/server/openerp-server -c /etc/odoo9-server.conf -u <module_name> -d <db_name>
/opt/odoo9/server/openerp-server -c /etc/odoo9-server.conf --addons-path=/opt/odoo9/custom/addons,/opt/odoo9/custom/
```

# Pentaho Reports for OpenERP Installation
1. add pentaho-reports folder in the same directory as the new module
2. install manually
3. check if requiremnt file `/var/lib/tomcat7/webapps/perntaho-reports-for-openerp.war` exists
4. install at destination using `wget http://toolkt.com/files/pentaho-reports-for-openerp`

# Pentaho Reports Parameter Usage
type | value
--- | ---
name | ids
label | ids
value typye | object

