# CLIENTE

## REACT
### EXAM

email:    hola@gmail.com
password: Hola1234
### USEEFFECT() 
- \[ \] -> INITIAL RENDER
- NO -> EVERY RE RENDER
- \[dep1,dep2,\] -> WHEN THOSE DEPENDENCIES CHANGE

CLEANUP FUNCTION? -> BEFORE RUNNING AGAIN + WHEN DESTROYED
### REACT-ROUTER-DOM
npm install react-router-dom localforage match-sorter sort-by

```javascript
import { createBrowserRouter, Outlet, RouterProvider } from "react-router-dom";

import {useLoaderData, useParams} from 'react-router-dom'; 

const navigate = useNavigate();
```

### **USELOADERDATA()**
- What the loader returns (anything)
`const products = useLoaderData();`
### **USEPARAMS()**
- Gets **URL parameters** from the current route
`const nameCategory = useParams().nameCat;`

## NODE

### EXPRESS SETUP
npm init 
npm install express --save
### MONGODB
** IF AM WORKING WITH NESTED OBJECTS I HAVE TO ALWAYS USE `' '`** 
> Nested Object :
{ $set: { 'account.accountActivated' : true } }
Top level not needed the '' 

updating with query on \_id: `{ _id: new mongoose.Types.ObjectId(idCliente), email },`
sending the id? -> \_id.toString()

existe? -> "paymentMethod.card": { $exists: true }"
array? -> paymentMethod: { $elemMatch: { card: { $exists: true } } }

nested, only property?: 
```mongodb
db.particulares.aggregate([
  { $match: { "paymentMethod.type": "VISA" } },
  { $project: { card: "$paymentMethod.card", _id: 0 } }
])`
```
- **$push** - ON ARRAYS ONLY
- **$set** - OBJECTS, ** DON'T WANNA OVERWRITE?? :
```js
$set: { // instead of $set {paymentMethod: { ...
      "paymentMethod.type": "newType",
      "paymentMethod.details":     }
```

update: 
- $set  would overwrite an array
### JWT

#### SIGN
```js
const JWT_ACTIVATION = jwt.sign(

{

email: payload.email,

idCliente: resInsert.insertedId.toString(),

},

process.env.JWT_SERVER_SIGNATURE,

{ expiresIn: "10min" }

);
```

#### VERIFY

```js
const jwt_payload = jwt.verify(token, process.env(JWT_SIGNING_KEY));
```

### BCRYPT

#### HASH DATA
```js
const hash = await bcrypt.hash(password, 10);
``` 
#### COMPARE
```js
const passwordsMatch = await bcrypt.compare(password, userData.password);
```


### URLSEARCHPARAMS

```javascript
const body = {
    name: `${nombre} ${apellidos}`,
    email,
    'address[line1]': direccionEnvio.calle,
    'address[city]': direccionEnvio.municipio,
    'address[state]': direccionEnvio.provincia,
    'address[country]': direccionEnvio.pais,
    'address[postal_code]': direccionEnvio.cp
}

const params = new URLSearchParams();
Object.entries(body).forEach(([key, value]) => {
    params.append(key, value);
});

const petCreateCustomer = await fetch(`${BASE_URL_STRIPE}/customers`, {
    method: 'POST',
    headers: {
        'Authorization': `Bearer ${process.env.STRIPE_SECRET_KEY}`,
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: params
});
```



## ANGULAR

//TODO ver componentes standalone diffs versiones

@ decorators A nivel de clase //TODO 

### FORMS

Two types of forms: 
- Reactive forms // recommended + complicated 
FormGroup-> constructor
controls: object of  form controls

We map FormGroup with the <form...> 
	*how?* -> FormGroupDirective 

and inside the FormGroup constructor:
	-> mapping 
	name_input : new FormControl()
isn't needed to put attribute name, if we use FormControlName (directive) 
- template-forms // simple forms


FormControl, *constructor* : 
- if you want to initialize the input with a value.
- second param, array of synchronous  Validators, we can program our own Validators. Fired in sequence
- asynchronous Validators (Custom) 
**ORDERN MATTERS FOR SYNC AND ASYNC VALIDATORS** 

Validation errors  -- *saved* - - > in property `errors` FormControl
some FormControl invalid ? form invalid

in component : imports : \[ ReactiveFormsModule \] 

//TODO map form , prepare endpoint node
button disabled until valid form.

- - - 

SERVICIOS, peticiones al exterior, al intercomunicar componentes que no tienen conexión entre sí.

@Injectable -> crea una instancia de la clase y la introduce en el DI
provideIn: 'root', para toda la app

Si no en root, en cada componente tienes que poner providers: [ ] 


NO HAY FETCH -> **HttpClient** . *HAY QUE HABILITARLO* !!!
> En app.config.ts -> EN PROVIDERS 
> provideHttpClient()

constructor (private http: HttpClient ) {}

a partir de la 19 (lo de arriba es antiguo) :

private http = inject(HttpClient);

Observable (post returns Observable) 

OnInit & OnDestroy - interfaces 

### ROUTING

`<router-outlet/>`

Lazy Loading 

Resolvers = React loaders.

2 servicios: Router y ActivatedRoute (leer param url, y recuperar datos de los resolvers)

antes eran clases, ahora funciones los RESOLVERS.

Como recuperar los datos en el comp: 

paramMap de ActivatedRoute... IGUAL QUE EN REACT

Guards: guarda, bloquea acceso a una ruta si se cumple codig de una función.
	true: permite, false : bloquea.
puedes redireccionar a otro aldo con Urltree, desde el guard.
canActivate (rutas padres) , canActivateChild (hijas)...

Url Tree en el guard para redirigir.

### INTERCEPTORS

Servicio http client, con interceptors, puedes configurarle que son funciones que interceptan la petición captan el objeto request, pueden modificar cabeceras, el body y la mandan. Puede haber más de un icterceptor (chain of interceptors) , como si fueran middleware de express. 

No puedes trabajar sobre el inmutable request, hay que hacer clone() .

Hay que inyectarlo en el Http Client. 

pipe()  para manipular los datos -> el orden tiene importancia. 
//tap 
//map
//switc3hMap ?? 

puedes detectar si es request o response. HttpEventType
siguiente coge el modificado en función rsjx

Ej: almacenar en cache con un interceptor. 

