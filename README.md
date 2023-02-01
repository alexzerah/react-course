# React

## Qu’est- ce que React ?
React est une bibliothèque JavaScript créée par Meta (Facebook).
React permet notamment de lier les données (state/store) avec l’UI (le DOM).  

## Installation
Il est possible d’installer React de plusieurs façons :
- Via un CDN ou un node\_modules (pour une installation progressive et modulaire).
- Via un CLI, comme Create react app ou vite (pour une installation clé en main, plus rapide, professionnelle et robuste).

```bash
$ npx create-react-app my-app
$ cd my-app
$ npm start
```

## Le dossier

```sh
|-node_modules
|-public
|-src
|-.gitignore
|-package-lock.json
|-package.json
|README.md
```

- `node\_modules` : Ensemble de tous les modules et les modules requis pour l’application.
- `public` : Dossier contenant des fichiers statiques non compilés. Il y a notamment le fichier index.html qui sera envoyé. Ce fichier contient une `div` qui a pour id root, et permet à React de cibler l’élément afin d’y injecter le code de notre app.
- `src` : Dossier contenant toue la logique de notre application (le fichier JS racine, les composants, …)
- `.gitignore`: Fichier fourni contenant les éléments que nous ne souhaitons pas versionnés.
- `package.json` et `package-lock.json` : Fichiers nécessaires à la gestion des modules et leur installation.
- `README.md` :  Documentation réalisée par l’équipe de create-react-app.

## Les composants
Les composants permettent de créer du code réutilisable.

Voici l’exemple d’un composant fonctionnel.

```js
// Books.js

export default function Books(props) {
	return (
		<>
			<h2>{props.title}</h2>
			<h3>{props.author}</h3>
			<p>{props.resume}</p>	
		</>
	)
}
```

Les éléments importants sont :
- `function Books` : Nous créeons une fonction qui porte le nom ShortStory (premiere lettre en majuscule par convention).
-  `export default` : Afin de pouvoir utiliser ce code dans une autre fichier (dans lequel nous allons l’importer), nous devons l’export. Le mot clé default indique que c’est la fonction exporté lorsque nous ne spécifions pas ce que nous voulons.
- `return(...)` : Return permet de renvoyer notre composant sans quoi nous ne pourrions rien afficher.
- `<>...</>` : Balise vide qui contient notre code et permet de l’encapsuler (il n’est pas possible de renvoyer à la racine des éléments adjacents).
- `…` : Ce qu’il y a au sein de la balise vide constitue notre code. Le fait d’avoir du balisage HTML dans notre fichier JS est possible car il s’agit d’un extension syntaxique nommée JSX.

### Importer un composant

```js
 // App.js

import Books from "./components/Books";

export default function App() {

	const [stories, setStories] = useState([
	{
		title: "Ulysse",
		author: "James Joyce",
		content: "L'action d'Ulysse se passe en un jour, à Dublin, en 1904.",
	},
	{
		title: "A la recherche du temps perdu",
		author: "Marcel Proust",
		content: "À l’instant où il mit en bouche la madeleine, le narrateur fut pris d’une étrange sensation : les souvenirs de son enfance ressuscitèrent.",
	},
	{
		title: "Les Raisins de la colère",
		subtitle: "John Steinbeck",
		content: "L’Amérique des années 30, en pleine dépression, à travers la famille Joad chassée de ses terres par la “Banque” et l’industrialisation des cultures.",
	}
	]);

	return(
		<>
			<h1>📚 Short story App</h1>
			stories.map(story => {
				return(<Books title={story.title} author={story.author} resume={story.resume}/>);
			})
			
		</>
	)	
};
```

- `useState` : Hook permettant d’affecter une valeur à un élément du state (ensemble de l’état des données accessible au sein de l’application React). Le premier élément du tableau `stories` constitue la variable contenant l’information. `setStories` constitue la fonction permettant de mettre à jour la valeur `stories`.
- Pour appeler le composant `Book`, nous l’implémentons comme une balise HTML. Il contient des paramètres qui sont passés du composant parent `App` vers le composant enfant `Books`. Ces propriétés sont accessible via la variable `props` du composant `Book`.

#### Questions
- Le nom de la variable de l’objet que nous importons doit-il être le même que celui de la variable du composant ?  

## Les props
Un propriété ou prop, permet d’envoyer des données aux composants utilisés. Par exemple, notre fichier `App.js` envoie les données au composant enfant `Book.js`.

```js
// App.js (simplifié)
<App>
	<Books title={story.title}/>
</App>

// Books.js (simplifié)

function Books(props) {
	return (<h2>{props.title}</h2>)
}
// On accède à la valeur de title grâce à notre variable props, que l'on a créé dans App.js 
```

## Les hooks
Les hooks permettent d’utiliser certaines fonctionnalités, notamment le state sans avoir besoin de classes (obligatoire avant).
Il existe plusieurs hooks natifs et vous pouvez écrire les votre.

Deux hooks sont particulièrement importants :
- `useState()` que nous avons déjà vu. Il permet déclarer une valeur au sein du state et de pouvoir la modifier avec la méthode appropriée (`stories` et `setStories()` dans notre exemple) .
- `useEffect(function, dependencies?)` permet de lancer une fonction lorsque le composant est appelé une première fois. Ensuite, lorsque les dépendances sont modifiées, le fonction est relancée.Il est important d’indiquer ou non des dépendances car il peut y avoir un effet de bord entrainant une boucle infinie.Généralement, vous utiliserez au début `useEffect()` pour vos call API.
 
 ```js
 // App.js
import { useEffect, useState } from "react";
import Books from "./components/Books";

export default function App() {

	const [stories, setStories] = useState([])
	
	useEffect(() => {
		// cette URL est un exemple. Elle ne fonctionne pas.
		 fetch("https://api.com/books")
			.then(res => { return res.json() })
			.then(data => { setStories(data)})
			/* Ici les données ne sont pas chargées instantannéments, même si à l'échelle humaine elles peuvent le sembler.Le traiement JS est tel qu'au départ, stories vaut []. Il faut donc gérer ce cas avec des conditions.
			*/
			
	}, [] ) // Ici je mets un tableau vide car je ne veux pas que celle-ci entraine un nouveau rendu.

	return(
		<>
			<h1>📚 Short story App</h1>
			stories.map(story => {
				return(<Books title={story.title} author={story?.author} resume={story.resume}/>);
			})
			
		</>
	)	
};
```

## React Router
React Router est une solution développé par la communauté React, devenue une solution officielle.

### Etapes 
#### Importer les modules

`$ npm install react-router-dom`

```js
// Dans votre fichier principal (par exemple `index.js` ou `App.js`)
import ReactDOM from "react-dom/router";
import {CreateBrowserRouter, RouterProvider} from "react-router-dom";
import Books from "./components/Books";

const router = createBrowserRouter([
	{
		path: "/books",
		element: <Books/>
	}
])

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

## Sources
Documentation officielle : 
- [React doc officielle](https://beta.reactjs.org/)
- [Create react app](https://create-react-app.dev/docs/getting-started/)
- [React router](https://reactrouter.com/en/main)
- [Dan Abramov](https://overreacted.io/)
- [Michael Jackson](https://twitter.com/mjackson) (le développeur pas le king of pop 🕺)
- [Remix](https://remix.run/)
- [Next](https://nextjs.org/)
- [Remix X Shopify](https://shopify.engineering/remix-joins-shopify)