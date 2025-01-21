# Vocabulaire Complet et Ludique de Node.js

## Introduction
Ce document compile l'ensemble des mots-clés, fonctions, modules et concepts que tu dois connaître pour exploiter pleinement Node.js. Chaque terme est accompagné d'une définition claire et d'exemples concrets.

---

## 1. Mots-Clés Importants
Voici un tableau classant les principaux termes par fonction et utilité :

| **Terme**          | **Catégorie**              | **Définition**                                                                                              | **Exemple**                                                                                                   |
|---------------------|--------------------------|----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Node.js**         | Environnement           | Environnement permettant d'exécuter JavaScript côté serveur.                                             | `node app.js` pour lancer un fichier JavaScript.                                                             |
| **npm**             | Gestionnaire de modules | Node Package Manager, utilisé pour installer des bibliothèques externes.                                 | `npm install express` installe Express.                                                                     |
| **require()**       | Importation             | Fonction utilisée pour importer des modules.                                                             | `const fs = require('fs');` importe le module `fs`.                                                         |
| **module.exports**  | Exportation             | Permet de rendre des fonctions ou objets accessibles à d'autres fichiers.                               | `module.exports = function() { return 'Bonjour'; }`.                                                        |
| **callback**        | Asynchronisme           | Une fonction passée en argument pour être exécutée plus tard.                                            | `fs.readFile('file.txt', (err, data) => { console.log(data); });`.                                           |
| **Promise**         | Asynchronisme           | Objet représentant une opération asynchrone qui peut réussir ou échouer.                                | `promise.then(result => console.log(result));`.                                                              |
| **async/await**     | Asynchronisme           | Syntaxe moderne pour gérer les opérations asynchrones de manière plus lisible.                          | `const data = await fs.promises.readFile('file.txt');`.                                                     |
| **stream**          | Gestion des données      | Flux de données manipulées de manière continue (lecture ou écriture).                                   | `fs.createReadStream('file.txt').pipe(process.stdout);`.                                                     |
| **middleware**      | Serveur HTTP            | Fonction exécutée entre la réception d'une requête et l'envoi de la réponse.                            | `app.use((req, res, next) => { console.log('Middleware exécuté'); next(); });`.                         |
| **event loop**      | Architecture            | Mécanisme permettant à Node.js de gérer les tâches asynchrones.                                         | Automatique, utilisé dans toutes les opérations non bloquantes.                                              |
| **API REST**        | Communication           | Interface permettant de communiquer entre applications via HTTP.                                        | Endpoint GET `/api/users` pour récupérer une liste d'utilisateurs.                                         |
| **JSON**            | Données                | Format de données léger utilisé pour échanger des informations.                                         | `{ "nom": "John", "age": 30 }`.                                                                         |
| **nodemon**         | Outil                   | Outil pour redémarrer automatiquement une application Node.js après chaque modification de code.         | Lancer avec : `npx nodemon app.js`.                                                                         |

---

## 2. Principaux Modules de Node.js
Node.js inclut des modules natifs pour des tâches courantes. Voici les plus importants :

| **Module**      | **Description**                                   | **Exemple d'utilisation**                                                                                  |
|------------------|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **fs**          | Gestion des fichiers.                            | `fs.readFile('file.txt', (err, data) => { console.log(data); });`.                                          |
| **http**        | Création de serveurs web.                        | `http.createServer((req, res) => { res.end('Bonjour'); }).listen(3000);`.                                  |
| **path**        | Manipulation des chemins de fichiers.             | `const path = require('path'); console.log(path.basename('/test/file.txt'));`.                             |
| **os**          | Informations sur le système d'exploitation.       | `console.log(os.platform());`.                                                                             |
| **events**      | Gestion des événements personnalisés.             | `const EventEmitter = require('events'); const emitter = new EventEmitter(); emitter.on('test', () => {});`.|
| **crypto**      | Opérations de cryptographie.                      | `const crypto = require('crypto'); console.log(crypto.randomBytes(16).toString('hex'));`.                  |

---

## 3. Exemple de Fonctionnement : Serveur Web

### Créer un serveur simple

Voici un exemple minimaliste de serveur HTTP créé avec Node.js :

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Bonjour, Node.js!');
});

server.listen(3000, () => {
    console.log('Serveur en écoute sur le port 3000');
});
```

Explication :
1. **`require('http')`** : Importe le module `http` pour créer un serveur.
2. **`createServer`** : Crée un serveur qui répond à toutes les requêtes avec "Bonjour, Node.js!".
3. **`listen(3000)`** : Démarre le serveur sur le port 3000.

---

## 4. Exemple Complet d'une API REST avec Express
Voici comment créer une API REST simple pour gérer des utilisateurs :

### Installation des dépendances
```bash
npm init -y
npm install express
```

### Code complet

```javascript
const express = require('express');
const app = express();
const port = 3000;

// Middleware pour parser le JSON
app.use(express.json());

// Données simulées
let users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' }
];

// Lire tous les utilisateurs (GET)
app.get('/users', (req, res) => {
    res.json(users);
});

// Lire un utilisateur spécifique (GET)
app.get('/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).send('Utilisateur non trouvé');
    res.json(user);
});

// Créer un nouvel utilisateur (POST)
app.post('/users', (req, res) => {
    const newUser = {
        id: users.length + 1,
        name: req.body.name
    };
    users.push(newUser);
    res.status(201).json(newUser);
});

// Mettre à jour un utilisateur (PUT)
app.put('/users/:id', (req, res) => {
    const user = users.find(u => u.id === parseInt(req.params.id));
    if (!user) return res.status(404).send('Utilisateur non trouvé');
    user.name = req.body.name;
    res.json(user);
});

// Supprimer un utilisateur (DELETE)
app.delete('/users/:id', (req, res) => {
    users = users.filter(u => u.id !== parseInt

