### ODOO static/description 中文乱码
** 源码位置 **
<pre>
@api.depends('name', 'description')
    def _get_desc(self):
        for module in self:
            path = modules.get_module_resource(module.name, 'static/description/index.html')
            if path:
                with tools.file_open(path, 'rb') as desc_file:
                    doc = desc_file.read()
</pre>
增加decode
<pre>
doc = desc_file.read().decode('utf-8')
</pre>