### 字段根据context传值进行筛选
例如采购订单中根据选中的供应商，筛选产品

>- view代码：

```xml
<field name="product_id"
attrs="{'readonly': [('state', 'in', ('purchase', 'to approve','done', 'cancel'))]}"
context="{'vendor_id':parent.partner_id,'partner_id':parent.partner_id, 'quantity':product_qty,'uom':product_uom, 'company_id': parent.company_id}"
force_save="1"/>
```
>- py代码

```python
@api.model
    def name_search(self, name='', args=None, operator='ilike', limit=100):
    ......
      if self._context.get('vendor_id'):
          vendor = self.env['product.supplierinfo'].search([('name', '=', self._context.get('vendor_id'))])
          if vendor:
              products = self.search([('product_tmpl_id.seller_ids', 'in', vendor.ids)], limit=limit)
      return products.name_get()
```

**原文代码中的partner_id是用于输入名字后的搜索，如果无法匹配产品名称则在供应商的产品名中匹配**  
### 此处在context 增加 vendor_id ,用于显示只由此供应商供应的产品
