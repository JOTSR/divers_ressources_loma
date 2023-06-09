---
title: Les technologies du web au service de la science
author: Julien Oculi
date: 2023-06-09Z
---

# Les technologies du web au service de la science

- **Portabilité** - Cross OS, cross plateforme, forward compatibilité (pas de
  breaking changes).
- **Sécurité** - Pas d'accès hors du processeur (filesystem, USB, internet, ...)
  sans authorisation explicit.
- **Performance** - Technologie critiques pour l'industrie donc finement
  optimisé.
- **Accessibilité** - Disponible sur tous les supports (navigateur,
  environnement local, cloud, embarqué, ...).
- **Documentation** - Documentation, tutos, demos, ... à foison.
- **Ressources** - Beacoup, beacoup beaucoup de ressources et de qualité (+1
  million de paquets su npm)
- **Standard** - Utilisation de standard, donc plus longue longévité et
  plusieurs acteur travaillant sur les améliorations.

Technologies **libres**, **open source** et **démocratiques** (les avancées des
technologie sont ouvertes aux propositions et votes de la communauté).

Assurez-vous d'un code aussi simple et expressif que du python, en beaucoup plus sécurisé, rapide, riche.
![Code bench](/assets/code_bench_tri.png)

## Le language

### Définitions

La spécification ECMAScript (European Computer Manufacturers Association) et
ourverte au publique et accessible sur github sous le nom de
[TC39](https://github.com/tc39/proposals).

L'implémentation phare de la spéc et nommé Javascript et la documentation
officielle (avec tutos) est disponible sur la
[MDN Web Docs](https://developer.mozilla.org/fr/docs/Web/JavaScript).

Le superset (méta language) le plus populaire et
[Typescript](https://www.typescriptlang.org/) et ajoute une notion de typage
static ultra puissante au JS.

### Caractéristiques

Le JS et un language nativement **asynchrone** et **multiparadygme**.

- Exemple asynchrone
  ```js
  import { sleep, sleepSync } from "https://deno.land/x/delayed@2.0.2/mod.ts";

  async function asyncLogData() {
    console.count("async call");
    await sleep(1_000);
    console.log(
      `Data is ${crypto.randomUUID()} @ ${new Date().toLocaleString()}`,
    );
  }

  function logData() {
    console.count("sync call");
    sleepSync(1_000);
    console.log(
      `Data is ${crypto.randomUUID()} @ ${new Date().toLocaleString()}`,
    );
  }

  console.log(`Sync @ ${new Date().toLocaleString()}`);
  console.time("sync");
  logData();
  logData();
  logData();
  console.timeEnd("sync");

  console.log(`Async @ ${new Date().toLocaleString()}`);
  console.time("sync");
  asyncLogData();
  asyncLogData();
  asyncLogData();
  console.timeEnd("sync");
  ```

  ```sh
  Sync @ 09/06/2023 23:46:05
  sync call: 1
  Data is 8931324f-96e8-4510-8fff-38c9d9ada169 @ 09/06/2023 23:46:07
  sync call: 2
  Data is 846d5836-133b-46bd-88c0-96a30260dd8b @ 09/06/2023 23:46:08
  sync call: 3
  Data is b7239afb-8bb5-427d-8c8e-387c45fff51b @ 09/06/2023 23:46:09
  sync: 3000ms
  Async @ 09/06/2023 23:46:09
  async call: 1
  async call: 2
  async call: 3
  sync: 2ms
  Data is a4f65ddd-4138-4a5b-9905-6fed69ff13be @ 09/06/2023 23:46:10
  Data is d59c3c9e-1880-4484-9cb5-ba57cff86fae @ 09/06/2023 23:46:10
  Data is 3d08fc07-6d6c-4d76-ba81-d2390c9ddeca @ 09/06/2023 23:46:10
  ```

- Exemple multiparadygme
  ```js
  const abscisse = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  //=========
  //Impératif
  //=========
  const ordonnéeRaw = []
  const ordonnée = []

  for (const x of abscisse) {
    ordonnéeRaw.push(-1 * x ** 2 + 4 * x + 5)
  }
  for (const y of ordonnéeRaw) {
    if (y % 2 === 0) {
      ordonnée.push(y)
    }
  }
  //ordonnée = [8, 8, 0, -7, -16, -40]

  //===========
  //Fonctionnel
  //===========
  const ordonnée = abscisse
    .map(x => -1 * x ** 2 + 4 * x + 5)
    .filter(x => x % 2 === 0)
  //ordonnée = [8, 8, 0, -7, -16, -40]
  ```

Le TS ajoute une notion de typage.

```ts
const text = 'message'
const nombre = 5

function decorate(arg: string): string {
    return `👍✨ ${arg}`
}

decorate(text) //Ok, "👍✨ message"
decorate(nombre) //Erreur
```

## L'écosystème

Un écosystème vaste et crossplateforme est disponible.

### Modules (équivalent paquets pip)

Aucune installation nécessaire, les modules sont importés à la première execution.

- NPM - gestionnaire de paquets de [Node.js (environnement JS locale)](https://nodejs.org/en)
  ```ts
  import { derivative } from 'npm:mathjs'
  ```
- Décentralisé (ex: [denoland](https://deno.land), [github](https://github.com), toute url ...)
  ```ts
  import tf from "https://deno.land/x/tensorflow/tf.js"; //Machine learning
  import { factors } from 'https://raw.githubusercontent.com/JOTSR/Denum/main/mod.ts' //get prime factors
  ```

- Fichier local
  ```ts
  import { exported } from './path/to/file.ts'
  ```

### Web API (outils tout en main, standardisés, performants et sécurisés)
Une multitude d'API standardisé, sécurisé et documenté à disposition sans la moindre installation.

- [MDN - Référence Web API](https://developer.mozilla.org/fr/docs/Web/API)
- [Web.dev documentation, tutoriels, ...](https://web.dev)
- [What Web Can Do Today - Exemples et demos](https://whatwebcando.today/)

- Web USB exemple
  ```ts
  const device = await navigator.usb.requestDevice({ filters: [{ vendorId: 0x2341 }] })
  console.log(device.productName) // "Arduino Micro"
  console.log(device.manufacturerName) // "Arduino LLC"
  ```

- Web Serial (port série)
  ```ts
    const port = await navigator.serial.requestPort()
    await port.open({ baudRate: 9600 })
    
    const decoder = new TextDecoderStream()
    
    port.readable.pipeTo(decoder.writable)

    const inputStream = decoder.readable
    const reader = inputStream.getReader()
    
    while (true) {
      const { value, done } = await reader.read()
      if (value) {
        console.log(value)
      }
      if (done) {
        console.log('[readLoop] DONE', done)
        reader.releaseLock()
        break
      }
    }
  ```

- Web speech recognition (reconnaissance vocale)
  ```ts
  const recognition = new SpeechRecognition()

  recognition.addEventListener('result', console.log)

  recognition.start()
  //...
  recognition.stop()
  ```

## WebGPU

Accès au calcul parallèle haute performance via le GPU via l'[API](https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API) ou via des modules clés en main (exemple [calcul matriciel sur GPU](https://deno.land/x/neo@0.0.1-pre.1)).

- [Getting started](https://developer.chrome.com/articles/gpu-compute/)
- [Tuto officiel en français](https://codelabs.developers.google.com/your-first-webgpu-app?hl=fr#0)

## WASM

WebAssembly est un nouveau type de code qui peut être exécuté dans un navigateur web moderne. C'est un langage bas niveau, semblable à l'assembleur permettant d'atteindre des performances proches des applications natives (par exemple écrites en C/C++) tout en fonctionnant sur le Web. WebAssembly est conçu pour fonctionner en lien avec JavaScript.

WebAssembly représente une avancée fondamentale de la plateforme web. Il permet d'exécuter du code (éventuellement écrit depuis différents langages) sur le Web avec des performances similaires aux applications natives.

WebAssembly est conçu pour être utilisé de pair avec JavaScript. Grâce à l'API JavaScript WebAssembly, on peut charger des modules WebAssembly au sein d'une application JavaScript et partager des fonctionnalités entre les deux. Cela permet de tirer parti des performances de WebAssembly et de la flexibilité de JavaScript, même si on ne sait pas écrire du code WebAssembly.

https://developer.mozilla.org/fr/docs/WebAssembly

```ts
const { instance, module } = await WebAssembly.instantiateStreaming(
  fetch("https://wpt.live/wasm/incrementer.wasm"),
)

const increment = instance.exports.increment as (input: number) => number
console.log(increment(41))
```

## HTML et CSS, du graphique à portée de main

Javascript et fait pour s'intégrer nativement avec ces technologies de création d'interface. Ajoutez simplement une interface graphique à vos script pour les rendre accessible au plus grand nombre. Créer des petits outils réutilisable pour simplifier le traitement et l'analyse de données en vous assurant d'un fonctionnement durable dans le temps et indépendant de la plateforme d'execution.

## Et en local

- [Node.js](https://nodejs.org/fr) : Runtime locale originel
- [Deno](https://deno.land) : Runtime basé sur un écosystème moderne, sans config, clé en main, tous les outils intégrés dans un seul binaire portable (pas d'installation nécessaire).
