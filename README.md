# Apuntes de Node.js
Node.js es un entorno de ejecución de JavaScript. Esto significa que permite ejecutar JavaScript fuera del navegador, en cualquier dispositivo que tenga Node.js instalado.

Node.js se basa en el motor de ejecución V8, el mismo que utiliza el navegador Chrome para interpretar y ejecutar JavaScript.

Una de las características clave de Node.js es que está basado en eventos y utiliza un modelo de ejecución no bloqueante. Esto quiere decir que no se detiene cada vez que se inicia una tarea. Por ejemplo, si tiene que leer un archivo y luego mostrar su contenido, el sistema no se queda esperando a que termine la lectura. En su lugar, pone esa tarea en segundo plano y continúa ejecutando otras operaciones. Cuando la lectura del archivo finaliza, se dispara un evento, y en ese momento Node.js retoma esa tarea y muestra el contenido del archivo.

## Instalar Node
Para instalar Node.js utilizaremos el gestor FNM (Fast Node Manager). Este programa nos permitirá instalar y actualizar Node.js; además, podremos tener diferentes versiones de Node.js en el mismo sistema.

FNM está disponible solo para Linux y macOS, por lo que en Windows sería necesario usar la consola de Git Bash o WSL.

Descargar FNM:

```bash
curl -o- https://fnm.vercel.app/install | bash
```

Para aplicar los cambios, debes abrir una nueva terminal o ejecutar el comando que aparece al final del output del comando anterior. En mi caso, siendo Linux, era este:

```bash
source /home/usuario/.bashrc
```

Instalar Node:

```bash
# Instala la versión LTS más reciente (recomendada)
fnm install --lts

# Instala la versión más reciente (experimental y con posibles errores)
fnm install --latest

# Instala una versión concreta
fnm install 18.16.0
```

Ver todas las versiones instaladas (la que esta en azul es la que se esta usando):

```bash
fnm list
```

Elegir la versión a utilizar:

```bash
# Cambia a la 18.16.0
fnm use 18.16.0

# Cambia a la versión LTS más reciente
fnm use lts-latest

# Cambia a la versión más reciente
fnm use latest

# Cambia a la versión establecida por defecto
fnm use default
```

Establecer la versión que se usará por defecto (cada vez que abras la terminal):

```bash
# Establece por defecto la versión LTS más reciente (recomendado)
fnm alias lts-latest default

# Establece por defecto la versión más reciente (experimental, pero util si quieres probar los últimos cambios)
fnm alias latest default

# Establece por defecto la 18.16.0 (si trabajas con un proyecto antiguo que esta hecho con una versión concreta)
fnm alias 18.16.0
```

Ver la versión de Node.js que se está utilizando:

```bash
node -v
```

## Ejecutar un archivo js
Para ejecutar un script de JavaScript con Node.js, se debe usar el comando node seguido del nombre del archivo:

```bash
node test.js
```

Esto hará que Node.js interprete y ejecute el código contenido en `test.js`.

## Sistema de modulos
Node.js utiliza un patrón de diseño basado en módulos. Los módulos permiten organizar el código en partes reutilizables, evitando la duplicación de funciones y facilitando el mantenimiento de la aplicación.

Existen dos formas de trabajar con módulos en Node.js: CommonJS (CJS) y ES Modules (ESM). CommonJS es el formato tradicional, mientras que ES Modules es la forma moderna y recomendada según los estándares actuales de JavaScript.

Al escribir un script de módulos, se suele usar la extensión `.cjs` para CommonJS y `.mjs` para ES Modules. Esto no es obligatorio, ya que también se puede utilizar la extensión `.js`, pero usar `.cjs` o `.mjs` ayuda a dejar más clara la estructura y el tipo de módulo que se está empleando.

### CommonJS
Un script de módulos escrito con CommonJS tendrá una lista de funciones que representan los módulos. Al final del script, se debe utilizar la variable global `module.exports`, que es donde se definen las funciones (o valores) que se van a exportar como módulos.

```js
function sum(a, b){
    return a + b;
}

function saludar(name){
    return `Hola, ${name}`;
}

module.exports = {
    sum,
    saludar
};
```

En otro archivo, podemos importar los módulos utilizando `require` y la ruta del archivo donde están definidos:

```js
// Importando todos los modulos en una sola variable
const utils = require('./utils.cjs');

console.log(utils.sum(1,2));
console.log(utils.saludar("Usuario"));

// Importando solo los modulos necesarios
const { saludar } = require('./utils.cjs');

console.log(saludar("Usuario"));
```

### ES Modules
La sintaxis de ES Modules es más sencilla y moderna. Para exportar funciones, simplemente se añade la palabra clave `export` delante de cada una:

```js
export function sum(a, b){
    return a + b;
}

export function saludar(name){
    return `Hola, ${name}`;
}
```

Para utilizar estas funciones en otro archivo, se importan utilizando `import`:

```js
// Importando todos los modulos en una sola variable
import * as utils from './utils.mjs' ;

console.log(utils.sum(1,2));
console.log(utils.saludar("Usuario"));

// Importando solo los modulos necesarios
import { saludar } from './utils.mjs';

console.log(saludar("Usuario"));
```