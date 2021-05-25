Modificamos el addon path en el archivo de configuración de odoo:

    $ sudo cat /etc/odoo/odoo.conf
    
    [options]  
    ; This is the password that allows database operations:  
    ; admin_passwd = admin  
    db_host = False  
    db_port = False  
    db_user = odoo  
    db_password = False  
    addons_path = /home/ubuntu/odoo-dev/addons

> En mi caso estoy apuntando a la carpeta addons en odoo-dev 


Una vez finalizada la edición del archivo, reiniciamos odoo:

    sudo systemctl restart odoo
Si todo ha ido correctamente podemos crear el esqueleto de nuestro módulo con:

    odoo scaffold /home/ubuntu/odoo-dev/addons/modulAutomatic

> Con scaffold, la ruta que hemos asignado en el archivo de configuración y el nombre que le queremos dar al módulo, podemos crear las bases. 

Con tree vemos si todo ha funcionado:

    $ tree /home/ubuntu/odoo-dev/addons/modul_automatic/
    
    /home/ubuntu/odoo-dev/addons/modul_automatic/  
    ├── __init__.py  
    ├── __manifest__.py  
    ├── controllers  
    │ ├── __init__.py  
    │ └── controllers.py  
    ├── demo  
    │ └── demo.xml  
    ├── models  
    │ ├── __init__.py  
    │ └── models.py  
    ├── security  
    │ └── ir.model.access.csv  
    └── views  
    ├── templates.xml  
    └── views.xml  
      
    5 directories, 10 files
Habilitamos el modo desarollador en:

    Ajustes>Herramientas desarrollo>Activar modo desarrollador
Ahora vamos a `Aplicaciones` y en el panel de búsqueda quitamos el primer filtro y escribimos el nombre de nuestro modulo:

    modul_automatic
Empezamos a añadir código, nos vamos a `Models/models.py` y añadimos lo siguiente:
```python
# -*- coding: utf-8 -*-

from odoo import models, fields

COLOR = [
	('negre', 'Negre'),
	('plata', 'Plata'),
	('gris', 'Gris'),
]

class Component(models.Model):
	_name = 'gestio.component'
	name = fields.Char(string='Nom del component', required=True)
	marca = fields.Many2one('gestio.marca', string='Marca')
	color = fields.Selection(COLOR, string='Color', default='negre')
	is_offer = fields.Boolean('Té descompte?')
```
En `Models/__init__.py`:

    $ cat /home/ubuntu/odoo-dev/addons/modul_automatic/models/__init__.py 
    
    # -*- coding: utf-8 -*-
    
    from . import models
Y en `views/templates.xml`:
```xml
<?xml version="1.0" encoding="utf-8" ?>
<odoo>
    <record id="view_component_form" model="ir.ui.view">
        <field name="name">gestio.component.form</field>
        <field name="model">gestio.component</field>
        <field name="arch" type="xml">
            <form string="Component">
                <sheet>
                    <div class="oe_title">
                        <h1>
                            <table>
                                <tr>
                                    <td style="padding-right:10px">
                                        <field name="name" required="1" placeholder="Marco 1DIN"/>
                                    </td>
                                </tr>
                            </table>
                        </h1>
                    </div>
                    <notebook colspan="4">
                        <page name="dades_component" string="Informació del component">
                            <group col="4" colspan="4" name="info_component">
                                <field name="marca"/>
                                <field name="color"/>
                                <field name="is_offer"/>
                            </group>
                        </page>
                    </notebook>
                </sheet>
            </form>
        </field>
    </record>
    <record id="view_component_tree" model="ir.ui.view">
        <field name="name">gestio.component.tree></field>
        <field name="model">gestio.component</field>
        <field name="arch" type="xml">
            <tree string="Gestió de components">
                <field name="name"></field>
                <field name="marca"></field>
                <field name="color"></field>
                <field name="is_offer"></field>
            </tree>
        </field>
    </record>
    <record id="action_view_component" model="ir.actions.act_window">
        <field name="name">Gestió de components</field>
        <field name="res_model">gestio.component</field>
        <field name="view_mode">tree,form</field>
        <field name="domain">[]</field>
        <field name="help" type="html">
            <p class="oe_view_nocontent_create">Crea un nou component</p>
        </field>
    </record>
    <record id="action_view_marca" model="ir.actions.act_window">
        <field name="name">Gestió de marques</field>
        <field name="res_model">gestio.marca</field>
        <field name="view_mode">tree,form</field>
        <field name="domain">[]</field>
        <field name="help" type="html">
            <p class="oe_view_nocontent_create">Crea una nova marca></p>
        </field>
    </record>
    <menuitem id="menu_root" name="Gestió de components" sequence="1"/>
    <menuitem id="sub_menu_component" name="Components" sequence="1" parent="menu_root" action="action_view_component"/>
    <menuitem id="sub_menu_marca" name="Marques" parent="menu_root" action="action_view_marca"></menuitem>
</odoo>
```
Si todo a salido como deberia, podremos instalar el módulo y en `Ajustes>Técnico>Modelos` y buscamos `gestio` podremos ver nuestro módelo con los elementos que hemos creado.

![imatge](https://user-images.githubusercontent.com/61594022/119538019-f3e5eb00-bd8a-11eb-9e1c-902bfce0b111.png)
