## Odoo reminders
### Odoo installation 
Softawares:  
- VMWare Workstation 14 Player
- Ubuntu Odoo Image File (Ubuntu64-14.04.5)
- Pentaho Report Designer
run Ubuntu Odoo Image file on VMWare.  
`cd` to `/opt/files/`  
download odoo `wget toolkt.com/files/odoo/odoo10.sh` or `odoo9.sh` or `odoo.sh` (odoov8)  
configure settings. comment out lines for postgresql installation  
run bash file `./odoo10.sh`  
go to **192.168.10.100:1069** to open odoo  

### Virtual Machine Settings
Memory - 1GB  
Processors - 1  
Hard Disk - 100GB  
CD/DVD - Using unknown backend  
Network Adapter - NAT (share host's IP address)  
Shared Folders - Add Folder path  
** shared folder viewed at /mnt/hgfs/  


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

### DEL MONTE
  
10.0 - https://purchase.delmontecsg.com  
	db dmpi_purchase_automation -u admin -p @DM!N123  
	(DMPI Distributors Ordering Module)  
  
8.0 - http://odoo1a.delmontecsg.com:8069/  
	db dmpi_clean -u admin -p odoomgr  
	(DMPI Distributors Sales, Purchasing, Warehouse)  
  
10.0 - http://odoo1a.delmontecsg.com:1069/  
	db dmpi_central_database -u admin -p admin123  
	(DMPI Central for managing Distributor Odoo Databases)  
  
Database Master Password -> P@SSW0RD  
  
### RESOURCES  
  
PO Allocation Upload File  
Dist Odoo User Manual  
PO Automation Encoder's Manual  
  
PO Automation  
	Purchase Automation Process  
Dist Odoo  
	Sales Process  
DMPI Central  

### TO-Do
1. Check if server_date near ExpDD and status is still Cofirmed Delivery
	- create CRON automated process
	- query data given the conditions
	- add mail message with POs having the same condition
	- check if inland/offshore
2. Check also for "For DRc conf" and "For Del conf" status

### Pentaho Reports for OpenERP Installation
1. add pentaho-reports folder in the same directory as the new module
2. install manually
3. check if requiremnt file `/var/lib/tomcat7/webapps/perntaho-reports-for-openerp.war` exists
4. install at destination using `wget http://toolkt.com/files/pentaho-reports-for-openerp.war` (for odoo8 pentaho reports) or here `http://cloud1.willowit.com.au/dist/pentaho-reports-for-openerp.war` (for odoo9 and odoo10)

### Pentaho Reports Parameter Usage
key | value
--- | ---
name | ids
label | ids
value typye | object

### Pentaho Reports System Parameters
key | value
--- | ---
pentaho.postgres.host | localhost
pentaho.postgres.port | 5432
pentaho.postgres.login | odoomgr
pentaho.postgres.password | odoomgr

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
4 | **stock** (Warehouse Management) | stock, stock_account, product
5 | **purchase** (Purchase Management) | purchase
6 | **purchase_requisitions** (Purchase Requisitions) | purchase_requisition
7 | **sale** (Sales) | sale, portal_sale,  portal_stock, sale_mrp, sale_stock  
8 | **mrp** (Manufacturing Resource Planning) | mrp  
9 | **hr** (Human Resources/Employee) | hr  

*Install First before adding on dependencies in the module to remove unwanted additional views/menus*

Upon installation of **account_accountant**, three app views appear, *messaging*, *accounting*, and *reporting*.  
Install **account_asset** and **account_budget** to have menus for assets and budgest under accounting.  
Upon installation of **purchase**, app view *purchases* appear. Under Purchase Manu are Requests for Quotation, Purchase Orders, and Suppliers. Install also **purchase_requisition** to have a Calls for Bid menu under Purchase/Purchase menu. Warehouse app also appears.

### Procurement-to-Pay process flow
1. **Requestor** requests products to be purchased from purchasing department by creating an *Online Purchase Requisition Slip (PRS)* .
2. Once the PRS is approved by the **Chief Operating Officer (COO) / Operations Manager (OM)**, a *Request for Quotation (RFQ)* which will be send via email to different suppliers.
3. Upon receiving a confirmation from the suppliers regarding the RFQ, the details in the RFQ can be updated. A *Canvassing Form (CF)* will be made to show the prices of a product in different suppliers.
4. The COO/OM will choose among the supplier based on the final bid.
5. Then a *Purchase Order (PO)* draft is submitted to be approved by the Purchasing Manager (COO/OM)

### pre-installed modules upon creation of db
base, base_import  
im_chat, im_odoo_support  
web_apps (kanban, graph, etc)  
report, bus, auth_crypt  


### get model_ids in db table ir_model_data



## python_codes
### Manifest file input
```python
# -*- coding: utf-8 -*-
{
    'name': "Trident Reports",

    'summary': """""",

    'description': """
    """,

    'author': "klguial",
    'website': "http://www.toolkt.com",
    'category': 'Test',
    'version': '0.1',
    'depends': ['base'],
    'data': [
        # 'security/ir.model.access.csv',
        'views/trident.xml',
        'reports/reports.xml',
    ],
}
```

### `ir.model.access.csv` sample
id | name | model_id:id | group_id:id | perm_read | perm_write | perm_create | perm_unlink
--- | --- | --- | --- | --- | --- | --- | ---
access_csr_dmpi_po_distributor | csr.dmpi.po.distributor | model_dmpi_po_distributor | group_dmpi_po_csr | 1 | 1 | 0 | 0
access_opn_dmpi_po_distributor | opn.dmpi.po.distributor | model_dmpi_po_distributor | group_dmpi_po_opn | 1 | 0 | 0 | 0

### domains sample
```python
[('name','=','ABC'),
 ('language.code','!=','en_US'),
 '|',('country_id.code','=','be'),
     ('country_id.code','=','de')]
```
is interpreted as
```sql
    (name is 'ABC')
AND (language is NOT english)
AND (country is Belgium OR Germany)
```

### domains for records with NULL value
In python there is no `null` value. Instead, we can use `None` when writing openerp domains.  
Note that by default, python domain `False` get records for both `NULL` or `False` values.

### using `self.env[]` vs using sql in python
```python
productVariants = self.env['product.product']
products = productVariants.search([])
for rec in products:
     print "product name ..", rec.name
```
is similar to 
```python
cr.execute("select * from product_product")
products = cr.dictfetchall()
for rec in products:
    print rec['name']
```

### pad with zeros
```python
number = 24
'{0:05d}'.format(number)
```
output `'00024'`. See [Format Specification Mini-Language](https://docs.python.org/3.4/library/string.html#format-specification-mini-language)

### default reference number
```python
_name = 'model.name'
name = fields.Char('Ref Number', default=lambda self: self.get_ref_no(), requried=True)

@api.model
def get_ref_no(self):
	self._cr.execute('SELECT max(id)+1 FROM mode_name')
	no = self._cr.fetchone()[0] or 1
	ref_no = 'PRS%s' % '{0:05d}'.format(no)
	return ref_no
```
Note: check whether it still works without the @api.model decorator

### create purchase_form and product_line relationship model
```python
class PurchaseForm(models.Model):
	_name = 'purchase.form'
	name = fields.Char('Form')
	procurement_state = fields.Selection([
    		('draft','New'),
    		('ops_mgr','Operations Manager'),
    		('purchase','Purchasing')], 'Procurement State')
	product_line_ids = fields.One2many('product.line', 'purchase_id', 'Products', copy=True)

class ProductLine(models.Model):
	_name = 'product.line'
	
	purchase_id = fields.Many2one('purchase.form', string='Purchase Form ID', ondelete='cascade')
	product_id = fields.Many2one('product.product', string='Product')
	qty = fields.Integer('Quantity')

class Product(models.Model):
	_name = 'product.product'
	name = fields.Char('Name')
	desctiption = fields.Text('Description')
```

### ORM api
**Get access to another model**
```python
self.env['model.name'] (in new api)
self.pool.get('model.name') ('in old api)
```
**Create record in another model**
```python
self.env['another.model'].create({'key1':'val','key2':val})
```
**Update record in another model**
```python
self.env['another.model'].write(x, {'key1':'val','key2':val})
```
**Create new record with one2many values**
```python
invoice_line_1 = {
   'name': 'line description 1',
   'price_unit': 100,
   'quantity': 1,
}

invoice_line_2 = {
   'name': 'line description 2',
   'price_unit': 200,
   'quantity': 1,
}

invoice = {
   'type': 'out_invoice',
   'comment': 'Invoice for example',
   'state': 'draft',
   'partner_id': 1,
   'account_id': 19,
   'invoice_line': [
       (0, 0, invoice_line_1),
       (0, 0, invoice_line_2)
   ]
}
```

where `x` is an id
**Using raw sql query**
```python
sql_record_cst = 'INSERT INTO modelB (curr_day, ord_proc) values(%s, 1)'
self.env.cr.execute(sql_record_cst,[self.fieldinmodelA])
self.env.cr.commit()
```

## sql_codes
### Pentaho query sample
```sql
SELECT
	p_ord.id,
	p_ord.name quote_no,
	ROW_NUMBER() OVER(PARTITION BY p_ord.id) prod_no,
	TO_CHAR(p_ord.date_order, 'dd-Mon-YYYY') order_date,
	p_ord_l.name product_name,
	p_ord_l.product_qty product_qty,
	p_ord_l.product_uom product_uom,
	p_ord_l.order_id order_id,
	comp.name company_name,
	decode(replace(encode(comp.logo_web,'escape')::text,E'\012',''),'base64') company_logo,
	sqry.name user_name
FROM purchase_order p_ord
INNER JOIN purchase_order_line p_ord_l ON p_ord.id = p_ord_l.order_id
INNER JOIN res_company comp ON p_ord.company_id = comp.id
INNER JOIN
	(SELECT part.name, usr.id
	 FROM res_partner part
	 INNER JOIN res_users usr ON usr.partner_id = part.id) as sqry
	ON sqry.id = p_ord.create_uid
WHERE p_ord.id in (${ids})
```

### get data type information of columns of a table
```sql
select column_name, data_type from information_schema.columns
where table_name = 'product_template';
```

## xml_codes
### create pentaho report
```xml
<record id="pur_req_report" model="ir.actions.report.xml">
    <field name="name">Pentaho Purchase Requisition Slip</field>
    <field name="report_name">Purchase Requisition Slip</field>
    <field name="model">purchase.order</field>
    <field name="pentaho_report_model_id" ref="model_trident_pur_req"/>
    <field name="rml">trident/reports/purchase_requisition.prpt</field>
    <field name="pentaho_filename">trident/reports/purchase_requisition.prpt</field>
    <field name="pentaho_report_output_type">pdf</field>
    <field name="report_type">pentaho</field>
    <field name="auto" eval="False"/>
    <field name="is_pentaho_report" eval="True"/>
    <field name="pentaho_load_file" eval="True"/>
</record>
```

### create menu
```xml
<menuitem id="trident_pur_req_main_menu" name="Purchase Requisition" parent="hr.menu_hr_root" group="" sequence=""/>
```

### add action to menus
```xml
<record id="trident_pur_req_action" model="ir.actions.act_window">
    <field name="name">Purchase Requisition</field>
    <field name="res_model">trident.pur.req</field>
    <field name="view_type">form</field>
    <field name="view_mode">tree,form</field>
    <field name="help" type="html">
        <p class="oe_view_nocontent_create">Click to add a new Purchase Requisition</p>
    </field>
</record>
<menuitem id="trident_pur_req_menu" parent="trident_pur_req_main_menu" action="trident_pur_req_action"/>
```

### create form view
```xml
<record id="trident_pur_req_form" model="ir.ui.view">
    <field name="name">trident.pur.req.form</field>
    <field name="model">trident.pur.req</field>
    <field name="arch" type="xml">
        <form string="Purchase Requisition">
            <sheet>
                <h4>Calls for Bids Reference</h4>
                <field name="bid_ref"/>
                <group>
                    <group>
                        <field name="responsible_id"/>
                        <field name="bid_type"/>
                        <field name="rfq_ok"/<name />
                        <field name="purpose"/>
                        <field name="company_type"/>
                    </group>
                    <group>
                        <field name="req_date"/>
                        <field name="date_need"/>
                        <field name="source_doc"/>
                    </group>
                </group>
                <notebook>
                    <page string="Products">
                        <field name=""/>
                    </page>
                </notebook>
            </sheet>
        </form>
    </field>
</record>
```
### odoo button samples
```xml
<button name="apply_to_all_lc" icon="fa-refresh" class="oe_stat_button" string="Set to All" confirm="All records will have these same settings" type="object" />

<button name="create_po"  string="Confirm" type="object" class="oe_highlight" confirm="Are you sure you APPROVE of this step?"  attrs="{'invisible':[('status','!=','draft')]}"/>

<button string="Cancel" class="oe_link" special="cancel"/>
```

### status bar
```xml
<field name="status" widget="statusbar" statusbar_visible="draft,submitted,for_dr_conf_sending,for_dr_conf,confirmed_dr,for_delivery_conf,confirmed_del,enroute,received,done"/>
```
if status bar not showing, check if the field `status` has also been used within the same form.


### edit tree view
```xml
<record id="library_book_view_tree" model="ir.ui.view">
    <field name="name">library.book.tree</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">
        <tree>
            <field name="name"/>
            <field name="date_release"/>
        </tree>
    </field>
</record>
```

### add search view
```xml
<record id="library_book_view_search" model="ir.ui.view">
    <field name="name">Library Book Search</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">
        <search>
            <field name="name"/>
            <field name="date_release"/>
            <field string="No Authors" domain="[('author_ids','=','False')]"/>
        </search>
    </field>
</record>
```

### inherit a view
```xml
<record model="ir.ui.view" id="partner_instructor_form_view">
    <field name="name">partner.author_publishers</field>
    <field name="model">res.partner</field>
    <field name="inherit_id" ref="base.view_partner_form"/>
    <field name="arch" type="xml">
        <notebook position="inside">
            <page string="Library">
                <group>
                    <field name="published_book_ids" widget="one2many_tags"/>
                    <field name="authored_book_ids" widget="many2many_tags"/>
                    <field name="count_auth_books" readonly="True"/>
                </group>
            </page>
        </notebook>
    </field>
</record>
<record id="library_res_partner_action" model="ir.actions.act_window">
    <field name="name">Authors and Publishers</field>
    <field name="res_model">res.partner</field>
    <field name="view_mode">tree,form</field>
</record>
```

### html label view
```xml
<div class="pull-left">
    <label for="name" class="oe_edit_only oe_inline"/>
    <h1>
	<field name="name" class="oe_inline" attrs="{'readonly': [('state','not in',('draft'))]}"/>
    </h1>
</div>
<div class="pull-left">
```

### one2many editable
```xml
<page string="Products">
    <field name="product_line_ids"> 
	<tree editable='bottom'>
	    <field name="product_id"/>
	    <field name="product_qty"/>
	    <field name="schedule_date"/>
	    <!-- <field name="uom"/> -->
	</tree>
    </field>
</page>
```

### Automatically grouped tree view
In your action, add context field
```xml
<field name="context">{'group_by': 'employee_id'}</field>
```
Or first create a filter in your search view
```xml
<filter name="group_employee_id" string="Category" icon="terp-partner" context="{'group_by':'employee_id'}"/>
```
And use it in the context field
```xml
<field name="context">{'search_default_group_employee_id': 1}</field>
```







