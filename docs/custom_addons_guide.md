# Creating Custom Addons for Odoo 18

This guide explains how to create and develop custom addons for Odoo 18 using the Docker setup.

## Table of Contents
1. [Basic Addon Structure](#basic-addon-structure)
2. [Creating a New Addon](#creating-a-new-addon)
3. [Essential Files](#essential-files)
4. [Model Development](#model-development)
5. [Views and Templates](#views-and-templates)
6. [Security and Access Rights](#security-and-access-rights)
7. [Testing Your Addon](#testing-your-addon)
8. [Best Practices](#best-practices)

## Basic Addon Structure

A typical Odoo addon has the following structure:
```
custom_addons/
└── my_module/
    ├── __init__.py
    ├── __manifest__.py
    ├── models/
    │   ├── __init__.py
    │   └── model_name.py
    ├── views/
    │   └── views.xml
    ├── security/
    │   └── ir.model.access.csv
    └── static/
        ├── description/
        │   └── icon.png
        └── src/
            ├── js/
            └── xml/
```

## Creating a New Addon

1. Create a new directory in `addons/custom_addons/`:
```bash
mkdir -p addons/custom_addons/my_module
```

2. Create the basic files:
```bash
touch addons/custom_addons/my_module/__init__.py
touch addons/custom_addons/my_module/__manifest__.py
```

## Essential Files

### 1. __manifest__.py
```python
{
    'name': 'My Module',
    'version': '1.0',
    'category': 'Uncategorized',
    'summary': 'Short description of the module',
    'description': """
        Long description of the module's purpose
    """,
    'author': 'Your Name',
    'website': 'https://www.yourwebsite.com',
    'depends': ['base'],
    'data': [
        'security/ir.model.access.csv',
        'views/views.xml',
    ],
    'demo': [],
    'installable': True,
    'application': False,
    'auto_install': False,
    'license': 'LGPL-3',
}
```

### 2. __init__.py
```python
from . import models
```

### 3. models/__init__.py
```python
from . import model_name
```

## Model Development

### Basic Model Example (models/model_name.py)
```python
from odoo import models, fields, api

class MyModel(models.Model):
    _name = 'my_module.my_model'
    _description = 'My Model Description'

    name = fields.Char(string='Name', required=True)
    description = fields.Text(string='Description')
    date = fields.Date(string='Date')
    state = fields.Selection([
        ('draft', 'Draft'),
        ('confirmed', 'Confirmed'),
        ('done', 'Done')
    ], string='Status', default='draft')

    @api.model
    def create(self, vals):
        # Custom create logic
        return super(MyModel, self).create(vals)

    def action_confirm(self):
        self.write({'state': 'confirmed'})
```

## Views and Templates

### Basic View Example (views/views.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <!-- Tree View -->
    <record id="view_my_model_tree" model="ir.ui.view">
        <field name="name">my.model.tree</field>
        <field name="model">my_module.my_model</field>
        <field name="arch" type="xml">
            <tree>
                <field name="name"/>
                <field name="date"/>
                <field name="state"/>
            </tree>
        </field>
    </record>

    <!-- Form View -->
    <record id="view_my_model_form" model="ir.ui.view">
        <field name="name">my.model.form</field>
        <field name="model">my_module.my_model</field>
        <field name="arch" type="xml">
            <form>
                <sheet>
                    <group>
                        <field name="name"/>
                        <field name="description"/>
                        <field name="date"/>
                        <field name="state"/>
                    </group>
                </sheet>
                <div class="oe_button_box" name="button_box">
                    <button name="action_confirm" type="object" string="Confirm" class="oe_highlight"/>
                </div>
            </form>
        </field>
    </record>

    <!-- Action -->
    <record id="action_my_model" model="ir.actions.act_window">
        <field name="name">My Model</field>
        <field name="res_model">my_module.my_model</field>
        <field name="view_mode">tree,form</field>
        <field name="help" type="html">
            <p class="o_view_nocontent_smiling_face">
                Create your first record!
            </p>
        </field>
    </record>

    <!-- Menu Item -->
    <menuitem id="menu_my_model"
        name="My Model"
        parent="base.menu_custom"
        action="action_my_model"
        sequence="10"/>
</odoo>
```

## Security and Access Rights

### Access Rights (security/ir.model.access.csv)
```
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
access_my_model_user,my.model.user,model_my_module_my_model,base.group_user,1,1,1,1
```

## Testing Your Addon

1. Install the module:
```bash
docker compose exec web odoo -d your_database -i my_module --stop-after-init
```

2. Update the module:
```bash
docker compose exec web odoo -d your_database -u my_module --stop-after-init
```

## Best Practices

1. **Naming Conventions**
   - Use snake_case for module names
   - Prefix custom models with your module name
   - Use clear and descriptive names

2. **Code Organization**
   - Keep models small and focused
   - Use separate files for different models
   - Group related functionality together

3. **Security**
   - Always define access rights
   - Use record rules when needed
   - Follow the principle of least privilege

4. **Performance**
   - Use `@api.model` for class methods
   - Use `@api.multi` for recordset methods
   - Avoid unnecessary database queries

5. **Documentation**
   - Document all methods and classes
   - Include examples in docstrings
   - Keep README files updated

## Common Issues and Solutions

1. **Module Not Appearing in Apps List**
   - Check if the module is in the correct directory
   - Verify `__manifest__.py` is correct
   - Check if the module is properly installed

2. **Database Connection Issues**
   - Verify PostgreSQL is running
   - Check database credentials
   - Ensure proper volume mappings

3. **View Loading Errors**
   - Check XML syntax
   - Verify field names match model
   - Check view inheritance

## Additional Resources

- [Odoo Documentation](https://www.odoo.com/documentation/18.0/)
- [Odoo Development Cookbook](https://www.odoo.com/documentation/18.0/developer/howtos/backend.html)
- [Odoo Community Association](https://odoo-community.org/) 