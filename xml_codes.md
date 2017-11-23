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
