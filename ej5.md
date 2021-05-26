
Modificamos `models` con el siguiente código:
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


class Marca(models.Model):
	_name = 'gestio.marca'
	name = fields.Char(string='Nombre', required=True)
	nac_fab = fields.Many2one('res.country', string='Nacionalitat')
```

> Tenemos que crear `Marcas` y añadirla en `component` en una relación Many2one

Creamos un nuevo componente para poder realizar las herencias, oseasé copiamos el modulo actual con los archivos con diferentes nombre:
De `component.py` a `component_preu.py`

Añadimos este nuevo código al archivo recién modificado:
```python
# -*- coding: utf-8 -*-

from odoo import models, fields

COLOR = [
	('negre', 'Negre'),
	('plata', 'Plata'),
	('gris', 'Gris'),
]

class ComponentPreu(models.Model):
	_inherit = 'gestio.component'
	precio = fields.Float(string='Preu del component')
	color = fields.Selection(COLOR, string='Color', default='gris')
```
Instalamos el nuevo modulo y en el desplegable podremos ver `venta.component`
