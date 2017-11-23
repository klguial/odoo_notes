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
output `'00024'`



