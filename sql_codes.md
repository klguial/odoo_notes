### Pentaho query sample
```sql
SELECT
    ROW_NUMBER() OVER(ORDER BY purchase_order_line.name ASC) AS prod_no,
    TO_CHAR(purchase_order.create_date, 'dd-Mon-YYYY') AS create_date,
    purchase_order_line.name AS product_name,
    purchase_order_line.product_qty AS product_qty,
    purchase_order_line.product_uom AS product_uom,
    purchase_order_line.order_id AS order_id,
    purchase_order.name AS quote_no,
    -- change defualt date format
    TO_CHAR(purchase_order.date_order, 'dd-Mon-YYYY') AS date_need,
    res_company.name AS company_name,
    -- for displaying dynamic images in pentaho
    -- 'string'::TEXT means to CAST
    decode(replace(encode(res_company.logo_web,'escape')::TEXT,E'\012',''),'base64') AS company_logo,
    subquery.name as user_name
FROM purchase_order
INNER JOIN purchase_order_line ON purchase_order.id = purchase_order_line.order_id
INNER JOIN res_company ON purchase_order.company_id = res_company.id
INNER JOIN
    (SELECT res_partner.name, res_users.id
        FROM res_partner
        INNER JOIN res_users ON res_users.partner_id = res_partner.id) as subquery
    ON subquery.id = purchase_order.create_uid
WHERE purchase_order.id in (${ids}) --- ${ids} calls the parameter ids set in pentaho RD
```
