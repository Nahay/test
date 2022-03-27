## Sommaire

- Partie Front
	- [Navigation](https://github.com/Nahay/test/blob/master/guide-dev-client.md)
	- Services
	- Structure
	- Style
- Partie Back
	- Models
	- Routes

<br/>

## Installation

Prérequis :
- [NodeJs](https://nodejs.org/en/download/)
- [MongoDB Community](https://www.mongodb.com/try/download/community)
- [Git](https://git-scm.com/downloads) ou télécharger le dépôt [ici](https://github.com/Nahay/Charlemagne/archive/refs/heads/master.zip)


Copier localement le repo :
```
git clone https://github.com/Nahay/Charlemagne.git
```

## Client

Aller dans le dossier du front
```
cd charlemagne-client
```
Installer les dépendances
```
npm i
```
Créer un fichier .env à la racine contenant l'url du back
```
REACT_APP_API_URL=http://localhost:5000
```
Run le projet
```
npm start
```

## Serveur

Aller dans le dossier du back
```
cd charlemagne-server
```
Installer les dépendances
```
npm i
```
Créer un fichier .env à la racine contenant le token et l'url du front
Le token est une chaine aléatoire de 25 caractères composée de chiffres et de lettres
```
JWT_TOKEN = DSHN3BqzE2Ggrtfk19DFQ56Po32Ym78VC28
ALLOWED_ORIGIN = http://localhost:3000
```
Run le projet
```js
npm start
```

## MongoDB

Pour que la base de données soit créée, ouvrir MongoDBCompass et cliquer sur `Fill in connection fields individually` , puis `connect` sans modifier les valeurs par défaut. La base de données sera créée automatiquement et sera accessible sous le nom de `charlemagne` .

![MongoDBCompass](https://cdn.discordapp.com/attachments/535830347942723584/956937700437549144/unknown.png)

- Dans la collection `admins` , il faudra créer un compte administrateur pour pouvoir accéder à la partie administrateur du site une première fois. Pour cela, cliquer sur `ADD DATA` puis `Insert Document` . En dessous d'`_id`, vous pourrez ajouter ce compte :

```json
username : "admin"
password : "$2b$08$HPNegUr6KQ4q3m2b3G8V1.r6fIVwcHShdNHDBMwX12jfBp/e9pT4."
// le mot de passe sera 'admin'
```

- Dans la catégorie `params` , il faudra également ajouter manuellement les deux messages personnalisables. Ils pourront ensuite être modifiés dans la partie admin du site :

```json
sentence : ""
type : "welcome"
```
```json
sentence : ""
type : "order"
```
