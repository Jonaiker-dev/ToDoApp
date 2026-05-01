# Guia de Implementacion JavaScript - TaskFlow

Esta guia detalla los pasos necesarios para darle vida a tu aplicacion de tareas utilizando JavaScript Vanilla. La estructura HTML y los estilos CSS ya estan preparados para ser manipulados.

## 1. Seleccion de Elementos Globales
Lo primero que debes hacer es referenciar todos los elementos del DOM que vas a manipular frecuentemente.

```javascript
// Contenedores principales
const listaTareas = document.getElementById('lista-tareas');
const formularioTarea = document.getElementById('formulario-tarea');
const estadoVacio = document.getElementById('estado-vacio');

// Entradas del formulario
const entradaTarea = document.getElementById('entrada-tarea');
const prioridadTarea = document.getElementById('prioridad-tarea');
const categoriaTarea = document.getElementById('categoria-tarea');
const fechaTarea = document.getElementById('fecha-tarea');

// Estadisticas
const totalTareas = document.getElementById('estadistica-total');
const tareasPendientes = document.getElementById('estadistica-pendientes');
const tareasCompletadas = document.getElementById('estadistica-completadas');
const barraProgreso = document.getElementById('relleno-progreso-estadistica');

// Modales y Notificaciones
const superposicionModal = document.getElementById('superposicion-modal');
const contenedorNotificacion = document.getElementById('contenedor-notificaciones');
```

---

## 2. Gestion del Estado
Crea un arreglo de objetos para almacenar tus tareas. Esto permitira que tu aplicacion sea reactiva y facil de filtrar.

```javascript
let tareas = []; // Array de objetos { id, titulo, prioridad, categoria, fecha, completada: false }
```

---

## 3. Funcionalidades Principales (Paso a Paso)

### Paso A: Agregar una Tarea
1. Escucha el evento `submit` del `formulario-tarea`.
2. Evita el comportamiento por defecto (`e.preventDefault()`).
3. Crea un nuevo objeto con los valores de los inputs.
4. Genera un ID unico (puedes usar `Date.now()`).
5. Agrega el objeto al array `tareas`.
6. Llama a una funcion `renderizarTareas()` para actualizar la UI.
7. Limpia el formulario y muestra una notificacion.

### Paso B: Renderizar la Lista
1. Limpia el contenido actual de `lista-tareas`.
2. Itera sobre el array `tareas`.
3. Crea los elementos HTML necesarios usando `createElement` o `innerHTML`.
4. **Importante:** Aplica la clase `item-tarea--completada` si la tarea tiene el estado `completada: true`.
5. Si el array esta vacio, aplica la clase `estado-vacio--visible` al elemento `estado-vacio`.

### Paso C: Marcar como Completada
1. Delega el evento `click` en la `lista-tareas`.
2. Si el click fue en la casilla (`item-tarea__casilla`), busca el ID de la tarea.
3. Cambia el valor de `completada` (true/false) en tu array de datos.
4. Vuelve a renderizar o usa `classList.toggle` para actualizar visualmente sin recargar todo.

### Paso D: Eliminar Tarea
1. Escucha el click en el boton de eliminar.
2. Aplica la clase de CSS `animacion-salida-deslizar` al item de la tarea.
3. Espera a que termine la animacion (`setTimeout`) y elimina el objeto del array.
4. Actualiza las estadisticas.

---

## 4. Actualizacion de Estadisticas
Crea una funcion `actualizarEstadisticas()` que se ejecute cada vez que el array de tareas cambie:
- Cuenta el total de elementos.
- Filtra para contar cuantas tienen `completada: true`.
- Calcula el porcentaje para el ancho de la `barra-estadisticas__relleno-progreso`.

---

## 5. Funciones Pro (Mejoras Futuras)
- **LocalStorage:** Guarda el array de tareas en el navegador para que no se borren al recargar.
- **Filtros:** Escucha el menu de filtros para mostrar solo "Pendientes" o "Completadas" manipulando que elementos del array se renderizan.
- **Buscador:** Usa `.filter()` y `.includes()` sobre el titulo de las tareas mientras el usuario escribe en la barra de busqueda.
- **Modo Edicion:** Al dar click en editar, abre el modal y llena los campos con los datos de la tarea seleccionada.

---

### Recordatorio de Clases CSS Utilitarias
Ya tienes estas clases listas en tu CSS para usarlas con `element.classList.add()`:
- `item-tarea--completada`: Aplica el tachado y opacidad.
- `superposicion-modal--visible`: Muestra el modal de edicion.
- `contenedor-notificacion--visible`: Muestra el toast de exito.
- `animacion-entrada-deslizar`: Para que las nuevas tareas aparezcan con estilo.
- `animacion-sacudida`: Ideal para cuando el usuario intenta agregar una tarea vacia.

---

## 6. Estructura HTML de una Tarea (Referencia)
Usa este bloque como plantilla cuando crees elementos dinamicamente en tu funcion `renderizarTareas()`.

```html
<li class="item-tarea" id="tarea-ID" data-tarea-id="ID">
  <div class="item-tarea__izquierda">
    <!-- Agregar clase 'item-tarea__casilla--marcada' si esta completada -->
    <button class="item-tarea__casilla" id="casilla-ID" type="button">
      <span class="item-tarea__icono-marcado">&#10003;</span>
    </button>
    <div class="item-tarea__contenido">
      <p class="item-tarea__titulo">TITULO_DE_LA_TAREA</p>
      <div class="item-tarea__meta">
        <!-- Clases de etiqueta: item-tarea__etiqueta--personal, --trabajo, --estudio -->
        <span class="item-tarea__etiqueta item-tarea__etiqueta--CATEGORIA">CATEGORIA</span>
        <!-- Clases de prioridad: item-tarea__prioridad--alta, --media, --baja -->
        <span class="item-tarea__prioridad item-tarea__prioridad--PRIORIDAD">PRIORIDAD</span>
        <span class="item-tarea__fecha">FECHA</span>
      </div>
    </div>
  </div>
  <div class="item-tarea__right">
    <button class="item-tarea__boton-editar" id="boton-editar-ID" type="button">
      <span>&#9998;</span>
    </button>
    <button class="item-tarea__boton-eliminar" id="boton-eliminar-ID" type="button">
      <span>&#10005;</span>
    </button>
  </div>
</li>
```
