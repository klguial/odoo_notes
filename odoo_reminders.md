### Odoo installation 
Softawares:  
- VMWare Workstation 14 Player
- Ubuntu Odoo Image File (Ubuntu64-14.04.5)
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

### about Trident
built-in modules needed:  

order of installation | module name | included modules
--- | --- | ---
1 | **account_accountant** (Accounting and Finance) | account_accountant, account_voucher
2 | **account_asset** (Assets Management) | account_asset
3 | **account_budget** (Budgets Management) | account_budget
4 | **stock** (Warehouse Management) | stock, stock_account, purchase, purchase_requisition  
5 | **purchase** (Purchase Management) | purchase
6 | **purchase_requisitions** (Purchase Requisitions) | purchase_requisition
7 | **sale** (Sales) | sale, portal_sale,  portal_stock, sale_mrp, sale_stock  
8 | **mrp** (Manufacturing Resource Planning) | mrp  
9 | **hr** (Human Resources/Employee) | hr  

Upon installation of **account_accountant**, three app views appear, *messaging*, *accounting*, and *reporting*.  
Install **account_asset** and **account_budget** to have menus for assets and budgest under accounting.  
Upon installation of **purchase**, app view *purchases* appear. Under Purchase Manu are Requests for Quotation, Purchase Orders, and Suppliers. Install also **purchase_requisition** to have a Calls for Bid menu under Purchase/Purchase menu. Warehouse app also appears.

Procurement-to-Pay process flow
1. Create Purchase Request slip under HR/Purchase Requisition/Purchase Requisition. The draft is to be approved by Operations Manager.
2. All submitted requests can be viewes under Purchase/Purchase/Calls for bids




