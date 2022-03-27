# Structure

Les pages (accueil, commander, contact) sont stockées dans le dossier `pages` . Chaque fichier contient le contenu de toute la page ; on y verra tous les composants appelés, etc... pour la page entière.

Pour modifier le contenu d'une page, il vaut donc mieux passer d'abord par le fichier correspondant à la page, puis descendre dans l'architecture jusqu'au composant recherché. *L'imbrication est le principe même de React !*

## Composants génériques

Certains composants ont été codés de manière génériques, c'est-à-dire qu'on va juste passer les informations voulues en `props` , et le composant va s'adapter selon les valeurs entrées.

Par exemple, le composant `InputButton` est appelé dans à peu près toutes les pages, mais il n'a été codé qu'une seule fois. Attention donc à ne pas coder de *nouveaux* éléments inutilement !

Ces composants sont stockés dans le dossier `components/generic` .

<br/>

# Navigation

Le fichier `App.js` va mener l'utlisateur vers le bon routeur en fonction de l'url choisie.

```js
// App.js
<Switch>
     // ...
     <Route  path="/admin"  component = {AdminRouter}/>
     <Route  path="/"  component = {UserRouter}/>
</Switch>
```

Par exemple si le chemin commence par `/` (admin exclu), on ira dans le routeur utilisateur. Si le chemin complet est `/commander` , le routeur va rediriger vers le composant `Order` .

```js
// routers/UserRouter.js
const  UserRouter  = () => {
return (
     <>
          // ...
          <main  className="main">
               <Switch>
                    <Route  exact  path="/commander"  component = {Order} />
                    // ...
               </Switch>
          </main>
     </>
);
}
```

## Routes protégées

La plupart des routes sont protégées, comme celles concernant la partie admin, pour que quelqu'un qui ne soit pas connecté en tant qu'admin ne puisse pas y accéder.

- Pour la partie admin, on les utilise de cette manière :
```js
<ProtectedAdminRoute  exact  path={'/admin/accueil'} component={AdminHome} />
```
Si l'utilisateur n'est pas connecté sur la partie administrateur, il sera redirigé vers la page de connexion administrateur.

- Pour la partie utilisateur :
```js
<ProtectedUserRoute  exact  path="/historique"  component = {History} />
```
Si l'utilisateur n'est pas connecté sur la partie utilisateur, il sera redirigé vers la page d'accueil.

- La vérification se fait via la base de données. Les informations du comptes sont stockées dans le localstorage, accessibles comme ceci :

![localstorage](https://github.com/Nahay/Assets/blob/master/Charlemagne/localstorage.png)

Le back va décrypter le token, qui contient les informations servant à l'authentification du compte. Si ces informations correspondent à celles d'un compte stocké dans la base de données, on peut accéder à la page.

- Si on ne veut pas protéger la route, on peut juste utiliser `Route` au lieu de `ProtectedUserRoute` / `ProtectedAdminRoute` .

<br/>

# Services

Les services servent à effectuer des requêtes vers l'api depuis l'application React (front). Ils contiennent des fonctions qui vont chacune effectuer une requête `post` , `get` , `patch` ou `delete` sur l'api dont on a renseigné l'url dans la variable `API_URL` .

- Pour utiliser une fonction d'un service dans un composant, on l'importe de cette manière :
```js
import { getDishes, getDishByName } from  '../services/dishesService';
```
- Ici la fonction retournera la liste des plats dont le nom correspond au paramètre `name`  préalablement entré lors de l'appel de la fonction dans un composant.

```js
const getDishByName = async (name) => {
     try {
          const { data } =  await axios.get(API_URL  +  "/dishes/name/"  +name);
          return data;
     } catch(err) {
          toast.error(err.message);
     }
};
```

## Requêtes privées

La plupart des requêtes sont privées, car autrement n'importe qui pourrait par exemple modifier ou supprimer un plat, voire récupérer les données d'un compte administrateur.

On va donc vérifier via le token si on est bien connecté, afin de pouvoir effectuer la requête (un peu comme pour les routes privées, voir page Navigation).

Ci-dessous on voit donc que le token est passé en paramètre. Ce token est utilisé dans la fonction `adminConfig` appelée dans la requête `get` . Cette fonction va simplement ajouter le token dans le `header` de la requête. Le back va ensuite s'en servir pour vérifier que les données du token sont valides.

```js
const getAdminById = async (id, token) => {
     try {
          const { data } =  await axios.get(API_URL  +  "/admins/"  +id, adminConfig(token));
          return data;
     } catch(err) {
          toast.error(err.message);
     }
}
```

*Contrairement aux routes privées, la vérification est ici faite dans le back au lieu du front, celui-ci ne servant qu'à passer le token dans la requête.*

<br/>

# Style

Les différents fichiers `scss` se situent dans le dossier `style` . Le fichier `index.scss` permet de centraliser tous les fichiers afin de ne pas les appeler un par un.

Le style est donc importé dans le fichier `index.js` de cette façon :
```js
import  "./styles/index.scss";
```

Pour ajouter un nouveau fichier `scss` , il faut absolument l'ajouter dans le fichier `index.scss` . Autrement, il ne sera pas pris en compte.

```scss
// settings en premier car il initialise le style du site
@import  "./settings";
// generics en deuxième car il initialise le style des composants génériques
@import  "./components/generics";

// --- ensuite tous les autres fichiers, peu importe l'ordre
@import  "./components/nomDuFichierAImporter";
// ----------------------------------------------------------

// à la fin sinon pas appliqués
@import  "./responsive";
@import  "./light";
```

Pour plus de lisibilité et de compréhension, chaque page a son propre fichier scss. Ainsi il  n'y a pas de risque qu'une classe prenne le dessus sur une autre.
