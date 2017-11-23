### Pentaho query sample
```sql
SELECT
	purchase_order.id, --- important to separate reports
	ROW_NUMBER() OVER(PARTITION BY purchase_order.id) as prod_no,
	purchase_order_line.name as product_name,
	purchase_order_line.product_qty as product_qty,
	purchase_order_line.product_uom as product_uom,
	purchase_order_line.order_id as order_id,
	purchase_order.name as quote_no,
	TO_CHAR(purchase_order.date_order, 'dd-Mon-YYYY') as order_date,
	res_company.name as company_name,
	decode(replace(encode(res_company.logo_web,'escape')::text,E'\012',''),'base64') as company_logo,
	subquery.name as user_name
FROM purchase_order
INNER JOIN purchase_order_line ON purchase_order.id = purchase_order_line.order_id
INNER JOIN res_company ON purchase_order.company_id = res_company.id
INNER JOIN
	(SELECT res_partner.name, res_users.id
		FROM res_partner
		INNER JOIN res_users ON res_users.partner_id = res_partner.id) as subquery
	ON subquery.id = purchase_order.create_uid
WHERE purchase_order.id in (2,3,6)
```
