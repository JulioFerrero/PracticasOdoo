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
