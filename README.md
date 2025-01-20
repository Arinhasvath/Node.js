# Node.js
intro a nodejs
# Cours Ludique : Les Bases de Node.js

## Introduction

Node.js est une plateforme qui permet d'exécuter du code JavaScript côté serveur. Elle est rapide, légère, et idéale pour créer des applications modernes comme des API ou des serveurs web.

---

## 1. Prérequis

Avant de commencer, assure-toi d'avoir les éléments suivants :

- **Un éditeur de texte** : Visual Studio Code (VSC) est recommandé.
- **Node.js installé** : Télécharge-le depuis [nodejs.org](https://nodejs.org).

### Vérification de l'installation

- Ouvre un terminal et tape les commandes suivantes :
  ```bash
  node -v    # Vérifie la version de Node.js
  npm -v     # Vérifie la version de NPM (Node Package Manager)
  ```

---

## 2. Créer un premier projet Node.js

1. **Créer un dossier projet** :

   ```bash
   mkdir projet-node
   cd projet-node
   ```

2. **Initialiser le projet** :

   ```bash
   npm init -y
   ```

   Cela génère un fichier `package.json` qui contient les informations du projet.

3. **Créer un fichier principal** :
   Crée un fichier nommé `app.js`.

4. **Écrire le premier code Node.js** :
   Dans `app.js`, écris :

   ```javascript
   console.log('Bonjour, Node.js!');
   ```

   Exécute le fichier :

   ```bash
   node app.js
   ```

---

## 3. Modules Natifs Importants

Node.js est livré avec des modules intégrés pour accomplir des tâches courantes. Voici les plus importants :

### 3.1. **fs (File System)**

Manipule des fichiers.

- Lire un fichier :

  ```javascript
  const fs = require('fs');

  fs.readFile('fichier.txt', 'utf8', (err, data) => {
      if (err) throw err;
      console.log(data);
  });
  ```

- Écrire dans un fichier :

  ```javascript
  fs.writeFile('nouveau.txt', 'Contenu écrit', (err) => {
      if (err) throw err;
      console.log('Fichier créé.');
  });
  ```

### 3.2. **http**

Créer un serveur web basique.

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Bienvenue sur mon serveur Node.js!');
});

server.listen(3000, () => {
    console.log('Serveur en écoute sur le port 3000');
});
```

Ouvre `http://localhost:3000` dans ton navigateur.

---

## 4. Modules NPM Importants

NPM (Node Package Manager) permet d'installer des bibliothèques externes.

### Installer un module (par exemple, Express) :

```bash
npm install express
```

### Exemple avec Express :

Crée une application web simple :

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Bonjour avec Express!');
});

app.listen(3000, () => {
    console.log('Serveur Express en écoute sur le port 3000');
});
```

---

## 5. Tâches Asynchrones

Node.js est conçu pour exécuter des tâches de manière non bloquante.

- Avec **callback** :

  ```javascript
  setTimeout(() => {
      console.log('Tâche exécutée après 2 secondes');
  }, 2000);
  ```

- Avec **promesses** :

  ```javascript
  const promise = new Promise((resolve, reject) => {
      const success = true;
      if (success) resolve('Promesse tenue!');
      else reject('Promesse échouée.');
  });

  promise
      .then((message) => console.log(message))
      .catch((err) => console.log(err));
  ```

---

## 6. Bonnes Pratiques pour Débuter

1. **Structure du projet** : Organise ton code par modules et dossiers.

   - Exemple :
     ```
     /projet-node
     |-- app.js
     |-- routes/
     |-- models/
     |-- views/
     ```

2. **Modules utiles à installer** :

   - `express` : Framework web.
   - `dotenv` : Gestion des variables d'environnement.
   - `nodemon` : Redémarre automatiquement ton application après chaque modification.
     ```bash
     npm install --save-dev nodemon
     ```
     Lancer avec :
     ```bash
     npx nodemon app.js
     ```

---

## Conclusion

Avec ces bases, tu peux commencer à développer des projets simples en Node.js. Pour aller plus loin, explore les frameworks comme Express, la gestion des bases de données (MySQL, MongoDB), et les outils comme Docker pour déployer tes applications.

**À toi de jouer !**

