![[Pasted image 20241113174519.png]]

Los SPA se fundamentan en la *programación por componentes*, solo hay una pagina HTML. En es pagina HTML se crea un *arbol de componentes* (siempre uno principal) y se va bifurcando y se van sustituyendo en función de la url que ponga el usuario en el navegador.



- directorios:
	- node modules<---paquetes instalados (externos etc)
	se comprime todo menos node_modules para entregar examen, se restauran en otro ordenador instalándolos a partir de package.json -|
		para restaurar: npm install (se genera node_modules , desde el dir. raiz del proyecto).
	
	- public. con el index.html, se meten las imagenes, *contenido estático*, css de aplicacion global, ficheros de js puros.
	
	- src: **EL MAS IMPORTANTE**. directorio del codigo fuente del react. Cada empresa dice cómo dividir el código... 
		vamos a crear:
			- directorio componentes: ficheros .jsx que definen componentes de la applicacion
			- direcorio servicios: ficheros .js que definen servicios de comunicacion del front de react con el back de nodejs
			- directorio hooks_personalizados: funciones (redondeo de precio por ej.)
			
			![[Pasted image 20240911185653.png]]
			Hay que dejar bien organizados componentes,etc.

- - - 
componentes REACT
- - - 
Unidad basica dentro de un proyecto de react (como en cualquier spa); un componente se mete en un fichero con extension .js (Javascript + HTML)
	componente = *funcion de javascript que devuelve codigo jsx Ningun componente puede no devolvelro.
	está formado cada fichero .jsx por:
		- PROS<- conjunto de variables que recibe el comp. desde el comp. padre en el arbol.
		- STATE<- variables internas al componente, son mutables. Cambian por eventos, acciones del usuario             sobre el componente. Cada vez que una variable state cambia, el componente se refresca y todos los que tenga arriba refrescan/renderizan también... *cuidado*
		- codigo-funcional<- codigo js encapsulado dentro del componente, solo accesible por el para def. variables, funciones...
		- codigo-JS<-  OBLIGATORIO!! es la vista (UI) del componente. Mezcla de codigo HTML, CSS y JS (un poco especial, con determinadas reglas).
--------------------------------------------
Ejemplo: en carpeta src: componente raíz App.js

import logo from './logo.svg';
import './App.css';

function App() {            -- TODOS LOS ELEMENTOS HTML TIENEN QUE IR CERRADOS
  return (
    `<div className="App">`    
      `<header className="App-header">` -- clase importada del css 
        `<img src={logo} className="App-logo" alt="logo" /> -- definido arriba`
        `<p>`
          `Edit <code>src/App.js</code> and save to reload.`
        `</p>`
        `<a`
          `className="App-link"`
          `href="https://reactjs.org"`
          `target="_blank"`
          `rel="noopener noreferrer"`
        `>`
          `Learn React`
        `</a>`
      `</header>`
    `</div>`
  );
}

donde se incrusta? vamos a index.html e index.js

![[Pasted image 20240911193149.png]]
el js esta cogiendo el id, el div del index.html, una vez lo selecciona, mete dentro el componente App (incrusta su codigo en el div).

![[Pasted image 20240911193207.png]]

 13/09/2024

el componente se incrusta o renderiza dentro del div id="root" que tiene la única página, el index.html gracias al script que se ejecuta nada más cargar la página: index.js.

const root = ReactDOM.createRoot(document.getElementById('root'));
			|
	define contenedor para incrustar comp. raiz del arbol de componentes, seleccionando	
	el elemento del DOM para hacerlo, el div con id="root"

	root.render(------- ->  en ese contenedor, alacenado en variable "root" de JavaScript renderiza el componente <App/> definido arriba
	<React.StrictMode>
	<App />
	</React.StrictMode>
	);
reglas de codificacion de JSX (mezcla de HTML + JS) 
- - - - - - - - - - - - - - - - - - - - - - - - - - - - 
- todos los tags deben tener su correspondiente etiqueta de cierre ( no puede quedar ningún tag abierto)
- cuando quieres incluir código JavaScript (mostrar contenido de una variable, llamar a una funcion manejadora de un evento, ...) va entre llaves:
		return (
		`<div className={variable_javascript} style={variable_javascript} |{propestilo: valor,propestilo:valor, ...} }`
		`{`
			`....codigo javascript}`
		`)`
IMPORTANTE :  solo se puede devolver un html unico, no puedes devolver varios elementos a la vez, si necesitas devolver varios los englobas en un elemento raiz, que puede ser un `<div>` o un elemento plantilla que ofrece react <>...</>
	

*falta*

dentro del codigo javascript de JSX no puedes usar bloques if... else ... ni for.... si quieres poner condicionales tienes que usar el operador ternario:
					`return (` 
						`<>`
							`<div...> .. </div>`
							`{`
								`if (tipocuenta=='personal') { <== casca`
								`} else {`
								`}`
							`}`
							`<footer...> </footer>`
					`)`
	sería : 
		`{ tipocuenta=='personal' ? (,,,,`
						        `,,,) : (...`
									`......)`
		`}`
si qieres poner condicion simple if....  se usa el operador && (sin else)
	`return (`
		`<>`
		`{`
			`tipocuenta== 'personal' && ( ... )` 
		`}`
		`</>`
	`)`
si qieres usar bucles for..... tienes que usar el metodo .map() de la "clase" array de javacript
ejemplo en la consola 
	(element, index array) 
	
		`let miarray= [`
			`{nombre: 'pablo',edad:15},`
		`{nombre: 'mariluz',edad:56},`
		`{nombre: 'pepe',edad:28}`
		`]`
map no toca el original, crea uno nuevo.
![[Pasted image 20240913201829.png]]
![[Pasted image 20240913201816.png]]

ver apuntes de Pablo, para desestructurar objetos (dividirlos en variables independientes, en funcion a la posicion).

	let cliente = { nombre:'pepe',apellidos:'gonzalez',edad:32,
	casado:false,nif:'1231321312A'}
quiero obtener de este objeto solo el nif y la edad en variables independientes.

	let _nif=cliente.nif   <--- esto es un coñazo
	let _edad = cliente.edad
hacemos **desestructuración**
	let {edad,nif,... resto} = cliente

coje todo el objeto, como nombre.... esta después, machaca a lo de arriba
![[Pasted image 20240913202546.png]]
solo se pueden desestructurar objetos y arrays.
let { nif } = cliente  *tengo que mirar que hace esto*


.... "si quieres utilizar bucles for"... 
	`return (`
		`<>`
			`<div...>..</div>`
			`{`
				`arrayproductos.map((el,pos,arr)=> { .... })`
			`}`
		`</>`
	`)`
*ver ejemplo de ternario 20:33*
-   -    -      -  -    -      -  -    -      -  -    -      -
Registro.js....
20:38 ver import, export.

	NUNCA USAR LAMBDAS PARA DEFINIR COMPONENTES, UTILIZAMOS FUNCTION

  // PRIMERA SECCION CON PROPIEDADES DEL COMPONENTE
  // SEGUNDA SECCION CON STATE COMPONENTE
  // TERCERA SECCION CODIGO FUNCINAL JAVSCRIPT (HOOKS,TRATAMIENTO,EVENTOS,...)
  // CUARTA SECCION CODIGO JSX

function Registro() {
	return (
	);
}

	![[Pasted image 20240913204506.png]]
Se ejecuta dos veces en desarrollo, al desplegarse no. 
![[Pasted image 20240913204907.png]]

*Propiedades de un componente* 
 `- - - - - - - - -`
cuando quieres pasar valores a un componente en forma de atributos, se usa el objeto props:
`<NombreComponente atributo=valor atributo2=valor ..... />`
	Estos atributos REACT los mapea contra un objeto llamado props, que recoje
	como parametros de la funcion que define el componente
		function NombreComponente ( props ) {
		...
		en props hay un objeto js con este formato: { atrib1: valor, atrib2:valor, ... }
		}
todos los componentes contienen un props.
En Registro.js
![[Pasted image 20240913205333.png]]


![[Pasted image 20240913205700.png]]
directamente en la funcion
![[Pasted image 20240913205928.png]]

Bootstrap 
	unidad basica : contenedor
	cdn en el index.html
	
## 17/09/2024

en react onSubmit={ una funcion manejadora de evento} declarada con una lambda y si tiene mucho codigo, definimos una funcion para ello

todas las funciones de evento definen un objeto tipo event.
el submit form -> method='GET' por URL y por method='POST' diferencia?
no aparece en la url nada con post. 

**AUNQUE HAGAMOS VALIDACIONES CORRECTA EN NAVEGADOR CLIENTE, TENEMOS QUE HACER SIEMPRE EN EL BACK VALIDACIONES, DA IGUAL LO QUE HAYA EN EL FRONT **

SIEMPRE QUE NECESITEMOS UNA VARIABLE, QUE SE CAMBIE POR ACCION DE UN USUARIO, NECESITAS METERLA DENTRO DEL STATE. 

definir *hook* en js: es una funcion JavScript del paquete de React que admite determinados parámetros en función del hook y que otorga funcionalidades a todos los componentes de la aplicación.

el hook useState admite como único parámetro:
	- un valor constante con el que inicializar la variable del state que estás creando
	- una función sin parámetros y que tiene que devolver un valor constante con el que se inicializa la variable del state.
este hook, devuelve un array de dos posiciones:
	- en la posicion 0 se devuelve el valor que tiene en ese momento la variable del state
	- en la posición 1: se devuelve una función SETTER para cambiar el valor de la variable del state (ante un evento).

let [ nombre_variable_state, setNombre_Variable ] = useState (valor_inicial| ()=>{...; return valor_inicial;}) se puede desestructurar este array de dos posiciones. 

VALIDACIONES 16:48

para expresiones en consola: new RegExp("") y metodo test para testear esa cadena.  o entre barras directamente = `/^[A-Z].*/`
REGEXR.COM


## 18/09/2024 

utiliza objetos para la validacion y sacar el mensaje. Como cambia la variable de ese objeto segun se producta el evento? Ya no es un string o booleano, ahora es con propiedades del objeto.
![[Pasted image 20240918192559.png]]

19:26 IMPORTANTE
// TODO -  OPERADOR ' . . . ' y el useState( ).

## 20/09/2024
![[Pasted image 20240920201623.png]]
what

20:33 formas de acceder a un objete. 

## 24/09/2024

como mandar datos desde js al backend (servidor)
	AJAX 
		http-request GET,POST              servidor backend nodejs
cliente ----------------------------------->  servicio REST (RESPUESTA SIEMPRE DATOS EN JSON)
       <----------------------------------    en http-response
	         http-response
lac cabecera http-request, content-type establece el tipo de datos que va en el body del http-request:
	1.application/x-www-form-urlencoded (por defecto el que usan los form) ===> variable= valor,variable=valor, . . . 
	2.application/json ===========================> json: {prop:valor,prop:valor, ...}
si con la cabecera numero 1, mandas json, no sabe cómo parsearlo
- - - - - - - -
![[Pasted image 20240924155517.png]]

Usando ajax, formas de mandar datos desde js al backend

1ª forma: usando el objeto XMLHttpRequest (xhr) - primitiva, usa todo navegador.
	- a) nos creamos un objeto XMLHttpRequest: 
				`let _petAjax= new XMLHttpRequest();`
	  b) usando el metodo .open() del objeto, abrimos conexión (socket) contra el servidor usando la url y metodo http que definas.:
`_petAjax.open( ' GET ' | ' POST ' , 'http://localhost:3000/api/zonaCliente/Registro' )

usando el  metodo send(...)  mando los datos al servidor, en el argumento van los datos.
	GET --->  en el body del http-request no van datos, van en la url, entonces :
			`_petAjax.send(null)`
	POST--> en el body del http-request si van los datos en funcion de la cabecera que pongas en el content-type, ej usando método .setRequestHeader('cabecera',valor)
			     `ej    _petAjax.setRequestHeader('Content-Type','text/plain');`
					`_petAjax.send('hola como estas')`
			   ej: mando json
					   `_petAjax.setRequestHeader('Content-Type','application/json');`
					   `_petAjax.send( { nombre:'...', apellidos: ' ....', edad:35 } )`
						|_ a veces antiguamente pasaba solo `[ Object object ]` 
						si da ese problema, hay que serializar el json
						JSON.stringify( { ... } ), y luego lo deserializas en server con JSON.parse (  ... )
	el codigo de js es single thread, si haces llamada a servidor, en espera te quedas. Para evitar esa limitación se crearon las promesas, eventos, funciones callbacks. 
	
c) se recomienda hacerlo antes del .send() al definir la funcion callback de llamada cuando se completa la pet.ajax al servidor. ¿Como sabe el single-thread de js que la pet.ajax ha finalizado? el objeto xmlhttprequest tiene un evento, llamado 'readystatechange'. La propiedad .readyState controla los valores de este evento 

		0 UNSENT  Client has been creadted. open() not called yet
		1 OPENED open() has been called.
		2 HEADERS_RECEIVED send() has been called, and headers and status are avaliable
		3 LOADING Downloading; responseText holds partial data
		4 The operation is complete <==== el servidor ha completado el procesamiento de los datos y te ha mandado respuesta...
	
	entonces creamos un handler para el evento readyStateChange buscando prop sea 4:
			_petAjax.addEvetListener('readyStateChange',(ev){
				if(_petAjax.readyState==4) {
					//ya tengo respuesta del server , la leo...
				}
				} );
	para leer respuesta del server usamos propiedades .responseText y .responseXML del objeto XMLHttpRequest: .responseXML no se suele usar, en formato XML (SOAP viejunos)
	servicio SOAP no se suelen usar.
	.responseText para cualquier otro tipo de dato, respuesta del server, 

		_petAjax.addEvetListener('readyStateChange',(ev){
				if(_petAjax.readyState==4) {
					//ya tengo respuesta del server , la leo...
				   	let _respuestaServer=JSON.parse(_petAjax.responseText);
					}
				} );
	
2ª FETCH-api , más modernos, en navegadores antiguos no funciona. 
3ª Paquetes externos, como AXIOS


## 25/09/2024

BACKEND node JS

Para iniciar un proyecto de node, te situas en el directorio donde vas a codificar el proyecto
			npm init -y
				-- te genera un fichero package.json (npm help init para ver +)
		!!OJO!! con el nombre del punto o modulo de entrada, porque es el modulo principal
			de nuestro proyecto de nodejs (por defecto pone index.js)
	
node funciona con MODULOS de codigo JS (cada modulo es un fichero con extension .js)
cada modulo es una funcion inmediata de JS que no ves, pero se genera: 
		server.js 
+--------------------+
		(function(module,require,exports, `_dirname,_filename`){
			//codigo js de tu modulo... de forma interna puedes usar
			- exports:  puntero al objeto module.exports. Valores a exportar del modulo.
			- require: funcion para importar de otros modulos. 
				se usa así: 
					const variable = require('ruta_modulo' | 'nombre_modulo_js')
			- module: objeto de la clase global Module, de NodeJS (con sus props y metodos)
			- `_filename` : string con ruta y nombre del modulo 
			- `_dirname`: string con ruta del modulo
		      }
		)(global)
funciones inmediatas, siempre entre parentesis, y entre parentesis los parámetros.

![[Pasted image 20240925180139.png]]

![[Pasted image 20240925180553.png]]


como montar un servidor web en nodejs sin utilizar parquete HTTP INTERNO: EXPRESS
-========================================================
- instalamos express: npm install express --save
![[Pasted image 20240925183928.png]]
request, respond, next IMPORTANTE lo vamos a ver mucho


funcionamiento de express:
		http-request       servidor express+nodejs         next (si no manda respusta la1)next
cliente ------------------>  [ funcion middleware-1 ]       <==> [ funcion middleware-2 ] <==> ....
						|procesa http-request        procesa http-request mod por midle1
						usando parametro:req        usando parametro req 
						(objeto HttpRequest de                        ||
						express)                                       puede mandar respuesta 
							||                                             usando parametro res
        <---------------| puede mandar respuesta usando 
		http-response     parametro res
				     (objeto HttpResponse de express)

el order es importante, siempre a la que va detras

en powershell:
	Invoke-RestMetod -Method post -url 'http://localhost:3000/api/zonaCliente/Registro' -Body 'hola mundo'
			  
## 01/10/2024
cookie-parser,body-parser,cors

el *estado de sesion importante*: almacenar los ultimos valores que el cliente ha hecho dentro del servidor.

thunder client.   

Todos los ordenadores tienen una politica de seguridad, como cliente solo puedes mandar datos al mismo puerto y url que estas manejando, no puedes salir hacia fuera. Sin embargo, cuando estamos invocando a node, es en otro puerto, el navegador corta. 

OPTIONS -> HABILITAR CORS PARA QUE PUEDA HACER POST
CORS->DEFINIR BIEN , SE HABILITA EN EL SERVIDOR. OPTION->REQUEST
Puedes restringir los endpoints a los que los clientes pueden acceder. 

lo que ha mandado-> sin promesas, eventos propios. ver e intentar entender el codigo de Javi (notepad)

tipo: restService cuando pinalice pet.ajax de RegistrarCliente
dispare un evento personalizado llamado respuestaRegistro
como datos del evento, van los datos de la pet.ajax (respuesta del servidor) 

El componente de react Registro.js de react tiene que estar a la escucha, añadir handler al evento "respuestaRegistro" del servicio restService

## 02/10/2024
----------------------
como modificar codigo asincrono js:
-  eventos y funciones callback
-  objetos PROMISE
-  async/wait
-------------------
lo que hay que tener en cuenta es que JS es single thread, y solo puede ejecutar código síncrono.
Cuando ejecuta instrucciones que le llevan tiempo, las lanza y salta a la siguiente instrucción, ¿cómo vuelve o recupera el resultado de esa instrucción u operación pendiente?

Hay tres formas: 
	- eventos y funciones callback: cuando defines la op.asincrona, siempre se le pasa una funcion callback.  Que es la que se va a ejecutar cando la op. asincrona finaliza y dispara un evento de finalización; esta función callback será el handler o manejador del evento de finalización de la op.asíncrona.
	Para crear o hacer que un objeto js sea susceptible de generar eventos, te creas una propiedad en el      
	objeto de tipo EventTarget.
ej: tengo un objeto propio, y quiero que sea capaz de disparar eventos y a su vez poder esuchar y ejecutar func. callback ante ellos (handlers)

```javascript
let miObjeto= {
	generaEventos: new EventTarget(), //<- con esta propiedad el objeto js 'miObjeto ya puead disparar eentos con metodo dispatchEvent() y añadir handlers o funciones callback a estos eventos con método .addEventListener()
// Para disparar eventos personalizados con .dispatch tienesque crearte objetos
// de tipo CustomEvent('nombre_evento')
}
```
![[Pasted image 20241002174815.png]] al ser personalizado, CustomEvent.
dispatch dispara a mano.

callback-1       callback-1
callback-2       callback-2
. . .                      . . . 
evento: . . .       evento:...
-----------+
+--------------+
|  miObjeto  |  ==> dispara evento...
+--------------+
	^
	| _ _ acción: RegistrarCliente.   Generada por componente Registro.js

Importante: this en funcion lambda, funcion normal js. No valen lo mismo. La normal hace referencia que invoca ese metodo. En lambdas, .this hace referencia al objeto global, superior. restService.

Por que en restservice el de addCallbackEvent  function, el .this es diferente? llama al objeto? 

-  **promesas**: son objetos Promise. Representa la 'promesa' de un valor que se va a entregar en un futuro inmediato. Cuando la tarea asíncrona finalice su ejecución.  
	Cada vez que quieras realizar una tarea asíncrona te creas un objeto Promise. 
	El constructor admite como único parámetro: -> una función -> que tiene 2 parámetros:
				- resolve : 1º parámetro, sirve para establecer el valor que devuelve la promesa cuando la op asíncrona acaba.
				- reject: 2º parámetro, estableces el tipo de error producido en la ejecucionde la op.asincrona (fallo durante su transcurso)
				-
	Para interceptar esos valores que genera la promesa CUANDO ACABA (tanto si acaba bien y ejecuta el 'resolve' como si acaba mal, y ejecuta el 'reject') el objeto Promise:

```javascript
.then ( ( datos_enviados_por_resolve) => { . . . . }   ) 
	
```
^metodo que se ejecuta cuando la promesa acaba bien y recibe los datos enviados por el resolve de la mism acomo parametros de una funcion para tratarlos.


``` javascript
.catch ( (error_enviado_por_reject) => { . . .}
```

^- metodo que se ejecuta cuando la promesa acaba mal, y recibe los datos enviados por reject de la misma como parametros de una funcion para tratar el error.

1er paso : creo objeto promise para op asíncrona:
```javascript
let _petAjaz= new Promise(
	(resolve,reject) => {
		...
		resolve(datosserver);
		...
		reject(errorServer);
		}
	);
```

2º paso: usas metodos .then()  y .catch()  sobre el objeto Promise:
```javascript
_petAjax.then( (datosServer)=> {...} ).catch ( (errorServer) => {...} )
```


## 15/10/2024
- - -
TOKENS DE SESION JWT (JSON -WEB-TOKEN)
usados en servicios REST-API (stateless)
- - -
servicios stateless no almacenan como un servidor normal web, el estado de sesión. 
- - - 
en aps web tradicionales: cliente <---> servidor
												  |								estado de sesión: para cada cliente que se conecta
												|							en RAM  tiene una tabla colección: id_sesion_cliente | valors_sesion
										se almacena en cliente cookie: id_sesion_cliente

En servicios REST los clientes no se conectan continuamente contra el servicio, sino de forma esporádica, no tiene sentido almacenar estado de sesión, para reemplazarlo surgieron los JWT, donde ahora es el CLIENTE el que almacena su token de sesión. Y cada vez que quiera usarlo se lo tendrá que mandar 
			       AJAX (JWT)
cliente  < - - - - - - - - - - -> servicio REST (servidor)
| - almacenar el jwt
|- mandarlo al servicio
para endpoints de acceso restringido
se manda en cabecera:
	Authorization: Bearer . . . jwt . . . .
cuando el servicio recibe un jwt dwel cliente, debe comprobar:
	- si ha sido generado por el (usando clave de firma)
	-  si ha expirado o no (aun en vigencia)

Los JWT tienen un tiempo de vigencia determinado

Hay dos tipos de JWT:
-  accessTokens  < - - - -  tiempo de expiracion muy corto, máximo 15 minutos
				        OJO!! como se almacena en el navegador del cliente el tiempo debe ser minimo
				        por seguridad (alguien acceda al navegador y use token suplantando identidad)
				        cuando caduca, el usuario debería loggearse de nuevo o usar refreshtoken
-  refreshTokens < - - - -  jwt de tiempo de exp. de varias horas. En este JWT no vna tantos datos como en 
		                              el accessToken, solo el mínimo para regenerar un accessToken caducado.
		                             (evitas que el usuario se vuelva a loggear, recomendable avisar al usuario si quiere hacer uso del mismo o no).
- - - 
FORMATO DE UN JWT: `https://jwt.io`
- - -
Esta formado por 3 campos: HEADER .  PAYLOAD . SIGNATURE
	 - HEADER: un json con info como el tipo de token (jwt), algoritmo para sacar el hash del jwt
	 - PAYLOAD o datos: un json formado por campos o propiedades llamados CLAIMS
											 dos tipos de claims:
											 - públicos predefinidos: props predefinidas como:
													 - issuer: nombre del servidor web que genera el token
													 - exp: tiempo en ms de vigencia del token (expiración)
													 .... muchos más, en la web aparecen
											- privados: o definidos por el server
	- SIGNATURE o firma:  usado para comprobar si el jwt ha sido generado o no por un servicio
- - - 
			MONGODB
- - - 
[Transacciones](https://www.mongodb.com/docs/manual/core/transactions/#transactions-and-atomicity), hasta que no se hagan todos los insert, no se hace ninguno.


# 18/10/2024

DOM Virtual react, ni se entera si cambias el dom directamente.

ref = {   }  puntero para referenciar al valor.    valor variable = .current

setInterval ---- --  setTimeOut


# 27/11 /2024
como el useState, cada cambio en cada campo.

validadores sincronos y asincronos. validan el input cada cambio.

angular language service
angular snippets 18,angular schematics

# 10/12/2024
npm para que use typescript 
		`npm install @types/node@latest --save-dev` 
cada vez que instalemos un paquete, a partir de ahora debemos instalar los tipos correspondientes:
ej. si instalamos express, debemos instalar tb sus tipos typescript:
	- `npm install express --save`
	- `npm install @types/express@latest --save-dev`
	los tipos de datos siempre en desarrollo (dev)

//modulo typescript q exporta una unica funcion q recibe como parametro el servidor web express
//a configurar su pipeline (funciones middleware)

import express,{Express} from 'express';
import cors from 'cors';

export default function config_pipeline(serverWeb:Express){
    serverWeb.use(express.json());
    serverWeb.use(express.urlencoded({extended: true}));
    serverWeb.use(cors());

    //midleware de enrutamiento...
	}
------------------
server.ts:

import 'dotenv/config'; //<---- lee el fichero .env y crea variables de entorno accesibles con process.env.
import express,{ Request, Response , Express} from 'express';
import { Server } from 'node:http';
import config_pipeline from './config_server/config_pipeline';

const app:Express=express();

//configuramos la pipeline del servidor express con sus funciones middleware
config_pipeline(app);

const webServer:Server=app.listen(
                                3003,
                                ()=>{ 
                                    console.log('....servidor WEB EXPRESS corriendo en puerto 3003...')
                                    }
                                )
============================
###################################
    typescript con nodejs
###################################
- instalamos typescript:
        npm install typescript --save-dev 

una vez instalado, te instala un comando "tsc" (typescript compiler)
incializamos proyecto typescript:

            cd ./ebay-nodejs
            npx tsc --init <------- inicializa proyecto typescript, crea un fichero
                                    tsconfig.json con las opciones de configuracion para
                                    compilar codigo typescript a js

                                    npx es un alias:  npm execute

- instalamos tipos de datos de nodejs para typescript:

        npm install @types/node@latest --save-dev

- a partir de ahora, cada vez que instale un paquete debo instalar los correspondientes tipos:

    ej. si instalo express debo instalar tb sus tipos typescript:
                
                npm install express --save
                npm install @types/express@latest --save-dev

**11/12/2024** 

el metodo get 


**un observable por el que pasa un unico dato es una promesa.**
mirar 18:11, metodos async, promesas... devolver Observable, promesa de...

**15/01/2025** 
inyección de dependencias constructor o parámetros?

cuando clase abstracta, cuando interfaz?
19:24 Interfaz no tiene métodos estáticos.
más facil clases abstractas, 
no puedes inyectar directamente interfaces, necesitas el token, **SÍ** puedes inyectar clases abstractas. 


**17/01/2025** 
Signals, react quiere quitar hooks como use state, effect, y pasar a señales. 
Angular quiere quitar los observables y pasar a señales.

