This is a documentation of the workflow of the pruchase automation of Del Monte.  
The file structure is shown below.  
```
dmpi-po-automation-master
│   .gitignore
│   README.md
│   .DS_Store
│
└───backend_theme_v10  
└───dmpi_po_central
```
Before anythin else, do this to disable communication with SAP db
### Del Monte SAP Config (disable SAP Middleware)
Host 0.0.0.0 (202.129.238.22)  
Disable CRON scheduled actions from dmpi_po_central  

### Del Monte Abbreviations
CSR - Customer Service Representative  
CRP - Continuous Replenishment Program  
DR - Delivery receipt


### Main views
`sale_dist_view.xml` (For distributor PO to-do's), `sale_view.xml` (For CSR PO to-do's) and `central_view.xml` for configurations such as allocation, products, plants etc.  

### PO Automation Notes
- Allocation -> allocated products to distributor on a monthly basis (date start, date end). details include product name, product source (plant), number of products allocated (allocation), cases

- ODOO STATUS
	- draft
	- submitted
	- for_dr_conf_sending
	- for_dr_conf
	- confirmed_dr
	- for_delivery_conf
	- confirmed_del
	- enroute
	- received
	- done
	- cancelled
	
- CRON functions
	- cron_send_files
	- cron_execute_queue
	- cron_create_files
	- cron_read_files
	- cron_get_remote_files
	- cron_send_received (removed)

- **Draft** Status
	- (NOTHING: for PO Drafts not submitted within RDD month, must not be proceeded)
	- PO object is `dmpi.po.sale` with inherited properties from `mail.thread` and `ir.needaction_mixin`
	- Upon creation, ODOO PO number is created.
		- Check function `create()` under `dmpi.po.sale` model
	- On creating Purchase order, required fields must first be set before being able to choose the allocated products.
		- Required Fields: Dsitributor, Ship To, Plant, Source, Distributor Channel, Reference No
		- `line_ids`: related to `dmpi.po.sale.line` object. Has the specifications on product qty, allocation price etc. 
	- Load configuration: CV (container van) or 10 Wheeler. Also depends on source location. 
		- CV for offshore
		- 10Wheel for inland
		- **Check function `on_change_deliver_source()`**
	- Cannot submit PO unless load requirements are met
	- Truck Load and Truck Load Weight
		- `max_truck_load_wieght_kg = 15000`. set at `dmpi.po.config`
		- `max_van_load_wieght_kg = 18000` (container van). set at `dmpi.po.config`
		- `load_configuration`: either CV or 10W
		- max and min `truck_load` (%) and max and min `truck_load_weight` are the constraints
		- Truck Load (%) refers to the space occupied by the products (simply called load)
		- Truck Load Weight (%) refers to the weight of the products (simply called weight)
		- Allocation is per integers of pallet. Each product has a given number of cases per pallet.
		- `weight = 100 * product_weight * qty_ordered / max_truck_weight`
		- `load = 100 * qty / allowed_cases`
		- **Check function `_get_truck_load()`**
	- Upon submission of PO, PO Dates adjust based on Source type (inland = 5 days or offshore = 15 days). 
		- **Check function `submit_po()`**
		- **Status to `submitted`**

- **Submitted** Status
	-  Po is submitted to SAP
	- An SO (sales order) is created for PO's with submitted status. 
	- Relevant information from the PO is prepared to a csv file with sampel name **`so_create_P000002084_20171130.csv`**.
	- CSV file is transferred to remote server
	- **Check function `so_create()`. **SO to SAP (`sent_to_sap_so: True`)**
	- SAP will then read the file in return would send back a DR (delivery receipt) together with its details such as:
		- sap_so_no
		- dr_no
		- dr_date
		- dr_lines (sap_line_no, odoo_line_no, product, dr_qty)
	- A cron task that read files is automatically. ODOO will read the outbound files from SAP. **Check CRON function `_cron_read_files()`**
	- Under the cron task is function for reading the DR. **Check function `read_dr()`.
		- the DR details will be read and necessary changes in the PO will be made such as the summary and DR details
		- **Status to `for_dr_conf_sending`**
	
- **For DR Sending** Status
	- CSR has already issued DR and waiting for full allocation
	- CSR has already issued DR for full allocation
	- If everything is OK, CSR will send the PO to distributor using the action **`send_dr_conf()`**
		- if user has id=47
			- ODOO will create a purchase oreder of object `purchase.order`
			- ODDOO will also create the products ofobject `product.product`
			- **Status to `for_dr_conf_sending`**
		- else
			- just change the **Status to `for_dr_conf_sending`**
		- **SO to SAP (`sent_to_dist_po_conf: True`)**

- **For DR Confirmation** Status
	- P000003414 (sample PO)
	- CSR has already determined that the PO is allocated and submitted for Distributor Approval
	- The distributor must verify the summary tab if the PO qty vs DR qty is acceptable.
	- status to `confirmed_dr` and `status_po_confirmed = True`
	- ODOO will send notification to SAP.
	- Create PO Conf `create_po_conf`
	- `sent_to_sap_po: True` (PO to SAP) update `sent_to_sap_po_date`
	- `status_po_confirmed` (PO Confirmed)
	
- **For Confirmed DR** Status
	- CSR will trigger status to `for_delivery_conf`
	- He can set RDD to be confirmed by distributor
	- 'status': "for_delivery_conf"
	- `send_to_dist_dr_conf: True` (DR Conf to Dist) update `send_to_dist_dr_conf`

- **For Delivery Confirmation** Status
	- Distributor must confirm the Exp RDD if acceptable
	- action `confirm_del`
	- status `confirmed_del`
	- `send_to_sap_dr: True` (DR to SAP) update `send_to_sap_dr_date`

- **Confirmed Delivery** Status
	- (ODOO Automation)
	- fill enroute
	- fill invoice
	- fill GR
	- Invoice From SAP
	- Invoice to Dist
	- Enroute to Dist

	
- **
