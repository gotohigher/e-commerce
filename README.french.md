[✔]: assets/images/checkbox-small-blue.png

# Bonnes pratiques Node.js

<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Bonnes pratiques Node.js">
</h1>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/⚙%20Item%20count%20-%20102%20Best%20Practices-blue.svg" alt="102 items"> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%20February%2012%202021-green.svg" alt="Dernière mise à jour : Novembre 2020"> <img src="https://img.shields.io/badge/ %E2%9C%94%20Updated%20For%20Version%20-%20Node%2014.0.0-brightgreen.svg" alt="Mis à jour pour Node 14.0.0">
</div>

<br/>

[![nodepractices](/assets/images/twitter-s.png)](https://twitter.com/nodepractices/) **Suivez nous sur Twitter !** [**@nodepractices**](https://twitter.com/nodepractices/)

<br/>

Lire dans une autre langue : [![CN](/assets/flags/CN.png)**CN**](/README.chinese.md), [![BR](/assets/flags/BR.png)**BR**](/README.brazilian-portuguese.md), [![RU](/assets/flags/RU.png)**RU**](/README.russian.md), [![PL](/assets/flags/PL.png)**PL**](/README.polish.md), [![JA](/assets/flags/JA.png)**JA**](/README.japanese.md), [(![ES](/assets/flags/ES.png)**ES**, ![FR](/assets/flags/FR.png)**FR**, ![HE](/assets/flags/HE.png)**HE**, ![KR](/assets/flags/KR.png)**KR**, ![TR](/assets/flags/TR.png)**TR** et ![EU](/assets/flags/EU.png)**EU** en cours ! )](#traductions)

<br/>

###### Construit et entretenu par notre [comité de pilotage](#comité-de-pilotage) et nos [collaborateurs](#collaborateurs)

# Dernières bonnes pratiques et nouveautés

- **🇯🇵 traduction japonaise :** Notre guide est désormais également traduit en japonais ! Avec l'aimable autorisation des extraordinaires [YukiOta](https://github.com/YukiOta) et [Yuta Azumi](https://github.com/YA21).

- **✅ Nouvelle bonne pratique :** Le point 6.8 de Josh Hemphill recommande de « protéger les mots de passe/secrets des utilisateurs en utilisant BCrypt ou Script ». Elle contient une explication approfondie sur le moment et les raisons pour lesquelles chaque option convient à un projet spécifique. Ne manquez pas ce petit guide avec un bref aperçu des différentes options de hachage

- **🎊 60,000 stars !** : Notre dépôt a reçu la reconnaissance et la confiance de 60 100 développeurs. Nous sommes sans voix.

<br/><br/>

# Bienvenue ! 3 Choses à savoir avant tout

**1. Vous êtes en train de lire un regroupement des meilleurs articles sur Node.js. -** ce référentiel est un résumé et il conserve le contenu le mieux classé sur les bonnes pratiques Node.js, ainsi que du contenu écrit ici par des collaborateurs

**2. Il s'agit du plus grand assemblage d'articles et il s'agrandit chaque semaine -** actuellement, plus de 80 bonnes pratiques, guides de style et astuces d'architecture sont présentées. Nous serions ravis de vous voir contribuer ici, qu'il s'agisse de corriger des erreurs de code, d'aider aux traductions ou de suggérer de nouvelles idées brillantes. Consultez nos [recommandations d'écriture](/.operations/writing-guidelines.french.md)

**3. Les bonnes pratiques contiennent des informations supplémentaires -** la plupart des points ont un lien **🔗Plus d'infos** qui développe la bonne pratique avec des exemples de code, des citations venant de pages sélectionnées et plus encore.

<br/><br/>

## Table des matières

1. [Structure de projet (5)](#1-structure-de-projet)
2. [Gestion des erreurs (11) ](#2-gestion-des-erreurs)
3. [Style du code (12) ](#3-style-du-code)
4. [Tests et pratiques générales de qualité (13) ](#4-tests-et-pratiques-générales-de-qualité)
5. [Pratiques de mise en production (19) ](#5-pratiques-de-mise-en-production)
6. [Sécurité (25)](#6-bonnes-pratiques-de-sécurité)
7. [Performance (2) (Travail en cours ✍️)](#7-brouillon-bonnes-pratiques-de-performance)
8. [Pratiques de Docker (15)](#8-bonnes-pratiques-de-docker)

<br/><br/>

# `1. Structure de projet`

## ![✔] 1.1 Organisez votre projet en composants

**TL;PL :** Le pire obstacle des énormes applications est la maintenance d'une base de code immense contenant des centaines de dépendances - un tel monolithe ralentit les développeurs tentant d'ajouter de nouvelles fonctionnalités. Pour éviter cela, répartissez votre code en composants, chacun dans son dossier avec son code dédié, et assurez vous que chaque unité soit courte et simple. Visitez le lien « Plus d'infos » plus bas pour voir des exemples de structure de projet correcte.

**Autrement :** Lorsque les développeurs qui codent de nouvelles fonctionnalités ont du mal à réaliser l'impact de leur changement et craignent de casser d'autres composants dépendants - les déploiements deviennent plus lents et plus risqués. Il est aussi considéré plus difficile d'élargir un modèle d'application quand les unités opérationnelles ne sont pas séparées.

🔗 [**Plus d'infos : organisez en composants**](/sections/projectstructre/breakintcomponents.french.md)

<br/><br/>

## ![✔] 1.2 Organisez vos composants en strates, gardez la couche web à l'intérieur de son périmètre

**TL;PL :** Chaque composant devrait contenir des « strates » - un objet dédié pour le web, un pour la logique et un pour le code d'accès aux données. Cela permet non seulement de séparer clairement les responsabilités mais permet aussi de simuler et de tester le système de manière plus simple. Bien qu'il s'agisse d'un modèle très courant, les développeurs d'API ont tendance à mélanger les strates en passant l'objet dédié au web (Par exemple Express req, res) à la logique opérationnelle et aux strates de données - cela rend l'application dépendante et accessible seulement par les frameworks web spécifiques.

**Autrement :** Les tests, les jobs CRON, les déclencheurs des files d'attente de messages et etc ne peuvent pas accéder à une application qui mélange les objets web avec les autres strates.

🔗 [**Plus d'infos : organisez en strates votre app**](/sections/projectstructre/createlayers.french.md)

<br/><br/>

## ![✔] 1.3 Externalisez les utilitaires communs en paquets NPM

**TL;PL :** Dans une grande appli rassemblant de nombreuses lignes de codes, les utilitaires opérant sur toutes les strates comme un logger, l'encryption et autres, devraient être inclus dans le code et exposés en tant que paquets NPM privés. Cela permet leur partage au sein de plusieurs projets.

**Autrement :** Vous devrez inventer votre propre roue de déploiement et de dépendance

🔗 [**Plus d'infos : organisez par fonction**](/sections/projectstructre/wraputilities.french.md)

<br/><br/>

## ![✔] 1.4 Séparez Express 'app' et 'server'

**TL;PL :** Evitez la sale habitude de définir l'appli [Express](https://expressjs.com/) toute entière dans un seul fichier immense - séparez la définition de votre 'Express' en au moins deux fichiers : la déclaration de l'API (app.js) et les responsabilités de gestion de réseau (WWW). Pour une structure encore plus poussée, localisez la déclaration de l'API dans les composants.

**Autrement :** Votre API sera seulement accessible aux tests par le biais d'appels HTTP (plus lent et plus difficile de générer des rapports de couverture). Cela ne sera pas un réel plaisir de maintenir des centaines de lignes de code dans un fichier unique.

🔗 [**Plus d'infos : séparez Express 'app' et 'server'**](/sections/projectstructre/separateexpress.french.md)

<br/><br/>

## ![✔] 1.5 Utilisez une configuration respectueuse de l'environnement, sécurisée et hiérarchique

**TL;PL :** La mise en place d'une configuration parfaite et sans faille doit garantir que (a) les clés peuvent être lues depuis un fichier ET à partir de la variable d'environnement (b) les secrets sont conservés hors du code source (c) la configuration est hiérarchique pour une recherche plus simple. Certains paquets peuvent gérer la plupart de ces points comme [rc](https://www.npmjs.com/package/rc), [nconf](https://www.npmjs.com/package/nconf), [config](https://www.npmjs.com/package/config) et [convict](https://www.npmjs.com/package/convict).

**Autrement :** Ne pas se soucier de ces exigences de configuration ne fera que ralentir l'équipe de développement ou l'équipe de DevOps. Probablement les deux.

🔗 [**Plus d'infos : bonnes pratiques de configuration**](/sections/projectstructre/configguide.french.md)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `2. Gestion des erreurs`

## ![✔] 2.1 Utilisez Async-Await ou les promesses pour le traitement des erreurs asynchrones

**TL;PL :** Gérer les erreurs asynchrone avec le style fonction de rappel est probablement le chemin le plus rapide vers l'enfer (ou la [pyramide condamnée](https://fr.wikipedia.org/wiki/Pyramide_condamn%C3%A9e)). Le meilleur cadeau que vous puissiez faire à votre code est d'utiliser une bibliothèque de promesses réputée ou async-await à la place, ceci permet une syntaxe de code beaucoup plus compacte et familière comme try-catch.

**Autrement :** Le style fonction de rappel de Node.js, function(err, response), constituent une autre manière d’obtenir une solution non maintenable mêlant gestion de l’erreur avec du code ordinaire, des imbrications excessives et une conception bancale.

🔗 [**Plus d'infos : évitez les fonctions de rappel**](/sections/errorhandling/asyncerrorhandling.french.md)

<br/><br/>

## ![✔] 2.2 Utilisez uniquement l'objet intégré Error

**TL;PL :** Beaucoup lèvent des erreurs sous forme de chaîne ou de type personnalisé - cela complique la logique de gestion des erreurs et l'interopérabilité entre les modules. Que vous rejetiez une promesse, leviez une exception ou émettiez une erreur - l'utilisation uniquement de l'objet intégré Error (ou un objet qui étend l'objet Error) augmentera l'uniformité et empêchera la perte d'informations. Il existe une règle ESLint `no-throw-literal` qui vérifie strictement cela (bien qu'elle ait quelques [limitations](https://eslint.org/docs/rules/no-throw-literal) qui peuvent être résolues en utilisant TypeScript et en définissant la règle `@typescript-eslint/no-throw-literal`).

**Autrement :** Lorsque vous appelez un composant, le type d'erreurs en retour étant incertain - cela rend la gestion des erreurs beaucoup plus difficile. Pire encore, l'utilisation de types personnalisés pour décrire des erreurs peut entraîner la perte d'informations d'erreurs critiques comme la trace de la pile !

🔗 [**Plus d'infos : utilisez uniquement l'objet intégré Error**](/sections/errorhandling/useonlythebuiltinerror.french.md)

<br/><br/>

## ![✔] 2.3 Distinguez les erreurs opérationnelles des erreurs de programmation

**TL;PL :** Les erreurs opérationnelles (par exemple, l'API a reçu une entrée non valide) se rapportent à des cas connus où l'impact de l'erreur est entièrement compris et peut être géré de manière réfléchie. D'autre part, une erreur de programmation (par exemple, essayer de lire une variable non définie) fait référence à des échecs de code inconnus qui dictent de redémarrer l'application en douceur.

**Autrement :** Vous pouvez toujours redémarrer l'application lorsqu'une erreur apparaît, mais pourquoi lâcher environ 5000 utilisateurs en ligne en raison d'une erreur opérationnelle mineure prévue ? L'inverse n'est pas non plus idéal - laisser l'application en place lorsqu'un problème inconnu (erreur de programmation) s'est produit peut conduire à un comportement imprévu. Différencier les deux permet d'agir avec tact et d'appliquer une approche équilibrée en fonction du contexte donné.

🔗 [**Plus d'infos : erreur opérationnelle vs erreur de programmation**](/sections/errorhandling/operationalvsprogrammererror.french.md)

<br/><br/>

## ![✔] 2.4 Gérez les erreurs de manière centralisée, pas dans un middleware

**TL;PL :** Les logiques de gestion des erreurs telles que le mail à l'administrateur et la journalisation doivent être encapsulées dans un objet dédié et centralisé, pour que tous les points de terminaison (par exemple, middleware Express, tâches cron, tests unitaires) l'appellent lorsqu'une erreur survient.

**Autrement :** Ne pas traiter les erreurs dans un seul endroit entraînera une duplication de code et probablement des erreurs mal gérées

🔗 [**Plus d'infos : gestion des erreurs dans un lieu centralisé**](/sections/errorhandling/centralizedhandling.french.md)

<br/><br/>

## ![✔] 2.5 Documentez les erreurs de l'API à l'aide de Swagger ou GraphQL

**TL;PL :** Faites savoir à vos appelants de l'API quelles erreurs peuvent survenir en retour afin de pouvoir les traiter de manière réfléchie sans planter. Pour les API RESTful, cela se fait généralement avec des frameworks de documentation comme Swagger. Si vous utilisez GraphQL, vous pouvez également utiliser votre schéma et vos commentaires.

**Autrement :** Un client d'une API peut décider de planter et de redémarrer uniquement parce qu'il a reçu une erreur qu'il ne comprend pas. Remarque: l'appelant de votre API peut être vous (très typique dans un environnement de microservice)

🔗 [**Plus d'infos : documentez les erreurs de l'API à l'aide de Swagger ou GraphQL**](/sections/errorhandling/documentingusingswagger.french.md)

<br/><br/>

## ![✔] 2.6 Quittez le processus avec élégance lorsqu'un étranger arrive en ville

**TL;PL :** Lorsqu'une erreur inconnue se produit (une erreur de programmation, voir la bonne pratique 2.3) - il existe une incertitude sur la bonne santé de l'application. Une pratique courante suggère de redémarrer le processus avec précaution à l'aide d'un outil de gestion des processus comme [Forever](https://www.npmjs.com/package/forever) ou [PM2](http://pm2.keymetrics.io/).

**Autrement :** Lorsqu'une exception inconnue se produit, certains objets peuvent être dans un état défectueux (par exemple, un émetteur d'événements qui est utilisé globalement et qui ne déclenche plus d'événements en raison d'une défaillance interne) et toutes les demandes futures peuvent échouer ou se comporter de manière folle.

🔗 [**Plus d'infos : arrêtez le processus**](/sections/errorhandling/shuttingtheprocess.french.md)

<br/><br/>

## ![✔] 2.7 Utilisez un outil de journalisation mature pour augmenter la visibilité des erreurs

**TL;PL :** Un ensemble d'outils de journalisation matures comme [Pino](https://github.com/pinojs/pino) ou [Log4js](http://stritti.github.io/log4js/), accélérera la découverte et la compréhension des erreurs. Alors oubliez console.log.

**Autrement :** En parcourant les console.logs ou manuellement par le biais d'un fichier texte désordonné sans outils d'interrogation ou d'une visionneuse de journaux décente, vous pourriez être occupé au travail jusqu'à tard dans la nuit.

🔗 [**Plus d'infos : utilisation d'un outil de journalisation mature**](/sections/errorhandling/usematurelogger.french.md)

<br/><br/>

## ![✔] 2.8 Testez les flux d'erreurs en utilisant votre framework de test préféré

**TL;PL :** Qu'il s'agisse d'un outil automatisée d'assurance qualité professionnelle ou de tests manuels simples pour les développeurs - Assurez-vous que votre code non seulement satisfait les scénarios positifs, mais gère et renvoie également les bonnes erreurs. Les frameworks de test comme Mocha & Chai peuvent gérer cela facilement (voir les exemples de code dans « Plus d'infos »)

**Autrement :** Sans test, que ce soit automatiquement ou manuellement, vous ne pouvez pas compter sur votre code pour renvoyer les bonnes erreurs. Sans erreurs significatives - il n'y a pas de gestion des erreurs.

🔗 [**Plus d'infos : test des flux d'erreurs**](/sections/errorhandling/testingerrorflows.french.md)

<br/><br/>

## ![✔] 2.9 Découvrez les erreurs et les indisponibilités à l'aide des produits de gestion de la performance applicative

**TL;PL :** Les produits de surveillance et de performance (alias GPA, APM en anglais : application performance management) évaluent de manière proactive votre base de code ou votre API afin qu'ils puissent mettre en évidence automatiquement les erreurs, les plantages et les parties lentes qui vous ont échappé.

**Autrement :** Vous pourriez consacrer beaucoup d'efforts à mesurer les performances et les indisponibilités de l'API, vous ne saurez probablement jamais quelles sont vos parties de code les plus lentes dans le scénario du monde réel et comment celles-ci affectent l'expérience utilisateur.

🔗 [**Plus d'infos : utilisation des produits GPA**](/sections/errorhandling/apmproducts.french.md)

<br/><br/>

## ![✔] 2.10 Capturez les rejets de promesses non gérés

**TL;PL :** Toute exception levée dans une promesse sera absorbée et écartée à moins qu'un développeur n'ait pas oublié de le traiter explicitement. Même si votre code est abonné à `process.uncaughtException` ! Surmontez cela en vous inscrivant à l'événement `process.unhandledRejection`.

**Autrement :** Vos erreurs seront absorbées et ne laisseront aucune trace. Il n'y a pas de quoi s'inquiéter.

🔗 [**Plus d'infos : capturez les rejets de promesses non gérés**](/sections/errorhandling/catchunhandledpromiserejection.french.md)

<br/><br/>

## ![✔] 2.11 Échouez rapidement, valider les arguments à l'aide d'une bibliothèque dédiée

**TL;PL :** Contrôlez les arguments de l'API pour éviter les bugs désagréables qui sont beaucoup plus difficiles à suivre plus tard. Le code de validation est généralement fastidieux, sauf si vous utilisez une bibliothèque d'aide très cool comme [ajv](https://www.npmjs.com/package/ajv) et [Joi](https://www.npmjs.com/package/joi).

**Autrement :** Considérez ceci - votre fonction attend un argument numérique « Discount » que l'appelant oublie de passer, plus loin dans le code, il vérifie si Discount!= 0 (le montant de la remise autorisée est supérieur à zéro), ensuite le code permet à l'utilisateur de profiter d'un remise. OMG, quel méchant bug. Le vois-tu ?

🔗 [**Plus d'infos : échec rapide**](/sections/errorhandling/failfast.french.md)

<br/><br/>

## ![✔] 2.12 Attendez toujours les promesses avant de retourner afin d'éviter des traces de pile partielles

**TL;PL :** Faites toujours `return await` lorsque vous retournez une promesse afin de bénéficier d'une trace de pile complète. Si une
fonction retourne une promesse, cette fonction doit être déclarée comme fonction `async` et explicitement
attendre (`await`) la promesse avant de la retourner.

**Autrement :** La fonction qui retourne une promesse sans attendre n'apparaîtra pas dans la trace de la pile.
De telles trames manquantes compliqueraient probablement la compréhension du flux qui conduit à l'erreur,
surtout si la cause du comportement anormal se situe à l'intérieur de la fonction manquante

🔗 [**Plus d'infos : le retour des promesses**](/sections/errorhandling/returningpromises.french.md)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `3. Style du code`

## ![✔] 3.1 Utilisez ESLint

**TL;PL :** [ESLint](https://eslint.org) est la norme de facto pour vérifier d'éventuelles erreurs de code et pour corriger le style du code, ce n'est pas uniquement pour identifier les problèmes d'espacement mais aussi pour détecter les antipatterns préoccupants du code comme par exemple les développeurs levant des erreurs sans classification. Bien qu'ESLint puisse corriger automatiquement les styles du code, d'autres outils comme [prettier](https://www.npmjs.com/package/prettier) et [beautify](https://www.npmjs.com/package/js-beautify) sont plus puissants dans le formatage de la correction et fonctionnent en collaboration avec ESLint.

**Autrement :** Les développeurs se concentreront sur les problèmes fastidieux d'espacement et de largeur de ligne, ce qui pourrait faire perdre du temps à trop réfléchir sur le style de code du projet.

🔗 [**Plus d'infos : Utilisez ESLint et Prettier**](/sections/codestylepractices/eslint_prettier.french.md)

<br/><br/>

## ![✔] 3.2 Plugins spécifiques à Node.js

**TL;PL :** En plus des règles standard ESLint couvrant JavaScript vanilla, ajoutez des plugins spécifiques à Node.js comme [eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node), [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) et [eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security).

**Autrement :** De nombreux modèles de code Node.js défectueux peuvent s'échapper des radars. Par exemple, les développeurs pourrait exiger des fichiers avec une variable donnée comme un chemin d'accès (`require(variableCommeChemin)`) qui permet aux attaquants d'exécuter n'importe quel script JS. Les linters de Node.js peuvent détecter de tels modèles et se plaindre en amont.

<br/><br/>

## ![✔] 3.3 Commencez les accolades d'un bloc de code sur la même ligne

**TL;PL :** Les accolades ouvrantes d'un bloc de code doivent être sur la même ligne que l'instruction d'ouverture.

### Code Example

```javascript
// À faire
function someFunction() {
  // bloc de code
}

// À éviter
function someFunction
{
  // bloc de code
}
```

**Autrement :** Le non-respect de cette bonne pratique peut conduire à des résultats inattendus, comme le montre la discussion de StackOverflow ci-dessous :

🔗 [**Plus d'infos :** « Pourquoi les résultats varient-ils en fonction du placement des accolades ? » (StackOverflow)](https://stackoverflow.com/questions/3641519/why-does-a-results-vary-based-on-curly-brace-placement)

<br/><br/>

## ![✔] 3.4 Séparez correctement vos instructions

Peu importe si vous utilisez les points-virgules ou non pour séparer vos instructions, le fait de connaître les pièges courants des sauts de ligne incorrects ou de l'insertion automatique de points-virgules, vous aidera à éliminer les erreurs syntaxiques habituelles.

**TL;PL :** Utilisez ESLint pour vous sensibiliser aux problèmes de séparation. [Prettier](https://prettier.io/) ou [Standardjs](https://standardjs.com/) peuvent résoudre automatiquement ces problèmes.

**Autrement :** Comme vu dans la section précédente, l'interpréteur JavaScript ajoute automatiquement un point-virgule à la fin d'une instruction s'il n'y en a pas, ou considère une instruction comme non terminée là où elle devrait, ce qui pourrait conduire à des résultats indésirables. Vous pouvez utiliser des affectations et éviter d'utiliser des expressions de fonction invoquées immédiatement pour éviter la plupart des erreurs inattendues.

### Exemple de code

```javascript
// À faire
function doThing() {
    // ...
}

doThing()

// À faire

const items = [1, 2, 3]
items.forEach(console.log)

// À éviter — lève une exception
const m = new Map()
const a = [1,2,3]
[...m.values()].forEach(console.log)
> [...m.values()].forEach(console.log)
>  ^^^
> SyntaxError: Unexpected token ...

// À éviter — lève une exception
const count = 2 // il essaie d'exécuter 2(), mais 2 n'est pas une fonction
(function doSomething() {
  // faire quelque chose d'incroyable
}())
// placez un point-virgule avant la fonction immédiatement invoquée, après la définition de const, enregistrez la valeur de retour de la fonction anonyme dans une variable ou évitez tous les IIFE
```

🔗 [**Plus d'infos :** « Règle de ESLint : points-virgules »](https://eslint.org/docs/rules/semi)
🔗 [**Plus d'infos :** « Règle de ESLint : pas de multiligne inattendue »](https://eslint.org/docs/rules/no-unexpected-multiline)

<br/><br/>

## ![✔] 3.5 Nommez vos fonctions

**TL;PL :** Nommez toutes les fonctions, y compris les fermetures *(closures, NdT)* et les fonctions de rappel. Évitez les fonctions anonymes. Cela est particulièrement utile lors du profilage d'une application de Node. Nommer toutes les fonctions vous permettra de comprendre facilement ce que vous regardez lors de la vérification d'un instantané de mémoire *(snapshot memory, NdT)*.

**Autrement :** Le débogage des problèmes de production à l'aide d'un vidage de mémoire (instantané de mémoire) peut devenir difficile lorsque vous remarquez une consommation de mémoire importante de la part de fonctions anonymes.

<br/><br/>

## ![✔] 3.6 Utilisez des conventions de nommage pour les variables, les constantes, les fonctions et les classes

**TL;PL :** Utilisez **_LowerCamelCase_** lorsque vous nommez des constantes, des variables et des fonctions et **_UpperCamelCase_** (première lettre en majuscule également) lorsque vous nommez des classes. Cela vous aidera à distinguer facilement les simples variables/fonctions et les classes qui nécessitent une instanciation. Utilisez des noms évocateurs, mais efforcez-vous de les garder concis.

**Autrement :** JavaScript est le seul langage au monde qui permet d'invoquer directement un constructeur (« Class ») sans l'instancier au préalable. Par conséquent, les classes et les fonctions-constructeurs sont différenciés en commençant par UpperCamelCase.

### 3.6 Exemple de code

```javascript
// pour le nom d'une classe, nous utilisons UpperCamelCase
class SomeClassExample {}

// pour les noms de constantes, nous utilisons le mot-clé const et lowerCamelCase
const config = {
  key: "value",
};

// pour les noms de variables et de fonctions, nous utilisons lowerCamelCase
let someVariableExample = "value";
function doSomething() {}
```

<br/><br/>

## ![✔] 3.7 Préférez const à let. Laissez tomber le var

**TL;PL :** L'utilisation de `const` signifie qu'une fois qu'une variable est affectée, elle ne peut pas être réaffectée. Préférer `const` vous aidera à ne pas être tenté d'utiliser la même variable pour différentes utilisations et rendra votre code plus clair. Si une variable doit être réaffectée, par exempledans une boucle for, utilisez `let` pour la déclarer. Un autre aspect important de `let` est qu'une variable déclarée l'utilisant n'est disponible que dans la portée du bloc dans laquelle elle a été définie. `var` est une portée de fonction, pas une portée de bloc, et [ne devrait pas être utilisé en ES6](https://hackernoon.com/why-you-shouldnt-use-var-anymore-f109a58b9b70) maintenant que vous avez `const` et `let` à votre disposition.

**Autrement :** Le débogage devient beaucoup plus lourd lorsque vous suivez une variable qui change fréquemment.

🔗 [**Plus d'infos : JavaScript ES6+ : var, let, ou const ?** ](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

<br/><br/>

## ![✔] 3.8 Utilisez en premier require pour les modules, pas dans des fonctions internes

**TL;PL :** Utilisez au début de chaque fichier `require` pour les modules, avant et en dehors de toute fonction. Cette bonne pratique simple vous aidera non seulement à identifier facilement et rapidement les dépendances en haut d'un fichier, mais évite également quelques problèmes potentiels.

**Autrement :** Les `require` sont exécutées de manière synchrone par Node.js. S'ils sont appelés depuis une fonction, cela peut empêcher le traitement d'autres demandes à un moment plus critique. De plus, si un module requis ou l'une de ses dépendances lance une erreur et plante le serveur, il est préférable de le découvrir le plus tôt possible, ce qui pourrait ne pas être le cas si ce module est requis depuis une fonction.

<br/><br/>

## ![✔] 3.9 Exiger les modules par leurs dossiers, contrairement à l'appel direct aux fichiers

**TL;PL :** Lorsque vous développez un module/bibliothèque dans un dossier, placez un fichier index.js qui expose les composants internes du module afin que chaque utilisateur puisse y accéder. Cela sert d '« interface » à votre module et facilite les modifications futures sans rompre le contrat.

**Autrement :** La modification de la structure interne des fichiers ou de la signature peut rompre l'interface avec les clients.

### 3.9 Exemple de code

```javascript
// À faire
module.exports.SMSProvider = require("./SMSProvider");
module.exports.SMSNumberResolver = require("./SMSNumberResolver");

// À éviter
module.exports.SMSProvider = require("./SMSProvider/SMSProvider.js");
module.exports.SMSNumberResolver = require("./SMSNumberResolver/SMSNumberResolver.js");
```

<br/><br/>

## ![✔] 3.10 Utilisez l'opérateur `===`

**TL;PL :** Préférez l'opérateur d'égalité stricte `===` à l'opérateur d'égalité abstraite plus faible `==`. `==` comparera deux variables après les avoir converties en un type commun. Il n'y a pas de conversion de type dans `===`, et les deux variables doivent être du même type pour être égales.

**Autrement :** Les variables inégales peuvent renvoyer true avec l'opérateur `==`.

### 3.10 Exemple de code

```javascript
"" == "0"; // false
0 == ""; // true
0 == "0"; // true

false == "false"; // false
false == "0"; // true

false == undefined; // false
false == null; // false
null == undefined; // true

" \t\r\n " == 0; // true
```

Toutes les déclarations ci-dessus renverront false si elles sont utilisées avec `===`

<br/><br/>

## ![✔] 3.11 Utilisez Async Await, évitez les fonctions de rappel

**TL;PL :** Node 8 LTS prend désormais entièrement en charge Async-wait. Il s'agit d'une nouvelle façon de gérer le code asynchrone qui remplace les fonctions de rappel et les promesses. L'attente asynchrone n'est pas bloquante et rend le code asynchrone synchrone. Le meilleur cadeau que vous puissiez faire à votre code est d'utiliser async-wait qui fournit une syntaxe de code beaucoup plus compacte et familière comme try-catch.

**Autrement :** La gestion des erreurs asynchrones dans le style des fonctions de rappel est probablement le chemin le plus rapide vers l'enfer - ce style oblige de vérifier les erreurs partout, à gérer les imbrications de code gênantes et rend difficile la compréhension du flux du code.

🔗[**Plus d'infos :** guide pour async-await 1.0](https://github.com/yortus/asyncawait)

<br/><br/>

## ![✔] 3.12 Utiliser les expressions de fonction fléchée (=>)

**TL;PL :** Bien qu'il soit recommandé d'utiliser async-wait et d'éviter les paramètres de fonction lorsque vous traitez avec des API plus anciennes qui acceptent des promesses ou des rappels - les fonctions fléchées rendent la structure de code plus compacte et gardent le contexte lexical de la fonction racine (c'est-à-dire `this`).

**Autrement :** Un code plus long (dans les fonctions ES5) est plus sujet aux bogues et lourd à lire.

🔗 [**Plus d'infos : il est temps d'adopter les fonctions fléchées**](https://medium.com/javascript-scene/familiarity-bias-is-holding-you-back-its-time-to-embrace-arrow-functions-3d37e1a9bb75)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `4. Tests et pratiques générales de qualité`

## ![✔] 4.1 Au minimum, écrivez des tests API (pour chaque composant)

**TL;PL :** La plupart des projets n'ont tout simplement pas de test automatisé en raison de délais courts ou souvent le « projet de test » est devenu incontrôlable et a été abandonné. Pour cette raison, priorisez et commencez par les tests d'API, qui est le moyen le plus simple d'écrire et qui offre plus de couverture que les tests unitaires (vous pouvez même créer des tests d'API sans code à l'aide d'outils comme [Postman](https://www.getpostman.com/)). Par la suite, si vous avez plus de ressources et de temps, continuez avec des types de tests avancés tels que les tests unitaires, les tests DB (base de données), les tests de performances, etc.

**Autrement :** Vous pouvez passer de longues journées à écrire des tests unitaires pour découvrir que vous n'avez qu'une couverture système de 20%

<br/><br/>

## ![✔] 4.2 Incluez 3 parties dans chaque nom de test

**TL;PL :** Donnez un nom de test éloquent selon son niveau d'exigences pour qu'il soit compréhensible par les ingénieurs et développeurs de l'assurance qualité qui ne sont pas familiers avec les codes internes. Indiquez dans le nom du test ce qui est testé (élément du test), dans quelles circonstances et quel est le résultat attendu.

**Autrement :** Un déploiement vient d'échouer, un test nommé « Ajoute un produit » a échoué. Cela vous indique-t-il exactement ce qui ne fonctionne pas correctement ?

🔗[**Plus d'infos : incluez 3 parties dans chaque nom de test**](/sections/testingandquality/3-parts-in-name.french.md)

<br/><br/>

## ![✔] 4.3 Structurez vos tests avec le format AAA

**TL;PL :** Structurez vos tests avec 3 sections bien séparées : Arrange, Act & Assert (AAA). La première partie comprend la configuration du test, puis l'exécution de l'unité testée et enfin la phase d'assertion (vérification). Le respect de cette structure garantit que le lecteur n'utilise pas de le CPU de son cerveau pour comprendre le plan de test.

**Autrement :** Non seulement vous passez de longues heures par jour à comprendre le code principal, mais maintenant, ce qui aurait dû être la partie la plus simple de la journée (les tests) accroche votre cerveau

🔗[**Plus d'infos : structurez vos tests avec le format AAA**](/sections/testingandquality/aaa.french.md)

<br/><br/>

## ![✔] 4.4 Détectez les problèmes de code avec un linter

**TL;PL :** Utilisez un linter de code pour vérifier la qualité et détecter les antipatterns au plus tôt. Exécutez-le avant les tests et ajoutez-le en tant que git-hook de pré-commit pour diminuer le temps nécessaire pour examiner et corriger tout problème. Vérifiez également la [section 3](#3-style-du-code) sur les pratiques de style de code.

**Autrement :** Vous pouvez laisser passer un code antipatterns et éventuellement vulnérable sur votre environnement de production.

<br/><br/>

## ![✔] 4.5 Évitez les tests globaux, ajoutez des données pour chaque test

**TL;PL :** Pour éviter le chevauchement de test et expliquer facilement le déroulement du test, chaque test doit ajouter et agir sur son propre ensemble d'enregistrement de la base de données. Chaque fois qu'un test a besoin de récupérer ou de présumer l'existence de certaines données de la BD - il doit explicitement ajouter ces données et éviter de modifier tout autre enregistrement.

****Autrement :** Considérez un scénario où le déploiement est interrompu à cause de l'échec des tests, l'équipe va maintenant passer un temps d'investigation précieux qui se terminera par une triste conclusion : le système fonctionne bien, les tests interfèrent cependant les uns avec les autres et interrompent la construction.

🔗[**Plus d'infos : évitez les tests globaux, ajoutez des données pour chaque test**](/sections/testingandquality/avoid-global-test-fixture.french.md)

<br/><br/>

## ![✔] 4.6 Inspectez en permanence les dépendances vulnérables

**TL;PL :** Même les dépendances les plus réputées telles qu'Express ont des vulnérabilités connues. Cela peut être facilement apprivoisé à l'aide d'outils communautaires et commerciaux tels que 🔗 [npm audit](https://docs.npmjs.com/cli/audit) et 🔗 [snyk.io](https://snyk.io) qui peuvent être appelé depuis votre CI pour chaque construction.

**Autrement :** Garder votre code propre contre les vulnérabilités sans outils dédiés nécessitera de suivre en permanence les publications en ligne sur les nouvelles menaces. C'est assez fastidieux.

<br/><br/>

## ![✔] 4.7 Étiquetez vos tests

**TL;PL :** Différents tests doivent s'exécuter selon différents scénarios : test d'intégrité, sans IO, les tests doivent s'exécuter lorsqu'un développeur enregistre ou commit un fichier, les tests complets de bout en bout s'exécutent généralement lorsqu'une nouvelle « pull request » est soumise, etc. Cela peut être réalisé en étiquetant les tests avec des mots clés comme #IO #api #integrite afin que vous puissiez utiliser votre harnais de test et invoquer le sous-ensemble souhaité. Par exemple, voici comment vous invoqueriez uniquement le groupe de test d'intégrité avec [Mocha](https://mochajs.org/) : mocha --grep 'sanity'

**Autrement :** Exécutez tous les tests, y compris les tests qui effectuent des dizaines de requêtes sur la base de données, chaque fois qu'un développeur apporte un petit changement, cela peut être extrêmement lent et souvent les développeurs s'abstiennent de faire des tests.

<br/><br/>

## ![✔] 4.8 Vérifiez votre couverture de test, cela aide à identifier les mauvaises conception de test

**TL;PL :** Les outils de couverture de code comme [Istanbul](https://github.com/istanbuljs/istanbuljs)/[NYC](https://github.com/istanbuljs/nyc) sont parfaits pour 3 raisons : ils sont gratuits (aucun effort n'est nécessaire pour bénéficier de ces rapports), ils aident à identifier une diminution de la couverture des tests, et enfin et surtout, ils mettent en évidence les incompatibilités de test : en regardant les rapports de couverture de code en couleur, vous remarquerez peut-être, par exemple, des zones de code qui ne sont jamais testées comme les clauses catch (ce qui signifie que les tests invoquent uniquement les chemins positifs et non le comportement de l'application en cas d'erreur). Configurez-les pour faire échouer les constructions si la couverture tombe sous un certain seuil.

**Autrement :** Il n'y aura aucune mesure automatisée vous indiquant quand une grande partie de votre code n'est pas couverte par les tests

<br/><br/>

## ![✔] 4.9 Inspectez les paquets obsolètes

**TL;PL :** Utilisez votre outil préféré (par exemple, `npm outdated` ou [npm-check-updates](https://www.npmjs.com/package/npm-check-updates)) pour détecter les paquets installés qui sont obsolètes, injectez cette vérification dans votre CI et même faites échouer une construction dans un scénario critique. Par exemple, un scénario critique peut se produire lorsqu'un paquet installé a 5 patchs de retard (par exemple, la version locale est 1.3.1 et la version du référentiel est 1.3.8) ou quand il est marqué comme obsolète par son auteur - stoppez la construction et empêchez le déploiement de cette version.

**Autrement :** Votre production exécutera des paquets qui ont été explicitement étiquetés par leur auteur comme risqués.

<br/><br/>

## ![✔] 4.10 Utilisez pour les tests e2e un environnement proche de la production

**TL;PL :** Les tests de bout en bout (e2e) qui comprennent l'utilisation de données en direct sont les maillons les plus faibles du processus du CI car ils dépendent de plusieurs services complexes comme la base de données. Utilisez un environnement de test continue aussi proche que possible de votre production actuelle. (Un oubli pour continue ici. A en juger par la clause **Autrement**, cela devrait mentionner docker-compose)

**Autrement :** Sans docker-compose, les équipes doivent maintenir une base de données de test pour chaque environnement de test, y compris les machines des développeurs, garder toutes ces bases de données synchronisées afin que les résultats des tests ne varient pas d'un environnement à l'autre.

<br/><br/>

## ![✔] 4.11 Refactorisez régulièrement à l'aide d'outils d'analyse statique

**TL;PL :** L'utilisation d'outils d'analyse statique vous aide en donnant des moyens concrets d'améliorer la qualité du code et permet de maintenir votre code. Vous pouvez ajouter des outils d'analyse statique à votre CI pour échouer lorsqu'il trouve du code incorrect. Ses principaux arguments de vente par rapport au contrôle ordinaire de code sont la capacité d'inspecter la qualité dans le contexte de plusieurs fichiers (par exemple, détecter les doublons), d'effectuer une analyse avancée (par exemple la complexité du code) et de suivre l'historique et la progression des problèmes de code. Deux exemples d'outils que vous pouvez utiliser sont [Sonarqube](https://www.sonarqube.org/) (+ de 2 600 [étoiles](https://github.com/SonarSource/sonarqube)) et [Code Climate](https://codeclimate.com/) (+ de 1 500 [étoiles](https://github.com/codeclimate/codeclimate)).

**Autrement :** Avec une mauvaise qualité de code, les bogues et les performances seront toujours un problème qu'aucune nouvelle bibliothèque brillante ou fonctionnalité de pointe ne peut résoudre.

🔗[**Plus d'infos: refactorisation !**](/sections/testingandquality/refactoring.french.md)

<br/><br/>

## ![✔] 4.12 Choisissez soigneusement votre plateforme CI (Jenkins vs CircleCI vs Travis vs Rest of the world)

**TL;PL :** Votre plateforme d'intégration continue (CICD) hébergera tous les outils de qualité (par exemple test, lint), elle devrait donc être accompagnée d'un écosystème dynamique de plugins. [Jenkins](https://jenkins.io/) était utilisé par défaut pour de nombreux projets car il a la plus grande communauté avec une plateforme très puissante au prix d'une configuration complexe qui nécessite une courbe d'apprentissage abrupte. De nos jours, il est devenu beaucoup plus facile de mettre en place une solution CI en utilisant des outils SaaS comme [CircleCI](https://circleci.com) et d'autres. Ces outils permettent de créer un pipeline de CI flexible sans avoir à gérer l'ensemble de l'infrastructure. Finalement, c'est un compromis entre robustesse et rapidité - choisissez votre camp avec soin.

**Autrement :** Le choix d'un fournisseur de niche peut vous bloquer une fois que vous aurez besoin d'une personnalisation avancée. En revanche, faire appel à Jenkins pourrait vous faire perdre un temps précieux à la mise en place de l'infrastructure.

🔗[**Plus d'infos : choisissez soigneusement votre plateforme CI**](/sections/testingandquality/citools.french.md)

## ![✔] 4.13 Testez vos middlewares de manière isolée

**TL;PL :** Lorsqu'un middleware contient une logique immense qui couvre de nombreuses requêtes, cela vaut la peine de le tester de manière isolée sans réveiller l'ensemble du framework du web. Cela peut être facilement réalisé en espionnant les objets {req, res, next}.

**Autrement :** Un bogue dans le middleware Express === un bogue dans toutes ou la plupart des requêtes

🔗 [**Plus d'infos : testez vos middlewares de manière isolée**](/sections/testingandquality/test-middlewares.french.md)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `5. Pratiques de mise en production`

## ![✔] 5.1. Surveillance

**TL;PL :** La surveillance est un jeu qui consiste à découvrir les problèmes avant que les clients ne les trouvent - il est évident qu'il faut accorder une importance sans précédent à cette question. Le marché est submergé d'offres, pensez donc à commencer par définir les mesures de base que vous devez suivre (mes suggestions à l'intérieur), puis passez en revue les fonctionnalités supplémentaires et choisissez la solution qui coche toutes les cases. Cliquez sur « l'essentiel » ci-dessous pour un aperçu des solutions.

**Autrement :** Échec === clients déçus. C'est simple.

🔗 [**Plus d'infos : surveillance !**](/sections/production/monitoring.french.md)

<br/><br/>

## ![✔] 5.2. Augmentez la clarté à l'aide de la journalisation intelligente

**TL;PL :** Les journaux peuvent être un stupide inventaire de relevés de débogage ou le facilitateur d'un magnifique tableau de bord qui raconte l'histoire de votre application. Prévoyez votre plateforme de journalisation dès le premier jour : comment les logs sont collectés, stockés et analysés pour s'assurer que les informations souhaitées (par exemple le taux d'erreur, le suivi d'une transaction complète via des services et des serveurs, etc.) peuvent réellement être exploitées.

**Autrement :** Vous vous retrouvez avec une boîte noire qui est difficile à analyser, puis vous commencez à réécrire toutes les instructions de journalisation pour ajouter des informations supplémentaires.

🔗 [**Plus d'infos : augmentez la clarté à l'aide de la journalisation intelligente**](/sections/production/smartlogging.french.md)

<br/><br/>

## ![✔] 5.3. Déléguez tout ce qui est possible (par exemple gzip, SSL) à un proxy inverse

**TL;PL :** Node est terriblement mauvais pour faire des tâches gourmandes en CPU comme la compression avec gzip, terminaison SSL, etc. Vous devriez utiliser à la place des services de middleware « réels » comme nginx, HAproxy ou des services de fournisseurs du cloud.

**Autrement :** Votre pauvre processus restera occupé à faire des tâches d'infrastructure au lieu de s'occuper du cœur de votre application et les performances se dégraderont en conséquence.

🔗 [**Plus d'infos : déléguez tout ce qui est possible (par exemple gzip, SSL) à un proxy inverse**](/sections/production/delegatetoproxy.french.md)

<br/><br/>

## ![✔] 5.4. Verrouillez les dépendances

**TL;PL :** Votre code doit être identique dans tous les environnements, mais étonnamment npm laisse les dépendances fluctuer entre les environnements par défaut - lorsque vous installez des paquets dans différents environnements, il essaie de récupérer la dernière version du patch des paquets. Surmontez cela en utilisant les fichiers de configuration de npm, .npmrc, qui indiquent à chaque environnement de sauvegarder la version exacte (et non la dernière) de chaque paquet. Alternativement, pour un contrôle plus fin, utilisez `npm shrinkwrap`. \*Mise à jour : à partir de NPM5, les dépendances sont verrouillées par défaut. Le nouveau gestionnaire de paquets en place, Yarn, nous a aussi fourni une couverture par défaut.

**Autrement :** Le service qualité testera le code de manière approfondie et approuvera une version qui se comportera différemment en production. Pire encore, différents serveurs dans le même cluster de production peuvent exécuter un code différent.

🔗 [**Plus d'infos : verrouillez les dépendances**](/sections/production/lockdependencies.french.md)

<br/><br/>

## ![✔] 5.5. Protégez la disponibilité du processus avec des bons outils

**TL;PL :** Le processus doit continuer et être redémarré en cas d'échec. Pour des scénarios simples, des outils de gestion de processus comme PM2 peuvent suffire, mais dans le monde « dockérizé » d'aujourd'hui, les outils de gestion de cluster doivent également être pris en compte.

**Autrement :** L'exécution simultanée de dizaines d'instances sans stratégie claire et trop d'outils (gestion de cluster, docker, PM2) pourrait conduire au chaos du DevOps.

🔗 [**Plus d'infos : protégez la disponibilité du processus avec des bons outils**](/sections/production/guardprocess.french.md)

<br/><br/>

## ![✔] 5.6. Utilisez tous les cœurs du CPU

**TL;PL :** Dans sa forme de base, une application Node fonctionne sur un seul cœur de CPU alors que tous les autres sont laissés au repos. Il est de votre devoir de répliquer le processus Node et d'utiliser tous les CPU - Pour les petites et moyennes applications, vous pouvez utiliser Node Cluster ou PM2. Pour une application plus grande, pensez à répliquer le processus à l'aide d'un cluster Docker (par exemple K8S, ECS) ou des scripts de déploiement basés sur le système d'initialisation Linux (par exemple systemd).

**Autrement :** Votre application n'utilisera probablement que 25% de ses ressources disponibles (!) ou même moins. Notez qu'un serveur typique possède 4 cœurs CPU ou plus, le déploiement naïf de Node.js n'en utilise qu'un (même en utilisant des services PaaS comme AWS beanstalk !)

🔗 [**Plus d'infos : utilisez tous les cœurs du CPU**](/sections/production/utilizecpu.french.md)

<br/><br/>

## ![✔] 5.7. Créez un « point de terminaison de maintenance »

**TL;PL :** Exposez dans une API sécurisée un ensemble d'informations liées au système, comme l'utilisation de la mémoire et le REPL, etc. Bien qu'il soit fortement recommandé de s'appuyer sur des outils standard et éprouvés au combat, certaines informations et opérations précieuses sont plus faciles à utiliser à l'aide de code.

**Autrement :** Vous constaterez que vous effectuez de nombreuses « livraisons de diagnostics » - la livraison de code vers la production uniquement pour extraire des informations à des fins de diagnostic.

🔗 [**Plus d'infos : créez un « point de terminaison de maintenance »**](/sections/production/createmaintenanceendpoint.french.md)

<br/><br/>

## ![✔] 5.8. Découvrez les erreurs et les indisponibilités à l'aide des produits APM

**TL;PL :** Les produits de surveillance et de performance des applications (a.k.a APM) mesurent de manière proactive la base de code et l'API afin qu'ils puissent aller automatiquement au-delà de la surveillance traditionnelle et mesurer l'expérience utilisateur globale à travers les services et les tiers. Par exemple, certains produits APM peuvent mettre en évidence une transaction qui se charge trop lentement du côté des utilisateurs finaux tout en suggérant la cause principale.

**Autrement :** Vous pourriez consacrer beaucoup d'efforts à mesurer les performances et l'indisponibilité de l'API, vous ne saurez probablement jamais quelles sont vos parties de code les plus lentes dans le scénario du monde réel et comment celles-ci affectent l'expérience utilisateur.

🔗 [**Plus d'infos : découvrez les erreurs et les indisponibilités à l'aide des produits APM**](/sections/production/apmproducts.french.md)

<br/><br/>

## ![✔] 5.9. Préparez votre code pour la production

**TL;PL :** Codez en pensant à la solution définitive, planifiez la production dès le premier jour. Cela semble un peu vague, j'ai donc compilé quelques conseils de développement qui sont étroitement liés à la maintenance de la production (cliquez sur l'essentiel ci-dessous)

**Autrement :** Même le champion du monde Architecte/DevOps ne sauvera pas un système mal écrit.

🔗 [**Plus d'infos : préparez votre code pour la production**](/sections/production/productioncode.french.md)

<br/><br/>

## ![✔] 5.10. Mesurez et protégez l'utilisation de la mémoire

**TL;PL :** Node.js a des relations controversées avec la mémoire : le moteur v8 a de faibles limites sur l'utilisation de la mémoire (1.4GB) et il y a des moyens connus pour faire fuir la mémoire dans le code de Node - donc surveiller la mémoire du processus de Node est une chose indispensable. Dans les petites applications, vous pouvez mesurer la mémoire périodiquement en utilisant des commandes shell mais dans les applications de taille moyenne, vous pouvez envisager de faire de votre surveillance mémoire via un système de surveillance robuste.

**Autrement :** La mémoire de votre processus peut fuir une centaine de mégaoctets par jour, comme cela s'est produit chez [Walmart](https://www.joyent.com/blog/walmart-node-js-memory-leak)

🔗 [**Plus d'infos : mesurez et protégez l'utilisation de la mémoire**](/sections/production/measurememory.french.md)

<br/><br/>

## ![✔] 5.11. Retirez vos ressources frontend de Node

**TL;PL :** Servez le contenu du frontend en utilisant un middleware dédié (nginx, S3, CDN) parce que les performances de Node sont vraiment diminuées lors du traitement de nombreux fichiers statiques en raison de son modèle mono-processus.

**Autrement :** Votre unique processus de Node sera occupé à diffuser des centaines de fichiers html/images/angular/react au lieu d'allouer toutes ses ressources à la tâche pour laquelle il est conçu - fournir du contenu dynamique

🔗 [**Plus d'infos : retirez vos ressources frontend de Node**](/sections/production/frontendout.french.md)

<br/><br/>

## ![✔] 5.12. Soyez sans état, tuez vos serveurs presque tous les jours

**TL;PL :** Stockez tout type de données (par exemple, sessions utilisateur, cache, fichiers téléchargés) dans des stockages de données externes. Envisagez de « tuer » vos serveurs périodiquement ou d'utiliser une plateforme « sans serveur » (par exemple AWS Lambda) qui impose explicitement un comportement sans état.

**Autrement :** La défaillance d'un serveur particulier entraînera l'arrêt des applications au lieu de simplement tuer une machine défectueuse. De plus, l'élasticité de l'extensibilité sera plus difficile à obtenir en raison de la dépendance à un serveur spécifique.

🔗 [**Plus d'infos : soyez sans état, tuez vos serveurs presque tous les jours**](/sections/production/bestateless.french.md)

<br/><br/>

## ![✔] 5.13. Utilisez des outils qui détectent automatiquement les vulnérabilités

**TL;PL :** Même les dépendances les plus réputées comme Express ont des vulnérabilités connues (de temps en temps) qui peuvent mettre un système en danger. Cela peut être facilement maîtrisé en utilisant des outils communautaires et commerciaux qui vérifient constamment les vulnérabilités et avertissent (localement ou sur GitHub), certains peuvent même les corriger immédiatement.

**Autrement :** Si vous ne disposez pas d'outils dédiés pour protéger votre code contre les vulnérabilités, vous devrez suivre en permanence les publications en ligne sur les nouvelles menaces. C'est assez fastidieux.

🔗 [**Plus d'infos : Utilisez des outils qui détectent automatiquement les vulnérabilités**](/sections/production/detectvulnerabilities.french.md)

<br/><br/>

## ![✔] 5.14. Attribuez un id de transaction à chaque relevé du journal

**TL;PL :** Attribuez le même identifiant, transaction-id : {une valeur}, à chaque entrée du journal à l'intérieur d'une même requête. Ensuite, lors de l'inspection des erreurs dans les journaux, il est facile de conclure ce qui s'est passé avant et après. Malheureusement, cela n'est pas facile à réaliser dans Node en raison de sa nature asynchrone, consultez les exemples de code.

**Autrement :** L'examen d'un journal d'erreurs de production sans le contexte (ce qui s'est passé auparavant) rend le travail de réflexion beaucoup plus difficile et lent.

🔗 [**Plus d'infos : attribuez un ‘TransactionId’ à chaque relevé du journal**](/sections/production/assigntransactionid.french.md)

<br/><br/>

## ![✔] 5.15. Définissez `NODE_ENV=production`

**TL;PL :** Définissez la variable d'environnement `NODE_ENV` avec « production » ou « development » pour indiquer si les optimisations de production doivent être activées - de nombreux paquets npm déterminent l'environnement en cours et optimisent leur code pour la production.

**Autrement :** L'omission de cette simple propriété pourrait fortement dégrader les performances. Par exemple, lors de l'utilisation d'Express pour le rendu côté serveur, l'omission de `NODE_ENV` le rend trois fois plus lent !

🔗 [**Plus d'infos : définissez NODE_ENV=production**](/sections/production/setnodeenv.french.md)

<br/><br/>

## ![✔] 5.16. Concevez des déploiements automatisés, atomiques et sans interruption de service

**TL;PL :** Les études montrent que les équipes qui effectuent de nombreux déploiements réduisent la probabilité de graves problèmes en production. Les déploiements rapides et automatisés qui ne nécessitent pas d'étapes manuelles risquées ni d'interruptions de service améliorent considérablement le processus de déploiement. Vous devriez probablement y parvenir en utilisant Docker combiné à des outils de CI, car ils sont devenus la norme du secteur pour un déploiement optimisé.

**Autrement :** Déploiements longs -> arrêt de la production et erreur humaine -> équipe peu confiante dans la réalisation du déploiement -> moins de déploiements et de fonctionnalités.

<br/><br/>

## ![✔] 5.17. Utilisez une version LTS de Node.js

**TL;PL :** Assurez-vous d'utiliser une version LTS de Node.js pour recevoir les corrections de bogues critiques, les mises à jour de sécurité et les améliorations de performance

**Autrement :** Les bogues ou vulnérabilités récemment découverts pourraient être utilisés pour exploiter une application en production, et votre application pourrait devenir non supportée par divers modules et plus difficile à maintenir

🔗 [**Plus d'infos : Utilisez une version LTS de Node.js**](/sections/production/LTSrelease.french.md)

<br/><br/>

## ![✔] 5.18. Ne redirigez pas les journaux vers l'application

**TL;PL :** Les destinations des journaux ne devraient pas être codées en dur par les développeurs dans le code de l'application, mais devraient plutôt être définies par l'environnement d'exécution dans lequel l'application s'exécute. Les développeurs doivent écrire des journaux dans `stdout` en utilisant un utilitaire de journalisation et laisser l'environnement d'exécution (conteneur, serveur, etc.) diriger le flux `stdout` vers la destination appropriée (c'est-à-dire Splunk, Graylog, ElasticSearch, etc.).

**Autrement :** Acheminement des journaux de gestion des applications === difficile à dimensionner, perte de journaux, mauvaise séparation des informations.

🔗 [**Plus d'infos : redirection du journal**](/sections/production/logrouting.french.md)

<br/><br/>

## ![✔] 5.19. Installez vos paquets avec `npm ci`

**TL;PL :** Vous devez vous assurer que le code de production utilise la version exacte des paquets avec lesquels vous l'avez testé. Exécutez `npm ci` pour faire une installation propre de vos dépendances correspondant aux fichiers package.json et package-lock.json.

**Autrement :****** Le service qualité testera le code de manière approfondie et approuvera une version qui se comportera différemment en production. Pire encore, différents serveurs dans le même cluster de production peuvent exécuter un code différent.

🔗 [**Plus d'infos : utilisez npm ci**](/sections/production/installpackageswithnpmci.french.md)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `6. Bonnes pratiques de sécurité`

<div align="center">
<img src="https://img.shields.io/badge/OWASP%20Threats-Top%2010-green.svg" alt="54 items"/>
</div>

## ![✔] 6.1. Adoptez les règles de sécurité du linter

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20XSS%20-green.svg" alt=""/></a>

**TL;PL :** Utilisez des plugins de sécurité de linter tels que [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security) pour détecter les vulnérabilités et les problèmes de sécurité le plus tôt possible, de préférence lors de leur codage. Cela peut aider à détecter des vulnérabilités de sécurité comme l'utilisation de eval, l'invocation d'un processus enfant ou l'importation d'un module avec une chaîne littérale (par exemple, une entrée utilisateur). Cliquez sur « Plus d'infos » ci-dessous pour voir des exemples de codes qui seront pris en compte par un linter de sécurité.

**Autrement :** Ce qui aurait pu être une simple défaillance de sécurité pendant le développement devient un problème majeur en production. En outre, le projet peut ne pas suivre des pratiques de sécurité de code conformes, ce qui peut entraîner l'introduction de vulnérabilités ou la divulgation de secrets sensibles dans des dépôts distants.

🔗 [**Plus d'infos : régles du linter**](/sections/security/lintrules.french.md)

<br/><br/>

## ![✔] 6.2. Limitez les requêtes simultanées en utilisant un  middleware

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;PL :** Les attaques DOS sont très populaires et relativement faciles à mener. Mettre en œuvre la limitation de débit en utilisant un service externe tel que les équilibreurs de charge, les pare-feux, nginx, un paquet [rate-limiter-flexible](https://www.npmjs.com/package/rate-limiter-flexible), ou (pour les applications plus petites et moins critiques) un middleware de limitation de débit (par exemple, [express-rate-limit](https://www.npmjs.com/package/express-rate-limit)).

**Autrement :** Une application pourrait faire l'objet d'une attaque entraînant un déni de service, de ce fait, les utilisateurs réels obtiennent un service dégradé ou indisponible.

🔗 [**Plus d'infos : mettez en œuvre la limitation de débit**](/sections/security/limitrequests.french.md)

<br/><br/>

## ![✔] 6.3 Retirez les secrets des fichiers de configuration ou utiliser des paquets pour les crypter

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A3:Sensitive%20Data%20Exposure%20-green.svg" alt=""/></a>

**TL;PL :** Ne stockez jamais les secrets en clair dans les fichiers de configuration ou le code source. Utilisez plutôt des systèmes de gestion des secrets comme les produits Vault, Kubernetes/Docker Secrets ou des variables d'environnement. En dernier recours, les secrets stockés dans le contenu des sources doivent être cryptés et gérés (clés de roulement, expiration, audit, etc.). Utilisez des hooks de pre-commit/push pour éviter de commeittez des secrets accidentellement.

**Autrement :** Le contenu des sources, même pour les dépôts privés, peut être rendu public par erreur, et tous les secrets sont alors dévoilés. L'accès au contenu des sources pour une partie externe donnera par inadvertance accès à des systèmes connexes (bases de données, apis, services, etc.).

🔗 [**Plus d'infos : gestion des secrets**](/sections/security/secretmanagement.french.md)

<br/><br/>

## ![✔] 6.4. Évitez les vulnérabilités d'injection de query avec les bibliothèques ORM/ODM

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;PL :** Pour éviter l'injection SQL/NoSQL et d'autres attaques malveillantes, utilisez toujours un ORM/ODM ou une bibliothèque de base de données qui échappe les données ou prend en charge les queries paramétrées nommées ou indexées et se charge de valider les entrées de l'utilisateur pour les types attendus. N'utilisez jamais simplement des chaînes de template JavaScript ou une concaténation de chaînes pour injecter des valeurs dans les queries, car cela ouvre votre application à un large éventail de vulnérabilités. Toutes les bibliothèques d'accès aux données Node.js réputées (par exemple [Sequelize](https://github.com/sequelize/sequelize), [Knex](https://github.com/tgriesser/knex), [mongoose](https://github.com/Automattic/mongoose)) disposent d'une protection intégrée contre les attaques par injection.

**Autrement :** Une entrée utilisateur non validée ou non assainie pourrait conduire à l'injection de l'opérateur lorsqu'on travaille avec MongoDB pour NoSQL, et le fait de ne pas utiliser un système d'assainissement approprié ou un ORM permettra facilement des attaques par injection SQL, créant une énorme vulnérabilité.

🔗 [**Plus d'infos : Prévention de l'injection de queries à l'aide des bibliothèques ORM/ODM**](/sections/security/ormodmusage.french.md)

<br/><br/>

## ![✔] 6.5. Collection des meilleures pratiques génériques en matière de sécurité

**TL;PL :** Il s'agit d'une collection de conseils de sécurité qui n'est pas directement liée à Node.js, l'implémentation de Node n'est pas très différente de toute autre langage. Cliquez pour en savoir plus.

🔗 [**Plus d'infos : les meilleures pratiques communes en matière de sécurité**](/sections/security/commonsecuritybestpractices.french.md)

<br/><br/>

## ![✔] 6.6. Adaptez les entêtes de réponse HTTP pour une sécurité accrue

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;PL :** Votre application doit utiliser des entêtes sécurisés pour empêcher les attaquants d'utiliser des attaques courantes comme le cross-site scripting (XSS), le clickjacking et d'autres attaques malveillantes. Celles-ci peuvent être facilement configurées à l'aide de modules tels que [helmet](https://www.npmjs.com/package/helmet).

**Autrement :** Les attaquants pourraient lancer des attaques directes sur les utilisateurs de votre application, ce qui entraînerait d'énormes vulnérabilités de sécurité.

🔗 [**Plus d'infos : utilisation d'entêtes sécurisés dans votre application**](/sections/security/secureheaders.french.md)

<br/><br/>

## ![✔] 6.7. Inspectez constamment et automatiquement les dépendances vulnérables

<a href="https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Known%20Vulnerabilities%20-green.svg" alt=""/></a>

**TL;PL :** Avec l'écosystème npm, il est courant d'avoir de nombreuses dépendances pour un projet. Les dépendances doivent toujours être contrôlées lorsque de nouvelles vulnérabilités sont détectées. Utilisez des outils comme [npm audit](https://docs.npmjs.com/cli/audit) ou [snyk](https://snyk.io/) pour suivre, surveiller et corriger les dépendances vulnérables. Intégrez ces outils à votre configuration CI afin de détecter une dépendance vulnérable avant qu'elle ne passe en production.

**Autrement :** Un attaquant pourrait détecter votre framework Web et attaquer toutes ses vulnérabilités connues.

🔗 [**Plus d'infos : sécurité des dépendances**](/sections/security/dependencysecurity.french.md)

<br/><br/>

## ![✔] 6.8. Protégez les mots de passe/secrets des utilisateurs à l'aide de bcrypt ou scrypt

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;PL :** Les mots de passe ou les secrets (par exemple les clés API) doivent être stockés en utilisant une fonction de hachage sécurisée + un salt comme `bcrypt`,` scrypt`, ou dans le pire des cas `pbkdf2`.

**Autrement :** Les mots de passe et les secrets qui sont stockés sans utiliser de fonction sécurisée sont vulnérables au brute-force et aux attaques de dictionnaire qui mèneront à leur divulgation.

🔗 [**Plus d'infos : mots de passe utilisateur**](/sections/security/userpasswords.french.md)

<br/><br/>

## ![✔] 6.9. Échappez les sorties HTML, JS et CSS

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a>

**TL;PL :** Les données non approuvées qui sont envoyées au navigateur peuvent être exécutées au lieu d'être simplement affichées, ce qui est communément appelé une attaque de script intersite (XSS). Atténuez cela en utilisant des bibliothèques dédiées qui marquent explicitement les données comme du contenu pur qui ne devrait jamais être exécuté (par exemple encodage, échappement).

**Autrement :** Un attaquant pourrait stocker du code JavaScript malveillant dans votre base de données qui sera ensuite envoyé tel quel aux pauvres clients.

🔗 [**Plus d'infos : échappez la sortie**](/sections/security/escape-output.french.md)

<br/><br/>

## ![✔] 6.10. Validez les schémas JSON entrants

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7: XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a>

**TL;PL :** Validez la charge utile du corps des requêtes entrantes et assurez-vous qu'elle répond aux exigences, sinon échouez rapidement. Pour éviter un codage de validation fastidieux pour chaque route, vous pouvez utiliser des schémas de validation légers basés sur JSON tels que [jsonschema](https://www.npmjs.com/package/jsonschema) ou [joi](https://www.npmjs.com/package/joi).

**Autrement :** Votre générosité et votre approche permissive augmentent considérablement la surface d'attaque et encouragent l'attaquant à essayer de nombreuses entrées jusqu'à ce qu'il trouve une combinaison pour planter l'application.

🔗 [**Plus d'infos : validez les schémas JSON entrants**](/sections/security/validation.french.md)

<br/><br/>

## ![✔] 6.11. Prenez en charge le blocage des JWT

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;PL :** Lorsque vous utilisez des jetons Web JSON (par exemple, avec [Passport.js](https://github.com/jaredhanson/passport)), il n'existe par défaut aucun mécanisme permettant de révoquer l'accès aux jetons émis. Lorsque vous découvrez une activité malveillante de la part d'un utilisateur, il n'y a aucun moyen de l'empêcher d'accéder au système tant qu'il détient un jeton valide. Pour atténuer ce problème, il est possible de mettre en place une liste de blocage des jetons non fiables qui sont validés à chaque requête.

**Autrement :** Les jetons expirés ou égarés pourraient être utilisés de manière malveillante par un tiers pour accéder à une application et se faire passer pour le propriétaire du jeton.

🔗 [**Plus d'infos : bloquez les jetons Web JSON**](/sections/security/expirejwt.french.md)

<br/><br/>

## ![✔] 6.12. Empêchez les attaques brute-force perpétrées contre les autorisations

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;PL :** Une technique simple et puissante consiste à limiter les tentatives d'autorisation en utilisant deux mesures :

1. Le premier est le nombre de tentatives infructueuses consécutives par le même ID/nom unique d'utilisateur et la même adresse IP.
2. Le deuxième est le nombre de tentatives infructueuses à partir d'une adresse IP sur une longue période de temps. Par exemple, bloquez une adresse IP si elle fait 100 tentatives infructueuses en une journée.

**Autrement :** Un attaquant peut lancer des tentatives de mot de passe automatisées illimitées pour accéder à des comptes privilégiés sur une application.

🔗 [**Plus d'infos : restrictions du nombre de connexions**](/sections/security/login-rate-limit.french.md)

<br/><br/>

## ![✔] 6.13. Exécutez Node.js en tant qu'utilisateur non root

<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A5:Broken%20Access%20Access%20Control-green.svg" alt=""/></a>

**TL;PL :** Il existe un scénario répandu dans lequel Node.js s'exécute en tant qu'utilisateur root avec des autorisations illimitées. Par exemple, c'est le comportement par défaut dans les conteneurs Docker. Il est recommandé de créer un utilisateur non root et de l'intégrer dans l'image du Docker (exemples donnés ci-dessous) ou d'exécuter le processus sous le nom de cet utilisateur en invoquant le conteneur avec l'indicateur « -u nom_utilisateur ».

**Autrement :** Un attaquant qui réussit à exécuter un script sur le serveur obtient un pouvoir illimité sur la machine locale (par exemple, modifier iptable et réacheminer le trafic vers son serveur).

🔗 [**Plus d'infos : exécutez Node.js en tant qu'utilisateur non root**](/sections/security/non-root-user.french.md)

<br/><br/>

## ![✔] 6.14. Limitez la capacité des données utiles en utilisant un proxy inverse ou un middleware

<a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;PL :** Plus les données utiles du corps sont grandes, plus votre processus unique travaille intensément pour les traiter. C'est l'occasion pour les attaquants de mettre les serveurs à genoux sans qu'il y ait beaucoup de requêtes (attaques DOS/DDOS). Atténuez cela en limitant la taille du corps des requêtes entrantes en périphérie (par exemple, pare-feu, ELB) ou en configurant [express body parser](https://github.com/expressjs/body-parser) pour accepter uniquement les données utiles de petite taille.

**Autrement :** Votre application devra traiter des requêtes importantes, incapable de traiter les autres travaux importants qu'elle doit accomplir, ce qui aura des conséquences sur les performances et la vulnérabilité aux attaques DOS.
Votre application devra faire face à de grosses requêtes, incapable de traiter les autres travaux importants qu'elle doit accomplir, entraînant des implications en termes de performances et une vulnérabilité face aux attaques DOS.
🔗 [**Plus d'infos : limitez la capacité des données utiles**](/sections/security/requestpayloadsizelimit.french.md)

<br/><br/>

## ![✔] 6.15. Évitez les instruction eval de JavaScript

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;PL :** `eval` est diabolique car il permet d'exécuter du code JavaScript personnalisé pendant l'exécution. Il ne s'agit pas seulement d'un problème de performances, mais également d'un problème de sécurité important en raison du code JavaScript malveillant qui peut provenir de la saisie de l'utilisateur. Une autre fonctionnalité du langage à éviter est le constructeur `new Function`. Il ne faut pas non plus passer de code JavaScript dynamique à `setTimeout` et `setInterval`.

**Autrement :** Un code JavaScript malveillant trouve un moyen d'accéder au texte passé dans `eval` ou d'autres fonctions d'évaluation en temps réel du langage JavaScript, et obtiendra un accès complet aux autorisations JavaScript sur la page. Cette vulnérabilité se manifeste souvent par une attaque XSS.

🔗 [**Plus d'infos : évitez les instruction eval de JavaScript**](/sections/security/avoideval.french.md)

<br/><br/>

## ![✔] 6.16. Empêchez une mauvaise RegEx de surcharger l'exécution de votre unique processus

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;PL :** Les expressions régulières, bien qu'elles soient pratiques, constituent une réelle menace pour les applications JavaScript en général et la plateforme Node.js en particulier. La saisie d'un texte par l'utilisateur peut nécessiter un nombre exceptionnel de cycles du CPU pour être traitée. Le traitement RegEx pourrait être inefficace, à tel point qu'une seule requête qui valide 10 mots peut bloquer toute la boucle d'événement pendant 6 secondes et mettre l'unité centrale en 🔥. Pour cette raison, préférez les paquets de validation tiers comme [validator.js](https://github.com/chriso/validator.js) au lieu d'écrire vos propres modèles de regex, ou utilisez [safe-regex](https://github.com/substack/safe-regex) pour détecter les modèles de regex vulnérables.

**Autrement :** Des RegEx mal écrites pourraient être susceptibles de faire l'objet d'attaques DoS par expression régulière qui bloqueraient complètement la boucle de l'événement. Par exemple, le populaire paquet "moment" a été déclaré vulnérable par une utilisation malveillante de RegEx en novembre 2017.

🔗 [**Plus d'infos : empêcher une RegEx malveillante**](/sections/security/regex.french.md)

<br/><br/>

## ![✔] 6.17. Évitez le chargement de modules à l'aide d'une variable

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;PL :** Évitez de demander/importer un autre fichier dont le chemin d'accès a été donné en paramètre, car on peut craindre qu'il provienne d'une saisie de l'utilisateur. Cette règle peut être étendue pour l'accès aux fichiers en général (c'est-à-dire `fs.readFile()`) ou à d'autres ressources sensibles avec des variables dynamiques provenant d'une entrée utilisateur. Le linter [Eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security) peut détecter de tels modèles et avertir suffisamment tôt.

**Autrement :** Une entrée utilisateur malveillante pourrait trouver son chemin vers un paramètre qui est utilisé pour exiger des fichiers falsifiés, par exemple, un fichier précédemment téléchargé sur le système de fichiers, ou accéder à des fichiers système déjà existants.

🔗 [**Plus d'infos : chargement sécurisé des modules**](/sections/security/safemoduleloading.french.md)

<br/><br/>

## ![✔] 6.18. Exécutez un code dangereux dans un bac à sable

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;PL :** Lorsqu'il s'agit d'exécuter du code externe donné en cours d'exécution (par exemple un plugin), utilisez un environnement d'exécution de type « bac à  sable » (« sandbox ») qui isole et protège le code principal contre le plugin. Ceci peut être réalisé en utilisant un processus dédié (par exemple `cluster.fork()`), un environnement sans serveur ou des paquets npm dédiés qui agissent comme un bac à sable.

**Autrement :** Un plugin peut attaquer à travers une variété infinie d'options comme les boucles infinies, la surcharge de la mémoire et l'accès aux variables sensibles de l'environnement du processus.

🔗 [**Plus d'infos : exécutez un code dangereux dans un bac à sable**](/sections/security/sandbox.french.md)

<br/><br/>

## ![✔] 6.19. Soyez particulièrement vigilants lorsque vous travaillez avec des processus fils

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;PL :** Évitez d'utiliser des processus fils lorsque c'est possible et validez et assainissez les entrées pour atténuer les attaques par injection dans l'interface sytème si vous devez encore le faire. Préférez l'utilisation de `child_process.execFile` qui, par définition, n'exécutera qu'une seule commande avec un ensemble d'attributs et ne permettra pas l'expansion des paramètres de l'interface système.

**Autrement :** L'utilisation naïve de processus fils pourrait entraîner l'exécution de commandes à distance ou des attaques par injection dans l'interface système en raison d'une entrée utilisateur malveillante transmise à une commande système non assainie.

🔗 [**Plus d'infos : soyez prudents lorsque vous travaillez avec des processus fils**](/sections/security/childprocesses.french.md)

<br/><br/>

## ![✔] 6.20. Masquez les détails des erreurs aux clients

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;PL :** Un gestionnaire d'erreur express intégré masque les détails de l'erreur par défaut. Cependant, il y a de grandes chances que vous implémentiez votre propre logique de traitement des erreurs avec des objets d'erreur personnalisés (considérés par beaucoup comme une meilleure pratique). Si vous le faites, veillez à ne pas renvoyer l'objet Error complet au client, qui pourrait contenir certains détails sensibles de l'application.

**Autrement :** Des détails sensibles de l'application pourraient être divulgués à partir d'informations trouvées dans une trace de pile, tels que les chemins d'accès aux fichiers du serveur, les modules tiers utilisés et d'autres flux de travail internes de l'application qui pourraient être exploités par un attaquant.

🔗 [**Plus d'infos : masquez les détails des erreurs au client**](/sections/security/hideerrors.french.md)

<br/><br/>

## ![✔] 6.21. Configurez 2FA pour npm ou Yarn

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;PL :** Toute étape de la chaîne de développement doit être protégée par une authentification multi-facteurs (MFA). npm/Yarn sont une belle opportunité pour les attaquants qui peuvent mettre la main sur le mot de passe d'un développeur. En utilisant les identifiants des développeurs, les attaquants peuvent injecter du code malveillant dans des bibliothèques qui sont largement installées dans les projets et les services. Peut-être même sur le web s'ils sont publiés publiquement. L'activation de l'authentification à deux facteurs dans npm ne laisse pratiquement aucune chance aux attaquants de modifier le code de votre paquet.

**Autrement :** [Avez-vous entendu parler du développeur d'eslint dont le mot de passe a été piraté ?](https://medium.com/@oprearocks/eslint-backdoor-what-it-is-and-how-to-fix-the-issue-221f58f1a8c8)

<br/><br/>

## ![✔] 6.22. Modifiez les paramètres du middleware de session

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;PL :** Chaque framework et technologie web a ses faiblesses connues - dire à un attaquant quel framework web nous utilisons est d'une grande aide pour lui. L'utilisation des paramètres par défaut des middlewares de session peut exposer votre application à des attaques de détournement sur un module ou un framework spécifique, de la même manière que l'entête « X-Powered-By ». Essayez de cacher tout ce qui identifie et révèle votre pile technique (par exemple, Node.js, express).

**Autrement :** Les cookies peuvent être envoyés via des connexions non sécurisées, et un attaquant peut utiliser l'identification de session pour identifier le framework sous-jacent de l'application web, ainsi que les vulnérabilités spécifiques aux modules.

🔗 [**Plus d'infos : cookie et sécurité des sessions**](/sections/security/sessions.french.md)

<br/><br/>

## ![✔] 6.23. Évitez les attaques DOS en définissant explicitement le moment où un processus doit s'interrompre

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;PL :** Le processus de Node plante lorsque les erreurs ne sont pas gérées. De nombreuses bonnes pratiques recommandent même de quitter Node même si une erreur a été détectée et traitée. Express, par exemple, plante sur toutes les erreurs asynchrones - à moins que vous n'ayez inclus une clause catch dans les routes. Cela ouvre un point d'attaque très intéressant pour les attaquants qui reconnaissent quelle entrée fait planter le processus et envoient la même requête de manière répétée. Il n'y a pas de remède immédiat à ce problème, mais quelques techniques peuvent atténuer la menace : alertez avec une gravité critique chaque fois qu'un processus se bloque à cause d'une erreur non gérée, valider l'entrée et éviter de planter le processus à cause d'une entrée utilisateur non valide, englober toutes les routes d'une clause catch et envisager de ne pas planter lorsqu'une erreur a été commise dans une requête (par opposition à ce qui se passe globalement).

**Autrement :** Ce n'est qu'une intuition : étant donné le grand nombre d'applications Node.js, si nous essayons de faire passer un corps JSON vide à toutes les requêtes POST - une poignée d'applications plantera. À ce stade, il suffit de répéter l'envoi de la même requête pour faire tomber les applications avec facilité.

<br/><br/>

## ![✔] 6.24. Empêchez les redirections dangereuses

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;PL :** Les redirections qui ne valident pas les entrées de l'utilisateur peuvent permettre aux attaquants de lancer des attaques de phishing, de voler les identifiants de l'utilisateur et d'effectuer d'autres actions malveillantes.

**Autrement :** Si un attaquant découvre que vous ne validez pas les données externes fournies par l'utilisateur, il peut exploiter cette vulnérabilité en publiant des liens spécialement conçus sur des forums, des médias sociaux et d'autres lieux publics pour inciter les utilisateurs à cliquer.

🔗 [**Plus d'infos : empêchez les redirections dangereuses**](/sections/security/saferedirects.french.md)

<br/><br/>

## ![✔] 6.25. Évitez de publier des secrets dans le registre npm

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;PL :** Des précautions doivent être prises pour éviter le risque de publier accidentellement des secrets dans les registres publics de npm. Un fichier `.npmignore` peut être utilisé pour ignorer des fichiers ou des dossiers spécifiques, ou le tableau `files` dans `package.json` peut agir comme une liste d'autorisation.

**Autrement :** Les clés API, mots de passe ou autres secrets de votre projet sont susceptibles d'être utilisés abusivement par toute personne qui les découvre, ce qui peut entraîner des pertes financières, une usurpation d'identité et d'autres risques.

🔗 [**Plus d'infos : évitez de publier des secrets**](/sections/security/avoid_publishing_secrets.french.md)
<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `7. Brouillon : Bonnes pratiques de performance`

## Nos contributeurs travaillent sur cette section. [Voulez-vous nous rejoindre ?](https://github.com/goldbergyoni/nodebestpractices/issues/256)

<br/><br/>

## ![✔] 7.1. Ne bloquez pas la boucle d'événement

**TL;PL :** Évitez les tâches gourmandes en CPU car elles bloqueront la boucle d'événement principalement mono-thread, il faut les décharger vers un thread dédié, un processus ou même une technologie différente en fonction du contexte.

**Autrement :** Comme la boucle d'événements est bloquée, Node.js sera incapable de traiter d'autres requêtes, ce qui entraînera des retards pour les utilisateurs concurrents. **3000 utilisateurs attendent une réponse, le contenu est prêt à être servi, mais une seule requêtes bloque le serveur pour qu'il ne puisse pas renvoyer les résultats**.

🔗 [**Plus d'infos : ne bloquez pas la boucle d'événement**](/sections/performance/block-loop.french.md)

<br /><br /><br />

## ![✔] 7.2. Préférez les méthodes JS natives aux utilitaires comme Lodash

**TL;PL :** Il est souvent plus pénalisant d'utiliser des bibliothèques utilitaires telles que `lodash` et `underscore` plutôt que des méthodes natives car cela conduit à des dépendances inutiles et à des performances plus lentes.
Gardez à l'esprit qu'avec l'introduction du nouveau moteur V8 en parallèle des nouvelles normes ES, les méthodes natives ont été améliorées de telle manière qu'elles sont maintenant environ 50% plus performantes que les bibliothèques utilitaires.

**Autrement :** Vous devez maintenir des projets moins performants où vous auriez pu simplement utiliser ce qui était **déjà** disponible ou traiter quelques lignes supplémentaires en échange de quelques fichiers supplémentaires.

🔗 [**Plus d'infos : natif supérieur aux utilitaires**](/sections/performance/nativeoverutil.french.md)

<br/><br/><br/>

<p align="right"><a href="#table-des-matières">⬆ Retourner en haut de la page</a></p>

# `8. Docker Best Practices`

🏅 Many thanks to [Bret Fisher](https://github.com/BretFisher) from whom we learned many of the following practices

<br/><br/>

## ![✔] 8.1 Use multi-stage builds for leaner and more secure Docker images

**TL;DR:** Use multi-stage build to copy only necessary production artifacts. A lot of build-time dependencies and files are not needed for running your application. With multi-stage builds these resources can be used during build while the runtime environment contains only what's necessary. Multi-stage builds are an easy way to get rid of overweight and security threats.

**Otherwise:** Larger images will take longer to build and ship, build-only tools might contain vulnerabilities and secrets only meant for the build phase might be leaked.

### Example Dockerfile for multi-stage builds

```dockerfile
FROM node:14.4.0 AS build

COPY . .
RUN npm ci && npm run build

FROM node:slim-14.4.0

USER node
EXPOSE 8080

COPY --from=build /home/node/app/dist /home/node/app/package.json /home/node/app/package-lock.json ./
RUN npm ci --production

CMD [ "node", "dist/app.js" ]
```

🔗 [**Read More: Use multi-stage builds**](/sections/docker/multi_stage_builds.md)

<br /><br /><br />

## ![✔] 8.2. Bootstrap using `node` command, avoid `npm start`

**TL;DR:** use `CMD ['node','server.js']` to start your app, avoid using npm scripts which don't pass OS signals to the code. This prevents problems with child-processes, signal handling, graceful shutdown and having zombie processes.

**Otherwise:** When no signals are passed, your code will never be notified about shutdowns. Without this, it will lose its chance to close properly possibly losing current requests and/or data.

[**Read More: Bootstrap container using node command, avoid npm start**](/sections/docker/bootstrap-using-node.md)

<br /><br /><br />

## ![✔] 8.3. Let the Docker runtime handle replication and uptime

**TL;DR:** When using a Docker run time orchestrator (e.g., Kubernetes), invoke the Node.js process directly without intermediate process managers or custom code that replicate the process (e.g. PM2, Cluster module). The runtime platform has the highest amount of data and visibility for making placement decision - It knows best how many processes are needed, how to spread them and what to do in case of crashes

**Otherwise:** Container keeps crashing due to lack of resources will get restarted indefinitely by the process manager. Should Kubernetes be aware of that, it could relocate it to a different roomy instance

🔗 [**Read More: Let the Docker orchestrator restart and replicate processes**](/sections/docker/restart-and-replicate-processes.md)

<br/><br /><br />

## ![✔] 8.4. Use .dockerignore to prevent leaking secrets

**TL;DR**: Include a `.dockerignore` file that filters out common secret files and development artifacts. By doing so, you might prevent secrets from leaking into the image. As a bonus the build time will significantly decrease. Also, ensure not to copy all files recursively rather explicitly choose what should be copied to Docker

**Otherwise**: Common personal secret files like `.env`, `.aws` and `.npmrc` will be shared with anybody with access to the image (e.g. Docker repository)

🔗 [**Read More: Use .dockerignore**](/sections/docker/docker-ignore.md)

<br /><br /><br />

## ![✔] 8.5. Clean-up dependencies before production

**TL;DR:** Although Dev-Dependencies are sometimes needed during the build and test life-cycle, eventually the image that is shipped to production should be minimal and clean from development dependencies. Doing so guarantees that only necessary code is shipped and the amount of potential attacks (i.e. attack surface) is minimized. When using multi-stage build (see dedicated bullet) this can be achieved by installing all dependencies first and finally running `npm ci --production`

**Otherwise:** Many of the infamous npm security breaches were found within development packages (e.g. [eslint-scope](https://eslint.org/blog/2018/07/postmortem-for-malicious-package-publishes))

🔗 Read More: [Remove development dependencies](/sections/docker/install-for-production.md)

<br /><br /><br />

## ![✔] 8.6. Shutdown smartly and gracefully

**TL;DR:** Handle the process SIGTERM event and clean-up all existing connection and resources. This should be done while responding to ongoing requests. In Dockerized runtimes shutting down containers is not a rare event, rather a frequent occurrence that happen as part of routine work. Achieving this demands some thoughtful code to orchestrate several moving parts: The load balancer, keep-alive connections, the HTTP server and other resources

**Otherwise:** Dying immediately means not responding to thousands of disappointed users

🔗 [**Read More: Graceful shutdown**](/sections/docker/graceful-shutdown.md)

<br /><br /><br />

## ![✔] 8.7. Set memory limits using both Docker and v8

**TL;DR:** Always configure a memory limit using both Docker and the JavaScript runtime flags. The Docker limit is needed to make thoughtful container placement decision, the --v8's flag max-old-space is needed to kick off the GC on time and prevent under utilization of memory. Practically, set the v8's old space memory to be a just bit less than the container limit

**Otherwise:** The docker definition is needed to perform thoughtful scaling decision and prevent starving other citizens. Without also defining the v8's limits, it will under utilize the container resources - Without explicit instructions it crashes when utilizing ~50-60% of its host resources

🔗 [**Read More: Set memory limits using Docker only**](/sections/docker/memory-limit.md)

<br /><br /><br />

## ![✔] 8.8. Plan for efficient caching

**TL;DR:** Rebuilding a whole docker image from cache can be nearly instantaneous if done correctly. The less updated instructions should be at the top of your Dockerfile and the ones constantly changing (like app code) should be at the bottom.

**Otherwise:** Docker build will be very long and consume lot of resources even when making tiny changes

🔗 [**Read More: Leverage caching to reduce build times**](/sections/docker/use-cache-for-shorter-build-time.md)

<br /><br /><br />

## ![✔] 8.9. Use explicit image reference, avoid `latest` tag

**TL;DR:** Specify an explicit image digest or versioned label, never refer to `latest`. Developers are often led to believe that specifying the `latest` tag will provide them with the most recent image in the repository however this is not the case. Using a digest guarantees that every instance of the service is running exactly the same code.

In addition, referring to an image tag means that the base image is subject to change, as image tags cannot be relied upon for a deterministic install. Instead, if a deterministic install is expected, a SHA256 digest can be used to reference an exact image.

**Otherwise:** A new version of a base image could be deployed into production with breaking changes, causing unintended application behaviour.

🔗 [**Read More: Understand image tags and use the "latest" tag with caution**](/sections/docker/image-tags.md)

<br /><br /><br />

## ![✔] 8.10. Prefer smaller Docker base images

**TL;DR:** Large images lead to higher exposure to vulnerabilities and increased resource consumption. Using leaner Docker images, such as Slim and Alpine Linux variants, mitigates this issue.

**Otherwise:** Building, pushing, and pulling images will take longer, unknown attack vectors can be used by malicious actors and more resources are consumed.

🔗 [**Read More: Prefer smaller images**](/sections/docker/smaller_base_images.md)

<br /><br /><br />

## ![✔] 8.11. Clean-out build-time secrets, avoid secrets in args

**TL;DR:** Avoid secrets leaking from the Docker build environment. A Docker image is typically shared in multiple environment like CI and a registry that are not as sanitized as production. A typical example is an npm token which is usually passed to a dockerfile as argument. This token stays within the image long after it is needed and allows the attacker indefinite access to a private npm registry. This can be avoided by coping a secret file like `.npmrc` and then removing it using multi-stage build (beware, build history should be deleted as well) or by using Docker build-kit secret feature which leaves zero traces

**Otherwise:** Everyone with access to the CI and docker registry will also get access to some precious organization secrets as a bonus

🔗 [**Read More: Clean-out build-time secrets**](/sections/docker/avoid-build-time-secrets.md)

<br /><br /><br />

## ![✔] 8.12. Scan images for multi layers of vulnerabilities

**TL;DR:** Besides checking code dependencies vulnerabilities also scan the final image that is shipped to production. Docker image scanners check the code dependencies but also the OS binaries. This E2E security scan covers more ground and verifies that no bad guy injected bad things during the build. Consequently, it is recommended running this as the last step before deployment. There are a handful of free and commercial scanners that also provide CI/CD plugins

**Otherwise:** Your code might be entirely free from vulnerabilities. However it might still get hacked due to vulnerable version of OS-level binaries (e.g. OpenSSL, TarBall) that are commonly being used by applications

🔗 [**Read More: Generic Docker practices**](/sections/docker/scan-images.md)

<br /><br /><br />

## ![✔] 8.13 Clean NODE_MODULE cache

**TL;DR:** After installing dependencies in a container remove the local cache. It doesn't make any sense to duplicate the dependencies for faster future installs since there won't be any further installs - A Docker image is immutable. Using a single line of code tens of MB (typically 10-50% of the image size) are shaved off

**Otherwise:** The image that will get shipped to production will weigh 30% more due to files that will never get used

🔗 [**Read More: Clean NODE_MODULE cache**](/sections/docker/clean-cache.md)

<br /><br /><br />

## ![✔] 8.14. Generic Docker practices

**TL;DR:** This is a collection of Docker advice that is not related directly to Node.js - the Node implementation is not much different than any other language. Click read more to skim through.

🔗 [**Read More: Generic Docker practices**](/sections/docker/generic-tips.md)

<br/><br /><br />

## ![✔] 8.15. Lint your Dockerfile

**TL;DR:** Linting your Dockerfile is an important step to identify issues in your Dockerfile which differ from best practices. By checking for potential flaws using a specialised Docker linter, performance and security improvements can be easily identified, saving countless hours of wasted time or security issues in production code.

**Otherwise:** Mistakenly the Dockerfile creator left Root as the production user, and also used an image from unknown source repository. This could be avoided with with just a simple linter.

🔗 [**Read More: Lint your Dockerfile**](/sections/docker/lint-dockerfile.md)

<br/><br /><br />

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# Milestones

To maintain this guide and keep it up to date, we are constantly updating and improving the guidelines and best practices with the help of the community. You can follow our [milestones](https://github.com/goldbergyoni/nodebestpractices/milestones) and join the working groups if you want to contribute to this project

<br/>

## Translations

All translations are contributed by the community. We will be happy to get any help with either completed, ongoing or new translations!

### Completed translations

- ![BR](/assets/flags/BR.png) [Brazilian Portuguese](./README.brazilian-portuguese.md) - Courtesy of [Marcelo Melo](https://github.com/marcelosdm)
- ![CN](/assets/flags/CN.png) [Chinese](./README.chinese.md) - Courtesy of [Matt Jin](https://github.com/mattjin)
- ![RU](/assets/flags/RU.png) [Russian](./README.russian.md) - Courtesy of [Alex Ivanov](https://github.com/contributorpw)
- ![PL](/assets/flags/PL.png) [Polish](./README.polish.md) - Courtesy of [Michal Biesiada](https://github.com/mbiesiad)
- ![JA](/assets/flags/JA.png) [Japanese](./README.japanese.md) - Courtesy of [Yuki Ota](https://github.com/YukiOta), [Yuta Azumi](https://github.com/YA21)

### Translations in progress

- ![FR](/assets/flags/FR.png) [French](https://github.com/gaspaonrocks/nodebestpractices/blob/french-translation/README.french.md) ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/129))
- ![HE](/assets/flags/HE.png) Hebrew ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/156))
- ![KR](/assets/flags/KR.png) [Korean](README.korean.md) - Courtesy of [Sangbeom Han](https://github.com/uronly14me) ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/94))
- ![ES](/assets/flags/ES.png) [Spanish](https://github.com/goldbergyoni/nodebestpractices/blob/spanish-translation/README.spanish.md) ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/95))
- ![TR](/assets/flags/TR.png) Turkish ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/139))
- ![EU](/assets/flags/EU.png) [Basque](README.basque.md) - Courtesy of [Ane Diaz de Tuesta](https://github.com/anediaz) & Joxefe Diaz de Tuesta ([Discussion](https://github.com/goldbergyoni/nodebestpractices/issues/842))

<br/><br/>

## Steering Committee

Meet the steering committee members - the people who work together to provide guidance and future direction to the project. In addition, each member of the committee leads a project tracked under our [GitHub projects](https://github.com/goldbergyoni/nodebestpractices/projects).

<img align="left" width="100" height="100" src="assets/images/members/yoni.png">

[Yoni Goldberg](https://github.com/goldbergyoni)
<a href="https://twitter.com/goldbergyoni"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://goldbergyoni.com"><img src="assets/images/www.png" width="16" height="16"></img></a>

Independent Node.js consultant who works with customers in the USA, Europe, and Israel on building large-scale Node.js applications. Many of the best practices above were first published at [goldbergyoni.com](https://goldbergyoni.com). Reach Yoni at [@goldbergyoni](https://github.com/goldbergyoni) or [me@goldbergyoni.com](mailto:me@goldbergyoni.com)

<br/>

<img align="left" width="100" height="100" src="assets/images/members/bruno.png">

[Bruno Scheufler](https://github.com/BrunoScheufler)
<a href="https://brunoscheufler.com/"><img src="assets/images/www.png" width="16" height="16"></img></a>

💻 full-stack web engineer, Node.js & GraphQL enthusiast

<br/>

<img align="left" width="100" height="100" src="assets/images/members/kyle.png">

[Kyle Martin](https://github.com/js-kyle)
<a href="https://twitter.com/kylemartin_93"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://www.linkedin.com/in/kylemartinnz"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Full Stack Developer & Site Reliability Engineer based in New Zealand, interested in web application security, and architecting and building Node.js applications to perform at global scale.

<br/>

<img align="left" width="100" height="100" src="assets/images/members/kevyn.png">

[Kevyn Bruyere](https://github.com/kevynb)
<a href="https://www.linkedin.com/in/kevynbruyere/"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>

Independent full-stack developer with a taste for Ops and automation.

<br/>

### Steering Committee Emeriti

<img align="left" width="100" height="100" src="assets/images/members/sagir.png">

[Sagir Khan](https://github.com/sagirk)
<a href="https://twitter.com/sagir_k"><img src="assets/images/twitter-s.png" width="16" height="16"></img></a>
<a href="https://linkedin.com/in/sagirk"><img src="assets/images/linkedin.png" width="16" height="16"></img></a>
<a href="https://sagirk.com"><img src="assets/images/www.png" width="16" height="16"></img></a>

Deep specialist in JavaScript and its ecosystem — React, Node.js, TypeScript, GraphQL, MongoDB, pretty much anything that involves JS/JSON in any layer of the system — building products using the web platform for the world’s most recognized brands. Individual Member of the Node.js Foundation.

<br/>

## Collaborators

Thank you to all our collaborators! 🙏

Our collaborators are members who are contributing to the repository on a regular basis, through suggesting new best practices, triaging issues, reviewing pull requests and more. If you are interested in helping us guide thousands of people to craft better Node.js applications, please read our [contributor guidelines](/.operations/CONTRIBUTING.md) 🎉

| <a href="https://github.com/idori" target="_blank"><img src="assets/images/members/ido.png" width="75" height="75"></a> | <a href="https://github.com/TheHollidayInn" target="_blank"><img src="assets/images/members/keith.png" width="75" height="75"></a> |target="_blank"><img src="assets/images/members/keith.png" width="75" height="75"></a> |
| :---------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------: |
|                                    [Ido Richter (Founder)](https://github.com/idori)                                    |                                        [Keith Holliday](https://github.com/TheHollidayInn)                                         |

### Collaborator Emeriti

| <a href="https://github.com/refack" target="_blank"><img src="assets/images/members/refael.png" width="50" height="50"></a> |
| :-------------------------------------------------------------------------------------------------------------------------: |
|                                        [Refael Ackermann](https://github.com/refack)                                        |

<br/>

## Contributing

If you've ever wanted to contribute to open source, now is your chance! See the [contributing docs](.operations/CONTRIBUTING.md) for more information.

## Contributors ✨

Thanks goes to these wonderful people who have contributed to this repository!

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/kevinrambaud"><img src="https://avatars1.githubusercontent.com/u/7501477?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kevin Rambaud</b></sub></a><br /><a href="#content-kevinrambaud" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/mfine15"><img src="https://avatars1.githubusercontent.com/u/1286554?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Michael Fine</b></sub></a><br /><a href="#content-mfine15" title="Content">🖋</a></td>
    <td align="center"><a href="http://squgeim.github.io"><img src="https://avatars0.githubusercontent.com/u/4996818?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Shreya Dahal</b></sub></a><br /><a href="#content-squgeim" title="Content">🖋</a></td>
    <td align="center"><a href="http://matheusrocha89.com"><img src="https://avatars1.githubusercontent.com/u/3718366?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Matheus Cruz Rocha</b></sub></a><br /><a href="#content-matheusrocha89" title="Content">🖋</a></td>
    <td align="center"><a href="https://bityog.github.io/Portfolio/"><img src="https://avatars2.githubusercontent.com/u/28219178?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Yog Mehta</b></sub></a><br /><a href="#content-BitYog" title="Content">🖋</a></td>
    <td align="center"><a href="http://kudapara.co.zw"><img src="https://avatars3.githubusercontent.com/u/13519184?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kudakwashe Paradzayi</b></sub></a><br /><a href="#content-kudapara" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.t1st3.com/"><img src="https://avatars1.githubusercontent.com/u/1469638?v=4?s=100" width="100px;" alt=""/><br /><sub><b>t1st3</b></sub></a><br /><a href="#content-t1st3" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/mulijordan1976"><img src="https://avatars0.githubusercontent.com/u/33382022?v=4?s=100" width="100px;" alt=""/><br /><sub><b>mulijordan1976</b></sub></a><br /><a href="#content-mulijordan1976" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/matchai"><img src="https://avatars0.githubusercontent.com/u/4658208?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Matan Kushner</b></sub></a><br /><a href="#content-matchai" title="Content">🖋</a></td>
    <td align="center"><a href="https://fabiothiroki.github.io"><img src="https://avatars2.githubusercontent.com/u/670057?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Fabio Hiroki</b></sub></a><br /><a href="#content-fabiothiroki" title="Content">🖋</a></td>
    <td align="center"><a href="http://james.sumners.info/"><img src="https://avatars1.githubusercontent.com/u/321201?v=4?s=100" width="100px;" alt=""/><br /><sub><b>James Sumners</b></sub></a><br /><a href="#content-jsumners" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/_DanGamble"><img src="https://avatars2.githubusercontent.com/u/7152041?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dan Gamble</b></sub></a><br /><a href="#content-dan-gamble" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/trainorpj"><img src="https://avatars3.githubusercontent.com/u/13276704?v=4?s=100" width="100px;" alt=""/><br /><sub><b>PJ Trainor</b></sub></a><br /><a href="#content-trainorpj" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/reod"><img src="https://avatars0.githubusercontent.com/u/3164299?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Remek Ambroziak</b></sub></a><br /><a href="#content-reod" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://ca.non.co.il"><img src="https://avatars0.githubusercontent.com/u/1829789?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Yoni Jah</b></sub></a><br /><a href="#content-yonjah" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/hazolsky"><img src="https://avatars1.githubusercontent.com/u/1270790?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Misha Khokhlov</b></sub></a><br /><a href="#content-hazolsky" title="Content">🖋</a></td>
    <td align="center"><a href="https://plus.google.com/+ЕвгенийОрехов/"><img src="https://avatars3.githubusercontent.com/u/8045060?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Evgeny Orekhov</b></sub></a><br /><a href="#content-EvgenyOrekhov" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/gediminasml"><img src="https://avatars3.githubusercontent.com/u/19854105?v=4?s=100" width="100px;" alt=""/><br /><sub><b>-</b></sub></a><br /><a href="#content-gediminasml" title="Content">🖋</a></td>
    <td align="center"><a href="http://hisaac.net"><img src="https://avatars3.githubusercontent.com/u/923876?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Isaac Halvorson</b></sub></a><br /><a href="#content-hisaac" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.vedrankaracic.com"><img src="https://avatars3.githubusercontent.com/u/2808092?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Vedran Karačić</b></sub></a><br /><a href="#content-vkaracic" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/lallenlowe"><img src="https://avatars3.githubusercontent.com/u/10761165?v=4?s=100" width="100px;" alt=""/><br /><sub><b>lallenlowe</b></sub></a><br /><a href="#content-lallenlowe" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/nwwells"><img src="https://avatars2.githubusercontent.com/u/1039473?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nathan Wells</b></sub></a><br /><a href="#content-nwwells" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/paulovitin"><img src="https://avatars0.githubusercontent.com/u/125503?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Paulo Reis</b></sub></a><br /><a href="#content-paulovitin" title="Content">🖋</a></td>
    <td align="center"><a href="https://snap.simpego.ch"><img src="https://avatars2.githubusercontent.com/u/1989646?v=4?s=100" width="100px;" alt=""/><br /><sub><b>syzer</b></sub></a><br /><a href="#content-syzer" title="Content">🖋</a></td>
    <td align="center"><a href="http://sancho.dev"><img src="https://avatars0.githubusercontent.com/u/3763599?v=4?s=100" width="100px;" alt=""/><br /><sub><b>David Sancho</b></sub></a><br /><a href="#content-davesnx" title="Content">🖋</a></td>
    <td align="center"><a href="https://apiforge.it"><img src="https://avatars0.githubusercontent.com/u/4929965?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Robert Manolea</b></sub></a><br /><a href="#content-pupix" title="Content">🖋</a></td>
    <td align="center"><a href="https://jumptoglide.com"><img src="https://avatars2.githubusercontent.com/u/708395?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Xavier Ho</b></sub></a><br /><a href="#content-spaxe" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.ocular-rhythm.io"><img src="https://avatars0.githubusercontent.com/u/2738518?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Aaron</b></sub></a><br /><a href="#content-ocularrhythm" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://septa97.me"><img src="https://avatars2.githubusercontent.com/u/13742634?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jan Charles Maghirang Adona</b></sub></a><br /><a href="#content-septa97" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.cakeresume.com/allenfang"><img src="https://avatars2.githubusercontent.com/u/5351390?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Allen</b></sub></a><br /><a href="#content-AllenFang" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/leonardovillela"><img src="https://avatars3.githubusercontent.com/u/8650543?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Leonardo Villela</b></sub></a><br /><a href="#content-leonardovillela" title="Content">🖋</a></td>
    <td align="center"><a href="https://michalzalecki.com"><img src="https://avatars1.githubusercontent.com/u/3136577?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Michał Załęcki</b></sub></a><br /><a href="#content-MichalZalecki" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.wealthbar.com"><img src="https://avatars1.githubusercontent.com/u/156449?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Chris Nicola</b></sub></a><br /><a href="#content-chrisnicola" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/aecorredor"><img src="https://avatars3.githubusercontent.com/u/9114987?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alejandro Corredor</b></sub></a><br /><a href="#content-aecorredor" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/cwar"><img src="https://avatars3.githubusercontent.com/u/272843?v=4?s=100" width="100px;" alt=""/><br /><sub><b>cwar</b></sub></a><br /><a href="#content-cwar" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/keyfoxth"><img src="https://avatars3.githubusercontent.com/u/10647132?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Yuwei</b></sub></a><br /><a href="#content-keyfoxth" title="Content">🖋</a></td>
    <td align="center"><a href="https://bigcodenerd.org"><img src="https://avatars3.githubusercontent.com/u/10895594?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Utkarsh Bhatt</b></sub></a><br /><a href="#content-utkarshbhatt12" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/duartemendes"><img src="https://avatars2.githubusercontent.com/u/12852058?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Duarte Mendes</b></sub></a><br /><a href="#content-duartemendes" title="Content">🖋</a></td>
    <td align="center"><a href="http://jasonkim.ca"><img src="https://avatars2.githubusercontent.com/u/103456?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jason Kim</b></sub></a><br /><a href="#content-serv" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Max101"><img src="https://avatars2.githubusercontent.com/u/2124249?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mitja O.</b></sub></a><br /><a href="#content-Max101" title="Content">🖋</a></td>
    <td align="center"><a href="http://sandromiguel.com"><img src="https://avatars0.githubusercontent.com/u/6423157?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sandro Miguel Marques</b></sub></a><br /><a href="#content-SandroMiguel" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/GabeKuslansky"><img src="https://avatars3.githubusercontent.com/u/9855482?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Gabe</b></sub></a><br /><a href="#content-GabeKuslansky" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://ripper234.com/"><img src="https://avatars1.githubusercontent.com/u/172282?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ron Gross</b></sub></a><br /><a href="#content-ripper234" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.thecodebarbarian.com"><img src="https://avatars2.githubusercontent.com/u/1620265?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Valeri Karpov</b></sub></a><br /><a href="#content-vkarpov15" title="Content">🖋</a></td>
    <td align="center"><a href="https://sergiobernal.com"><img src="https://avatars3.githubusercontent.com/u/20087388?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sergio Bernal</b></sub></a><br /><a href="#content-imsergiobernal" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ntelkedzhiev"><img src="https://avatars2.githubusercontent.com/u/7332371?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nikola Telkedzhiev</b></sub></a><br /><a href="#content-ntelkedzhiev" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/vitordagamagodoy"><img src="https://avatars0.githubusercontent.com/u/26370059?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Vitor Godoy</b></sub></a><br /><a href="#content-vitordagamagodoy" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.manishsaraan.com/"><img src="https://avatars2.githubusercontent.com/u/19797340?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Manish Saraan</b></sub></a><br /><a href="#content-manishsaraan" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/uronly14me"><img src="https://avatars2.githubusercontent.com/u/5186814?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sangbeom Han</b></sub></a><br /><a href="#content-uronly14me" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://blackmatch.github.io"><img src="https://avatars3.githubusercontent.com/u/12443954?v=4?s=100" width="100px;" alt=""/><br /><sub><b>blackmatch</b></sub></a><br /><a href="#content-blackmatch" title="Content">🖋</a></td>
    <td align="center"><a href="https://simmsreeve.com"><img src="https://avatars3.githubusercontent.com/u/5173131?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Joe Reeve</b></sub></a><br /><a href="#content-ISNIT0" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/BusbyActual"><img src="https://avatars2.githubusercontent.com/u/14985016?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ryan Busby</b></sub></a><br /><a href="#content-BusbyActual" title="Content">🖋</a></td>
    <td align="center"><a href="http://jsdecorator.com"><img src="https://avatars3.githubusercontent.com/u/4482199?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Iman Mohamadi</b></sub></a><br /><a href="#content-ImanMh" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/HeeL"><img src="https://avatars1.githubusercontent.com/u/287769?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sergii Paryzhskyi</b></sub></a><br /><a href="#content-HeeL" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/kapilepatel"><img src="https://avatars3.githubusercontent.com/u/25738473?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kapil Patel</b></sub></a><br /><a href="#content-kapilepatel" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/justjavac"><img src="https://avatars1.githubusercontent.com/u/359395?v=4?s=100" width="100px;" alt=""/><br /><sub><b>迷渡</b></sub></a><br /><a href="#content-justjavac" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/hozefaj"><img src="https://avatars1.githubusercontent.com/u/2084833?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Hozefa</b></sub></a><br /><a href="#content-hozefaj" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/el-ethan"><img src="https://avatars3.githubusercontent.com/u/10249884?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ethan</b></sub></a><br /><a href="#content-el-ethan" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/milkdeliver"><img src="https://avatars2.githubusercontent.com/u/3108407?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sam</b></sub></a><br /><a href="#content-milkdeliver" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ArlindXh"><img src="https://avatars0.githubusercontent.com/u/19508764?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Arlind</b></sub></a><br /><a href="#content-ArlindXh" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ttous"><img src="https://avatars0.githubusercontent.com/u/19815440?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Teddy Toussaint</b></sub></a><br /><a href="#content-ttous" title="Content">🖋</a></td>
    <td align="center"><a href="http://ardern.io"><img src="https://avatars2.githubusercontent.com/u/2419690?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Lewis</b></sub></a><br /><a href="#content-LewisArdern" title="Content">🖋</a></td>
    <td align="center"><a href="https://gabriellidenor.com/"><img src="https://avatars2.githubusercontent.com/u/765963?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Gabriel Lidenor </b></sub></a><br /><a href="#content-GabrielLidenor" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/animir"><img src="https://avatars3.githubusercontent.com/u/4623196?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Roman</b></sub></a><br /><a href="#content-animir" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Francozeira"><img src="https://avatars1.githubusercontent.com/u/47419763?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Francozeira</b></sub></a><br /><a href="#content-Francozeira" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/invvard"><img src="https://avatars0.githubusercontent.com/u/7305493?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Invvard</b></sub></a><br /><a href="#content-Invvard" title="Content">🖋</a></td>
    <td align="center"><a href="https://romulogarofalo.github.io/"><img src="https://avatars1.githubusercontent.com/u/18492592?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Rômulo Garofalo</b></sub></a><br /><a href="#content-romulogarofalo" title="Content">🖋</a></td>
    <td align="center"><a href="http://thoqbk.github.io/"><img src="https://avatars0.githubusercontent.com/u/1491103?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tho Q Luong</b></sub></a><br /><a href="#content-thoqbk" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Qeneke"><img src="https://avatars2.githubusercontent.com/u/20271568?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Burak Shen</b></sub></a><br /><a href="#content-Qeneke" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.happy-css.com"><img src="https://avatars0.githubusercontent.com/u/2950505?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Martin Muzatko</b></sub></a><br /><a href="#content-MartinMuzatko" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/autoboxer"><img src="https://avatars3.githubusercontent.com/u/2757601?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jared Collier</b></sub></a><br /><a href="#content-autoboxer" title="Content">🖋</a></td>
    <td align="center"><a href="http://hiltonmeyer.com"><img src="https://avatars3.githubusercontent.com/u/4545860?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Hilton Meyer</b></sub></a><br /><a href="#content-bikingbadger" title="Content">🖋</a></td>
    <td align="center"><a href="http://kr.vuejs.org"><img src="https://avatars0.githubusercontent.com/u/1451365?v=4?s=100" width="100px;" alt=""/><br /><sub><b>ChangJoo Park(박창주)</b></sub></a><br /><a href="#content-ChangJoo-Park" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/MasahiroSakaguchi"><img src="https://avatars0.githubusercontent.com/u/16427431?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Masahiro Sakaguchi</b></sub></a><br /><a href="#content-MasahiroSakaguchi" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/TheHollidayInn"><img src="https://avatars1.githubusercontent.com/u/1253400?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Keith Holliday</b></sub></a><br /><a href="#content-TheHollidayInn" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.coreycleary.me"><img src="https://avatars3.githubusercontent.com/u/1485356?v=4?s=100" width="100px;" alt=""/><br /><sub><b>coreyc</b></sub></a><br /><a href="#content-coreyc" title="Content">🖋</a></td>
    <td align="center"><a href="http://maxcubing.wordpress.com"><img src="https://avatars0.githubusercontent.com/u/8260834?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Maximilian Berkmann</b></sub></a><br /><a href="#content-Berkmann18" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/DouglasMV"><img src="https://avatars3.githubusercontent.com/u/32845487?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Douglas Mariano Valero</b></sub></a><br /><a href="#content-DouglasMV" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/marcelosdm"><img src="https://avatars0.githubusercontent.com/u/18266600?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Marcelo Melo</b></sub></a><br /><a href="#content-marcelosdm" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/mperk_"><img src="https://avatars0.githubusercontent.com/u/3465794?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mehmet Perk</b></sub></a><br /><a href="#content-mperk" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/ryanouyang"><img src="https://avatars2.githubusercontent.com/u/360426?v=4?s=100" width="100px;" alt=""/><br /><sub><b>ryan ouyang</b></sub></a><br /><a href="#content-ryanouyang" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/shabeer-mdy"><img src="https://avatars0.githubusercontent.com/u/26842535?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Shabeer</b></sub></a><br /><a href="#content-shabeer-mdy" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/halfzebra"><img src="https://avatars1.githubusercontent.com/u/3983879?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Eduard Kyvenko</b></sub></a><br /><a href="#content-halfzebra" title="Content">🖋</a></td>
    <td align="center"><a href="http://deyvisonrocha.com"><img src="https://avatars2.githubusercontent.com/u/686067?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Deyvison Rocha</b></sub></a><br /><a href="#content-deyvisonrocha" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="http://twitter.com/georgemamer"><img src="https://avatars1.githubusercontent.com/u/20108934?v=4?s=100" width="100px;" alt=""/><br /><sub><b>George Mamer</b></sub></a><br /><a href="#content-georgem3" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/leimonio"><img src="https://avatars0.githubusercontent.com/u/1969742?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Konstantinos Leimonis</b></sub></a><br /><a href="#content-leimonio" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/Zybax"><img src="https://avatars3.githubusercontent.com/u/22094453?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Oliver Lluberes</b></sub></a><br /><a href="#translation-Zybax" title="Translation">🌍</a></td>
    <td align="center"><a href="https://stackoverflow.com/story/tiendq"><img src="https://avatars2.githubusercontent.com/u/815910?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tien Do</b></sub></a><br /><a href="#content-tiendq" title="Content">🖋</a></td>
    <td align="center"><a href="http://singh1114.github.io/"><img src="https://avatars0.githubusercontent.com/u/11356398?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ranvir Singh</b></sub></a><br /><a href="#content-singh1114" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/collierrgbsitisfise"><img src="https://avatars3.githubusercontent.com/u/13496126?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Vadim Nicolaev</b></sub></a><br /><a href="#content-collierrgbsitisfise" title="Content">🖋</a> <a href="#translation-collierrgbsitisfise" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/germangamboa95"><img src="https://avatars3.githubusercontent.com/u/28633849?v=4?s=100" width="100px;" alt=""/><br /><sub><b>German Gamboa Gonzalez</b></sub></a><br /><a href="#content-germangamboa95" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/AbdelrahmanHafez"><img src="https://avatars3.githubusercontent.com/u/19984935?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Hafez</b></sub></a><br /><a href="#content-AbdelrahmanHafez" title="Content">🖋</a></td>
    <td align="center"><a href="http://linkedin.com/in/chandiran-dmc"><img src="https://avatars3.githubusercontent.com/u/42678579?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Chandiran</b></sub></a><br /><a href="#content-chandiran-dmc" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/VinayaSathyanarayana"><img src="https://avatars2.githubusercontent.com/u/16976677?v=4?s=100" width="100px;" alt=""/><br /><sub><b>VinayaSathyanarayana</b></sub></a><br /><a href="#content-VinayaSathyanarayana" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.kimkern.de"><img src="https://avatars1.githubusercontent.com/u/2671139?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kim Kern</b></sub></a><br /><a href="#content-kiwikern" title="Content">🖋</a></td>
    <td align="center"><a href="https://kennethfreitas.github.io/"><img src="https://avatars2.githubusercontent.com/u/55669043?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kenneth Freitas</b></sub></a><br /><a href="#content-kennethfreitas" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/songe"><img src="https://avatars2.githubusercontent.com/u/1531561?v=4?s=100" width="100px;" alt=""/><br /><sub><b>songe</b></sub></a><br /><a href="#content-songe" title="Content">🖋</a></td>
    <td align="center"><a href="http://ksed.dev"><img src="https://avatars1.githubusercontent.com/u/30693707?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Kirill Shekhovtsov</b></sub></a><br /><a href="#content-Ksedline" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/SerzN1"><img src="https://avatars0.githubusercontent.com/u/2534649?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Serge</b></sub></a><br /><a href="#content-SerzN1" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/keyrwinz"><img src="https://avatars3.githubusercontent.com/u/21241761?v=4?s=100" width="100px;" alt=""/><br /><sub><b>keyrwinz</b></sub></a><br /><a href="#content-keyrwinz" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/nDmitry"><img src="https://avatars0.githubusercontent.com/u/2134568?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dmitry Nikitenko</b></sub></a><br /><a href="#content-nDmitry" title="Content">🖋</a></td>
    <td align="center"><a href="https://bushuai.cc"><img src="https://avatars0.githubusercontent.com/u/1875256?v=4?s=100" width="100px;" alt=""/><br /><sub><b>bushuai</b></sub></a><br /><a href="https://github.com/goldbergyoni/nodebestpractices/pulls?q=is%3Apr+reviewed-by%3Abushuai" title="Reviewed Pull Requests">👀</a> <a href="#content-bushuai" title="Content">🖋</a></td>
    <td align="center"><a href="https://stackoverflow.com/users/1348195/benjamin-gruenbaum"><img src="https://avatars2.githubusercontent.com/u/1315533?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Benjamin Gruenbaum</b></sub></a><br /><a href="#content-benjamingr" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/byeze"><img src="https://avatars1.githubusercontent.com/u/7424138?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ezequiel</b></sub></a><br /><a href="#translation-byeze" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/juaoose"><img src="https://avatars3.githubusercontent.com/u/994594?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Juan José Rodríguez</b></sub></a><br /><a href="#translation-juaoose" title="Translation">🌍</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/OrBin"><img src="https://avatars1.githubusercontent.com/u/6897234?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Or Bin</b></sub></a><br /><a href="#content-OrBin" title="Content">🖋</a></td>
    <td align="center"><a href="https://twitter.com/andreoav07"><img src="https://avatars2.githubusercontent.com/u/508827?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Andreo Vieira</b></sub></a><br /><a href="#content-andreoav" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/mikicho"><img src="https://avatars1.githubusercontent.com/u/11459632?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Michael Solomon</b></sub></a><br /><a href="#content-mikicho" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/jimmycallin"><img src="https://avatars0.githubusercontent.com/u/2225828?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jimmy Callin</b></sub></a><br /><a href="#content-jimmycallin" title="Content">🖋</a></td>
    <td align="center"><a href="https://www.linkedin.com/in/siddharthofficial/"><img src="https://avatars2.githubusercontent.com/u/26025955?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Siddharth</b></sub></a><br /><a href="#content-w01fS" title="Content">🖋</a></td>
    <td align="center"><a href="https://ryansmith.tech/"><img src="https://avatars0.githubusercontent.com/u/1578766?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ryan Smith</b></sub></a><br /><a href="#content-ryan3E0" title="Content">🖋</a></td>
    <td align="center"><a href="https://de.linkedin.com/in/tom-boettger"><img src="https://avatars2.githubusercontent.com/u/49961674?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tom Boettger</b></sub></a><br /><a href="#content-bttger" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/jormaechea"><img src="https://avatars3.githubusercontent.com/u/5612500?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Joaquín Ormaechea</b></sub></a><br /><a href="#translation-jormaechea" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/dfrzuz"><img src="https://avatars3.githubusercontent.com/u/71859096?v=4?s=100" width="100px;" alt=""/><br /><sub><b>dfrzuz</b></sub></a><br /><a href="#translation-dfrzuz" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/victor-homyakov"><img src="https://avatars1.githubusercontent.com/u/121449?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Victor Homyakov</b></sub></a><br /><a href="#content-victor-homyakov" title="Content">🖋</a></td>
    <td align="center"><a href="http://joshuahemphill.com"><img src="https://avatars3.githubusercontent.com/u/46608115?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Josh</b></sub></a><br /><a href="#content-josh-hemphill" title="Content">🖋</a> <a href="#security-josh-hemphill" title="Security">🛡️</a></td>
    <td align="center"><a href="https://github.com/alec-francis"><img src="https://avatars2.githubusercontent.com/u/32949882?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alec Francis</b></sub></a><br /><a href="#content-alec-francis" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/arjun6610"><img src="https://avatars1.githubusercontent.com/u/61268891?v=4?s=100" width="100px;" alt=""/><br /><sub><b>arjun6610</b></sub></a><br /><a href="#content-arjun6610" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/jan-osch"><img src="https://avatars2.githubusercontent.com/u/11651780?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jan Osch</b></sub></a><br /><a href="#content-jan-osch" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/thiagotrs"><img src="https://avatars2.githubusercontent.com/u/32005779?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Thiago Rotondo Sampaio</b></sub></a><br /><a href="#translation-thiagotrs" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/Alexsey"><img src="https://avatars0.githubusercontent.com/u/6392013?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alexsey</b></sub></a><br /><a href="#content-Alexsey" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/13luismb"><img src="https://avatars1.githubusercontent.com/u/32210483?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Luis A. Acurero</b></sub></a><br /><a href="#translation-13luismb" title="Translation">🌍</a></td>
    <td align="center"><a href="https://lromano97.github.io/"><img src="https://avatars1.githubusercontent.com/u/22394847?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Lucas Romano</b></sub></a><br /><a href="#translation-lromano97" title="Translation">🌍</a></td>
    <td align="center"><a href="https://github.com/denisecase"><img src="https://avatars0.githubusercontent.com/u/13016516?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Denise Case</b></sub></a><br /><a href="#content-denisecase" title="Content">🖋</a></td>
    <td align="center"><a href="http://stackoverflow.com/story/elektronik"><img src="https://avatars3.githubusercontent.com/u/1078554?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nick Ribal</b></sub></a><br /><a href="#content-elektronik2k5" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/0xflotus"><img src="https://avatars3.githubusercontent.com/u/26602940?v=4?s=100" width="100px;" alt=""/><br /><sub><b>0xflotus</b></sub></a><br /><a href="#content-0xflotus" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://www.dijonkitchen.org/"><img src="https://avatars3.githubusercontent.com/u/11434205?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jonathan Chen</b></sub></a><br /><a href="#content-dijonkitchen" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/dilansri"><img src="https://avatars2.githubusercontent.com/u/5089728?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Dilan Srilal</b></sub></a><br /><a href="#content-dilansri" title="Content">🖋</a></td>
    <td align="center"><a href="https://vectree.ru"><img src="https://avatars3.githubusercontent.com/u/4215285?v=4?s=100" width="100px;" alt=""/><br /><sub><b>vladthelittleone</b></sub></a><br /><a href="#translation-vladthelittleone" title="Translation">🌍</a></td>
    <td align="center"><a href="https://www.nikolaso.com"><img src="https://avatars0.githubusercontent.com/u/60047271?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nik Osvalds</b></sub></a><br /><a href="#content-nosvalds" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/kdaniel21"><img src="https://avatars0.githubusercontent.com/u/39854385?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Daniel Kiss</b></sub></a><br /><a href="https://github.com/goldbergyoni/nodebestpractices/commits?author=kdaniel21" title="Documentation">📖</a></td>
    <td align="center"><a href="https://twitter.com/forresst17"><img src="https://avatars2.githubusercontent.com/u/163352?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Forresst</b></sub></a><br /><a href="#content-forresst" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/svenheden"><img src="https://avatars1.githubusercontent.com/u/76098?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jonathan Svenheden</b></sub></a><br /><a href="#content-svenheden" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/AustrisC"><img src="https://avatars2.githubusercontent.com/u/12381652?v=4?s=100" width="100px;" alt=""/><br /><sub><b>AustrisC</b></sub></a><br /><a href="#content-AustrisC" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/cisco0808"><img src="https://avatars0.githubusercontent.com/u/60251188?v=4?s=100" width="100px;" alt=""/><br /><sub><b>kyeongtae kim</b></sub></a><br /><a href="#translation-cisco0808" title="Translation">🌍</a></td>
    <td align="center"><a href="https://keybase.io/651z9pz968v2accj"><img src="https://avatars.githubusercontent.com/u/65741741?v=4?s=100" width="100px;" alt=""/><br /><sub><b>007</b></sub></a><br /><a href="#content-6gx7iycn53ioq2e8apk1j1ypwov4giui" title="Content">🖋</a></td>
    <td align="center"><a href="http://www.anediaz.com"><img src="https://avatars.githubusercontent.com/u/17216937?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ane Diaz de Tuesta</b></sub></a><br /><a href="#translation-anediaz" title="Translation">🌍</a></td>
    <td align="center"><a href="http://yukioh.net"><img src="https://avatars.githubusercontent.com/u/23182489?v=4?s=100" width="100px;" alt=""/><br /><sub><b>YukiOta</b></sub></a><br /><a href="#translation-YukiOta" title="Translation">🌍</a></td>
    <td align="center"><a href="https://www.yeovilhospital.co.uk/"><img src="https://avatars.githubusercontent.com/u/43814140?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Frazer Smith</b></sub></a><br /><a href="#content-Fdawgs" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/rluvaton"><img src="https://avatars.githubusercontent.com/u/16746759?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Raz Luvaton</b></sub></a><br /><a href="#content-rluvaton" title="Content">🖋</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/YA21"><img src="https://avatars.githubusercontent.com/u/37298463?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Yuta Azumi</b></sub></a><br /><a href="#content-YA21" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/andrewjbarbour"><img src="https://avatars.githubusercontent.com/u/77080074?v=4?s=100" width="100px;" alt=""/><br /><sub><b>andrewjbarbour</b></sub></a><br /><a href="#content-andrewjbarbour" title="Content">🖋</a></td>
    <td align="center"><a href="https://MasujimaRyohei.jp"><img src="https://avatars.githubusercontent.com/u/17163541?v=4?s=100" width="100px;" alt=""/><br /><sub><b>mr</b></sub></a><br /><a href="#content-MasujimaRyohei" title="Content">🖋</a></td>
    <td align="center"><a href="https://github.com/kubanac95"><img src="https://avatars.githubusercontent.com/u/16191931?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Aleksandar</b></sub></a><br /><a href="#content-kubanac95" title="Content">🖋</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->