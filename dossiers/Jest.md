# Installation et Configuration de Jest
Jest est un framework de test pour JavaScript, voici son utilité principale :

1. **Pourquoi utiliser Jest ?**
- Vérifier que votre code fonctionne comme prévu
- Détecter les bugs avant qu'ils n'arrivent en production
- Faciliter la maintenance du code
- Documenter le comportement attendu du code
- Permettre les modifications sans crainte de casser quelque chose

2. **Fonctions principales :**
- Test unitaire : Tester des fonctions individuelles
- Test d'intégration : Tester comment différentes parties du code fonctionnent ensemble
- Test de comportement : Vérifier que l'application répond correctement aux actions
- Mocking : Simuler des comportements (API, base de données...)

3. **Exemple concret :**
```javascript
// Notre fonction
function additionner(a, b) {
    return a + b;
}

// Sans test :
// - Comment être sûr que ça marche ?
// - Comment savoir si une modification future va casser la fonction ?

// Avec Jest :
test('additionne correctement deux nombres', () => {
    expect(additionner(2, 3)).toBe(5);
    expect(additionner(-1, 1)).toBe(0);
    expect(additionner(0, 0)).toBe(0);
});
```

4. **Cas d'utilisation typiques :**
- Vérifier le traitement des données
- Tester les calculs
- Valider les transformations
- S'assurer que les erreurs sont bien gérées
- Vérifier les interactions avec une API


```bash
# Installation de Jest
npm install --save-dev jest
```

Dans votre `package.json`, ajoutez :
```json
{
  "scripts": {
    "test": "jest",
    "test-watch": "jest --watch"
  }
}
```

# 2. Structure des Tests

Les fichiers de test doivent suivre une convention de nommage :
- `nomFichier.test.js` ou
- `nomFichier.spec.js`

Structure recommandée du projet :
```
projet/
├── src/
│   └── calculateur.js
└── tests/
    └── calculateur.test.js
```

# 3. Anatomie d'un Test

Prenons un exemple simple :

```javascript
// src/calculateur.js
function additionner(a, b) {
    return a + b;
}

module.exports = { additionner };
```

```javascript
// tests/calculateur.test.js
const { additionner } = require('../src/calculateur');

// Suite de tests
describe('Calculateur', () => {
    // Test individuel
    test('additionne correctement 2 nombres', () => {
        expect(additionner(2, 3)).toBe(5);
    });
});
```

# 4. Fonctions Principales de Jest

## describe()
Créé un groupe de tests :
```javascript
describe('Groupe de tests pour la fonction X', () => {
    // Tests ici
});
```

## test() ou it()
Définit un test individuel :
```javascript
test('description du test', () => {
    // Test ici
});

// Ou
it('should do something', () => {
    // Test ici
});
```

## expect()
Crée une assertion :
```javascript
expect(valeurObtenue).toBe(valeurAttendue);
```

# 5. Matchers Courants

```javascript
// Égalité stricte
expect(2 + 2).toBe(4);

// Égalité d'objets
expect({nom: 'test'}).toEqual({nom: 'test'});

// Vérifications de type
expect(valeur).toBeNull();
expect(valeur).toBeDefined();
expect(valeur).toBeTruthy();
expect(valeur).toBeFalsy();

// Nombres
expect(valeur).toBeGreaterThan(3);
expect(valeur).toBeLessThan(5);

// Chaînes
expect('texte').toMatch(/expression/);

// Tableaux
expect(['pomme', 'banane']).toContain('pomme');
```

# 6. Exemple Pratique Complet

```javascript
// src/utilisateur.js
class Utilisateur {
    constructor(nom, age) {
        this.nom = nom;
        this.age = age;
    }

    estMajeur() {
        return this.age >= 18;
    }

    saluer() {
        return `Bonjour, je suis ${this.nom}`;
    }
}

module.exports = Utilisateur;
```

```javascript
// tests/utilisateur.test.js
const Utilisateur = require('../src/utilisateur');

describe('Classe Utilisateur', () => {
    // Hook d'initialisation
    let utilisateur;
    beforeEach(() => {
        utilisateur = new Utilisateur('Jean', 25);
    });

    test('création d\'un utilisateur', () => {
        expect(utilisateur.nom).toBe('Jean');
        expect(utilisateur.age).toBe(25);
    });

    test('vérification si majeur', () => {
        expect(utilisateur.estMajeur()).toBe(true);
        
        const mineur = new Utilisateur('Pierre', 15);
        expect(mineur.estMajeur()).toBe(false);
    });

    test('méthode saluer', () => {
        expect(utilisateur.saluer()).toBe('Bonjour, je suis Jean');
    });
});
```

# 7. Exécution des Tests

```bash
# Lancer tous les tests
npm test

# Mode watch (relance les tests à chaque modification)
npm run test-watch

# Lancer un fichier de test spécifique
npm test -- monFichier.test.js

# Avec couverture de code
npm test -- --coverage
```

# 8. Bonnes Pratiques

1. Un test par comportement
2. Noms de tests descriptifs
3. Organisation en suites logiques
4. Tests isolés et indépendants
5. Utilisation de beforeEach() pour le setup commun
6. Tests de cas limites et d'erreurs

# 9. Tests d'Asynchrone

```javascript
// Pour les Promesses
test('données async', () => {
    return fetchData().then(data => {
        expect(data).toBe('ok');
    });
});

// Avec async/await
test('données async', async () => {
    const data = await fetchData();
    expect(data).toBe('ok');
});
```
Je vais vous créer un "dictionnaire" détaillé des fonctions principales de Jest avec leurs utilisations :

# 1. Fonctions de Structure de Test

### `describe(name, fn)`
```javascript
describe('Calculateur', () => {
    // groupe de tests
});
```
- Crée un bloc qui regroupe plusieurs tests liés
- Aide à organiser les tests par thème/fonctionnalité
- Permet une meilleure lisibilité des rapports de test

### `test(name, fn)` ou `it(name, fn)`
```javascript
test('doit additionner deux nombres', () => {
    // test ici
});
```
- Définit un test individuel
- `test` et `it` sont identiques, choix stylistique
- Chaque test devrait vérifier une seule chose

# 2. Matchers (Comparateurs)

### Égalité
```javascript
expect(value).toBe(autre)         // Égalité stricte (===)
expect(value).toEqual(autre)      // Égalité profonde pour objets/tableaux
expect(value).toStrictEqual(autre)// Égalité très stricte (vérifie aussi les undefined)
```

### Vérifications de Type
```javascript
expect(value).toBeNull()          // Vérifie si null
expect(value).toBeUndefined()     // Vérifie si undefined
expect(value).toBeDefined()       // Vérifie si défini
expect(value).toBeTruthy()        // Vérifie si vrai
expect(value).toBeFalsy()         // Vérifie si faux
```

### Nombres
```javascript
expect(value).toBeGreaterThan(3)      // >
expect(value).toBeGreaterThanOrEqual(3)// >=
expect(value).toBeLessThan(5)         // 
expect(value).toBeLessThanOrEqual(5)  // <=
expect(value).toBeCloseTo(0.3)        // Pour les décimaux (évite erreurs d'arrondi)
```

### Chaînes
```javascript
expect(str).toMatch(/expression/)  // Test avec regex
expect(str).toContain('texte')    // Vérifie si contient
expect(str).toHaveLength(6)       // Vérifie la longueur
```

### Tableaux et Collections
```javascript
expect(array).toContain(item)     // Vérifie si élément présent
expect(array).toHaveLength(3)     // Vérifie la taille
expect(array).toContainEqual(obj) // Vérifie si objet similaire présent
```

# 3. Fonctions de Configuration

### `beforeEach(fn)`
```javascript
beforeEach(() => {
    // exécuté avant chaque test
});
```
- Prépare l'environnement de test
- Réinitialise des variables
- Configure des mocks

### `afterEach(fn)`
```javascript
afterEach(() => {
    // exécuté après chaque test
});
```
- Nettoie après les tests
- Ferme des connexions
- Réinitialise l'état

### `beforeAll(fn)` et `afterAll(fn)`
```javascript
beforeAll(() => {
    // Une fois avant tous les tests
});
```
- Configuration initiale
- Connexion à une base de données
- Création de données de test

# 4. Tests Asynchrones

### Pour les Promesses
```javascript
test('async test', () => {
    return maFonctionAsync().then(data => {
        expect(data).toBe('resultat');
    });
});
```

### Avec async/await
```javascript
test('async test', async () => {
    const data = await maFonctionAsync();
    expect(data).toBe('resultat');
});
```

# 5. Mocking (Simulation)

### `jest.fn()`
```javascript
const mockCallback = jest.fn();
mockCallback.mockReturnValue(42);
```
- Crée une fonction simulée
- Permet de suivre les appels
- Simule des retours

### `jest.spyOn()`
```javascript
jest.spyOn(object, 'method');
```
- Surveille une méthode existante
- Permet de vérifier si/comment elle est appelée

# 6. Exemple Complet
```javascript
describe('Service Utilisateur', () => {
    let userService;
    
    beforeEach(() => {
        userService = new UserService();
    });

    test('doit créer un utilisateur', async () => {
        const mockDB = jest.spyOn(database, 'save');
        const user = { name: 'Jean' };
        
        await userService.create(user);
        
        expect(mockDB).toHaveBeenCalledWith(user);
    });
});
```

Ces fonctions permettent de :
1. Organiser les tests logiquement
2. Vérifier précisément les résultats
3. Simuler des comportements complexes
4. Tester le code asynchrone
5. Isoler les tests les uns des autres

# Esemple ludique

Je vais créer un exemple ludique et détaillé d'une application de gestion de "Zoo virtuel" 🦁

```javascript
// animalManager.js - Notre gestionnaire de zoo virtuel 🎪
class AnimalManager {
    constructor() {
        this.animals = [];  // Notre liste d'animaux du zoo
    }

    // Ajoute un nouvel animal au zoo
    addAnimal(name, type, age) {
        const animal = {
            id: this.animals.length + 1,
            name,
            type,
            age,
            isHungry: false,
            happiness: 100
        };
        this.animals.push(animal);
        return animal;
    }

    // Nourrit un animal (change son état de faim)
    feedAnimal(id) {
        const animal = this.animals.find(a => a.id === id);
        if (!animal) throw new Error('Animal non trouvé ! 😢');
        
        animal.isHungry = false;
        animal.happiness += 10;
        return animal;
    }

    // Fait jouer un animal (augmente son bonheur)
    playWithAnimal(id) {
        const animal = this.animals.find(a => a.id === id);
        if (!animal) throw new Error('Animal non trouvé ! 😢');
        
        animal.happiness += 20;
        animal.isHungry = true;  // Le jeu donne faim !
        return animal;
    }
}

module.exports = AnimalManager;
```

```javascript
// animalManager.test.js - Nos tests pour le zoo ! 🧪
const AnimalManager = require('./animalManager');

// 🎯 On regroupe tous nos tests liés au zoo
describe('Zoo Virtuel - Tests', () => {
    // 🔧 Avant chaque test, on crée un nouveau zoo vide
    let zoo;
    beforeEach(() => {
        zoo = new AnimalManager();
    });

    // 📝 Tests pour l'ajout d'animaux
    describe('Ajout d\'animaux', () => {
        // ✨ Test simple d'ajout d'animal
        test('devrait ajouter un nouvel animal correctement', () => {
            // 🎬 Action : on ajoute un lion
            const lion = zoo.addAnimal('Simba', 'Lion', 2);

            // 🔍 Vérifications
            expect(lion).toEqual({
                id: 1,
                name: 'Simba',
                type: 'Lion',
                age: 2,
                isHungry: false,
                happiness: 100
            });
        });

        // 🎭 Test d'ajout multiple
        test('devrait gérer plusieurs animaux', () => {
            // 🎬 Actions : on ajoute plusieurs animaux
            const lion = zoo.addAnimal('Simba', 'Lion', 2);
            const giraffe = zoo.addAnimal('Sophie', 'Giraffe', 3);

            // 🔍 Vérifications
            expect(lion.id).toBe(1);
            expect(giraffe.id).toBe(2);
            expect(zoo.animals.length).toBe(2);
        });
    });

    // 🍖 Tests pour nourrir les animaux
    describe('Nourrissage des animaux', () => {
        // 🎯 Test du nourrissage
        test('devrait nourrir un animal affamé', () => {
            // 🎬 Préparation : on ajoute un animal
            const lion = zoo.addAnimal('Simba', 'Lion', 2);
            lion.isHungry = true;  // On le rend affamé

            // 🎬 Action : on nourrit le lion
            const fedLion = zoo.feedAnimal(1);

            // 🔍 Vérifications
            expect(fedLion.isHungry).toBe(false);
            expect(fedLion.happiness).toBe(110);  // 100 + 10 de bonus
        });

        // ❌ Test d'erreur
        test('devrait échouer avec un ID invalide', () => {
            // 🔍 Vérifie que ça lance bien une erreur
            expect(() => zoo.feedAnimal(999)).toThrow('Animal non trouvé ! 😢');
        });
    });

    // 🎮 Tests pour jouer avec les animaux
    describe('Jeu avec les animaux', () => {
        // 🎯 Test du jeu
        test('devrait rendre l\'animal heureux mais affamé', () => {
            // 🎬 Préparation
            const penguin = zoo.addAnimal('Rico', 'Penguin', 1);

            // 🎬 Action : on joue avec le pingouin
            const playfulPenguin = zoo.playWithAnimal(1);

            // 🔍 Vérifications
            expect(playfulPenguin.happiness).toBe(120);  // 100 + 20 de jeu
            expect(playfulPenguin.isHungry).toBe(true);  // Le jeu donne faim !
        });

        // 📊 Test complexe avec plusieurs interactions
        test('devrait gérer une séquence d\'interactions', () => {
            // 🎬 Préparation
            const elephant = zoo.addAnimal('Dumbo', 'Elephant', 5);

            // 🎬 Actions : on joue puis on nourrit
            zoo.playWithAnimal(1);  // Bonheur +20, devient affamé
            const finalState = zoo.feedAnimal(1);  // Plus affamé, bonheur +10

            // 🔍 Vérifications finales
            expect(finalState).toEqual({
                id: 1,
                name: 'Dumbo',
                type: 'Elephant',
                age: 5,
                isHungry: false,
                happiness: 130  // 100 + 20 (jeu) + 10 (nourriture)
            });
        });
    });
});
```

Pour exécuter ces tests :
```bash
npm test
```

Explications des concepts clés utilisés :

1. **describe** 🗂️
   - Comme des dossiers qui regroupent nos tests par thème
   - Permet une organisation claire et logique

2. **beforeEach** 🔄
   - Prépare un "zoo propre" avant chaque test
   - Évite les interférences entre les tests

3. **test** ✅
   - Chaque test vérifie une chose spécifique
   - Nom descriptif qui explique ce qu'on teste

4. **expect** 🔍
   - Vérifie que les résultats correspondent à nos attentes
   - Différents "matchers" pour différents types de vérifications

5. **Structure des tests** 📝
   - Préparation (setup)
   - Action (what we're testing)
   - Vérification (assertions)
