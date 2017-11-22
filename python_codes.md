## Manifest file input
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

## `ir.model.access.csv` sample
id | name | model_id:id | group_id:id | perm_read | perm_write | perm_create | perm_unlink
--- | --- | --- | --- | --- | --- | --- | ---
access_csr_dmpi_po_distributor | csr.dmpi.po.distributor | model_dmpi_po_distributor
 | group_dmpi_po_csr | 1 | 1 | 0 | 0
