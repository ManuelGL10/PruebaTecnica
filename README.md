# PruebaTecnica

Aplicación de gestión de productos desarrollada con Vue y Vuetify. Permite registrar, editar y eliminar productos, así como visualizar e identificar rápidamente productos por vencer y vencidos. Los datos se almacenan en el navegador mediante localStorage.

# Instrucciones 

Antes de ejecutar la aplicación, asegúrate de tener instalado Node.js y npm. Puedes verificarlo con los siguientes comandos:

node -v 
npm -v 

1- Clona o descarga el respositorio desde GitHub.
2- Accede a la carpeta del proyecto desde la terminal.

  cd PruebaTecnica

3- Instala las dependencias necesarias para ejecutar el proyecto. 

 Dependendicas Basicas: npm install
 Instala Vurtiy: npm install vuetify 
 Instala Vue Router y plugin de Vuetify: npm install vue-router vite-plugin-vuetify
 Instala los íconos de Material Design: npm install @mdi/font

 4- Ejucuta el proyecto

 npm run dev

# Funcionalidad 

Al iniciar la aplicación, no habrá productos registrados. Se mostrará una tabla vacía donde se visualizarán los productos conforme se vayan agregando.

Haz clic en el botón "Agregar Producto" para registrar un nuevo producto. Esto abrirá un formulario en una ventana modal. El producto solo se registrará si todos los campos están completos y son válidos.

Una vez registrado, el producto se mostrará en la tabla con los siguientes datos:
    Nombre
    Cantidad
    Precio
    Fecha de vencimiento
También contará con acciones para editar y eliminar.

Cada producto mostrará su estado de vencimiento mediante un recuadro de color:

Rojo: Producto vencido.
Amarillo: Producto por vencer (dentro de los próximos 7 días).
Verde: Producto vigente.

# Persistencia de Datos

La aplicación utiliza localStorage para almacenar los productos de forma local en el navegador. Los datos se conservan incluso si se recarga o se cierra la página.

Cada vez que se agrega, edita o elimina un producto, la lista se actualiza y se vuelve a guardar automáticamente en localStorage.


# Calculo de productos por vencer

La aplicación evalúa las fechas de vencimiento de cada producto comparándolas con la fecha actual. Este cálculo determina si un producto:

    Ya venció.
    Está por vencer (dentro de los próximos 7 días).
    Se encuentra en estado normal.


Se obtiene la fecha actual.Se convierte la fecha de vencimiento del producto a un objeto Date, calcula la diferencia en milisegundos entre ambas fechas, esa diferencia se convierte a días. Con base al resulta se determina el estado del producto

    Si es menor a 0: el producto ya venció.
    Si es menor o igual a 7: el producto está por vencer.
    Si es mayor a 7: el producto está en buen estado.