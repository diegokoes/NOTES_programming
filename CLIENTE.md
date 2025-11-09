# CLIENTE

## REACT
### EXAM
- [ ] 1-
- [ ] 2-
- [ ] 3
- [ ] 4-
- [ ] 5-
- [ ] 6-

### useEffect() 
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

### **useLoaderData()**
- What the loader returns (anything)
`const products = useLoaderData();`
### **useParams()**
- Gets **URL parameters** from the current route
`const nameCategory = useParams().nameCat;`

## NODE

### Express SETUP
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

### bcrypt

#### hash data
```js
const hash = await bcrypt.hash(password, 10);
``` 
#### compare
```js
const passwordsMatch = await bcrypt.compare(password, userData.password);
```


### URLSearchParams

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