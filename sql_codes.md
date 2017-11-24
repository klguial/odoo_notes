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
