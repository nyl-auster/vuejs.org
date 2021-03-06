---
type: api
---

## Configuration globale

`Vue.config` est un objet contenant les configurations globales de Vue. Vous pouvez modifier les propriétés listées ci-dessous avant de mettre en place votre application:

### silent

- **Type :** `boolean`

- **Par défaut :** `false`

- **Utilisation :**

  ``` js
  Vue.config.silent = true
  ```

  Supprime tous les logs et warnings de Vue.js.

### optionMergeStrategies

- **Type :** `{ [key: string]: Function }`

- **Par défaut :** `{}`

- **Utilisation :**

  ``` js
  Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {
    return child + 1
  }

  const Profile = Vue.extend({
    _my_option: 1
  })

  // Profile.options._my_option = 2
  ```

  Définit des stratégies personnalisées de fusion pour les options.

  La stratégie de fusion reçoit en arguments la valeur de cette option définie dans le parent et les instances enfants en tant que premier et second argument, respectivement. L'instance de Vue est passée en troisième argument.

- **Voir aussi**: [Stratégies personnalisées de fusion d'options](../guide/mixins.html#Custom-Option-Merge-Strategies)

### devtools

- **Type :** `boolean`

- **Par défaut :** `true` (`false` dans les versions de production)

- **Utilisation :**

  ``` js
  // assurez-vous d'assigner ça de manière synchrone immédiatement après avoir chargé Vue
  Vue.config.devtools = true
  ```

  Autorise ou non l'inspection des [vue-devtools](https://github.com/vuejs/vue-devtools). Cette option a comme valeur par défaut `true` dans les versions de développement et `false` dans les versions de production. Vous pouvez l'assigner à `true` pour activer l'inspection avec les versions de production.

### errorHandler

- **Type :** `Function`

- **Par défaut :** les exceptions ne sont pas interceptées

- **Utilisation :**

  ``` js
  Vue.config.errorHandler = function (err, vm) {
    // gérer le cas d'erreur
  }
  ```

  Définit un gestionnaire pour les erreurs non interceptées pendant le rendu d'un composant et les appels aux observateurs. Ce gestionnaire sera appelé avec comme arguments l'erreur et l'instance de Vue associée.

  > [Sentry](https://sentry.io), un service de traçage d'erreur, fournit une [intégration officielle](https://sentry.io/for/vue/) utilisant cette option.

### ignoredElements

- **Type :** `Array<string>`

- **Par défaut :** `[]`

- **Utilisation :**

  ``` js
  Vue.config.ignoredElements = [
    'my-custom-web-component', 'another-web-component'
  ]
  ```

  Indique à Vue d'ignorer les éléments personnalisés définis en dehors de Vue (ex: en utilisant les API Web Components). Autrement, un message d'avertissement `Unknown custom element` sera affiché, supposant que vous avez oublié d'inscrire un composant global ou fait une faute de frappe dans son nom.

### keyCodes

- **Type :** `{ [key: string]: number | Array<number> }`

- **Par défaut :** `{}`

- **Utilisation :**

  ``` js
  Vue.config.keyCodes = {
    v: 86,
    f1: 112,
    mediaPlayPause: 179,
    up: [38, 87]
  }
  ```

  Définit des alias pour les touches du clavier avec `v-on`.

## API globale

<h3 id="Vue-extend">Vue.extend( options )</h3>

- **Arguments :**
  - `{Object} options`

- **Utilisation :**

  Crée une « sous-classe » du constructeur de base Vue. L'argument doit être un objet contenant les options du composant.

  Le cas spécial à noter ici est l'option `data` - il doit s'agir d'une fonction quand utilisé avec `Vue.extend()`.

  ``` html
  <div id="mount-point"></div>
  ```

  ``` js
  // crée un constructeur réutilisable
  var Profile = Vue.extend({
    template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
    data: function () {
      return {
        firstName: 'Walter',
        lastName: 'White',
        alias: 'Heisenberg'
      }
    }
  })
  // crée une instance de Profile et la monte sur un élément
  new Profile().$mount('#mount-point')
  ```

  Cela donnera comme résultat :

  ``` html
  <p>Walter White aka Heisenberg</p>
  ```

- **Voir aussi :** [Composants](../guide/components.html)

<h3 id="Vue-nextTick">Vue.nextTick( [callback, context] )</h3>

- **Arguments :**
  - `{Function} [callback]`
  - `{Object} [context]`

- **Utilisation :**

  Reporte l'exécution du callback au prochain cycle de mise à jour du DOM. Utilisez-le immédiatement après avoir changé des données afin d'attendre la mise à jour du DOM.

  ``` js
  // modification de données
  vm.msg = 'Hello'
  // le DOM n'a pas encore été mis à jour
  Vue.nextTick(function () {
    // le DOM est à jour
  })
  ```

  > Nouveauté  de la 2.1.0: retourne une Promise si aucune fonction de callback n'est fournie et si Promise est supporté par l'environnement d'exécution.

- **Voir aussi :** [File de mise à jour asynchrone](../guide/reactivity.html#Async-Update-Queue)

<h3 id="Vue-set">Vue.set( object, key, value )</h3>

- **Arguments :**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Retourne:** la valeur assignée.

- **Utilisation :**

  Assigne une propriété à un objet. Si l'objet est réactif, cette méthode s'assure que la propriété est créée en tant que propriété réactive et déclenche les mises à jour de la vue. Ceci est principalement utilisé pour passer outre la limitation de Vue qui est de ne pas pouvoir détecter automatiquement l'ajout de nouvelles propriétés.

  **Notez que l'objet ne peut pas être une instance de Vue, ou l'objet de données à la racine d'une instance de Vue.**

- **Voir aussi :** [Réactivité en détail](../guide/reactivity.html)

<h3 id="Vue-delete">Vue.delete( object, key )</h3>

- **Arguments :**
  - `{Object} object`
  - `{string} key`

- **Utilisation :**

  Supprime une propriété d'un objet. Si l'objet est réactif, cette méthode s'assure que la suppression déclenche les mises à jour de la vue. Ceci est principalement utilisé pour passer outre la limitation de Vue qui est de ne pas pouvoir détecter automatiquement la suppression de propriétés, mais vous devriez rarement en avoir besoin.

**Notez que l'objet ne peut pas être une instance de Vue, ou l'objet de données à la racine d'une instance de Vue.**

- **Voir aussi :** [Réactivité en détail](../guide/reactivity.html)

<h3 id="Vue-directive">Vue.directive( id, [definition] )</h3>

- **Arguments :**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Utilisation :**

  Inscrit ou récupère une directive globale.

  ``` js
  // inscrit une directive
  Vue.directive('my-directive', {
    bind: function () {},
    inserted: function () {},
    update: function () {},
    componentUpdated: function () {},
    unbind: function () {}
  })

  // inscrit une directive comme simple fonction
  Vue.directive('my-directive', function () {
    // cette fonction sera appelée comme `bind` et `update` ci-dessus
  })

  // accesseur, retourne la définition de la directive si inscrite
  var myDirective = Vue.directive('my-directive')
  ```

- **Voir aussi :** [Directives personnalisées](../guide/custom-directive.html)

<h3 id="Vue-filter">Vue.filter( id, [definition] )</h3>

- **Arguments :**
  - `{string} id`
  - `{Function} [definition]`

- **Utilisation :**

  Inscrit ou récupère un filtre global.

  ``` js
  // inscrit un filtre
  Vue.filter('my-filter', function (value) {
    // retourne la valeur modifiée
  })

  // accesseur, retourne le filtre si inscrit
  var myFilter = Vue.filter('my-filter')
  ```

<h3 id="Vue-component">Vue.component( id, [definition] )</h3>

- **Arguments :**
  - `{string} id`
  - `{Function | Object} [definition]`

- **Utilisation :**

  Inscrit ou récupère un composant global. L'inscription assigne aussi automatiquement la propriété `name` du composant au paramètre `id` donné.

  ``` js
  // inscrit un constructeur étendu
  Vue.component('my-component', Vue.extend({ /* ... */ }))

  // inscrit un composant avec un objet options (appelle automatiquement Vue.extend)
  Vue.component('my-component', { /* ... */ })

  // récupère un composant inscrit (retourne toujours le constructeur)
  var MyComponent = Vue.component('my-component')
  ```

- **Voir aussi :** [Composants](../guide/components.html).

<h3 id="Vue-use">Vue.use( plugin )</h3>

- **Arguments :**
  - `{Object | Function} plugin`

- **Utilisation :**

  Installe un plugin Vue.js. Si l'argument plugin est de type Object, il doit exposer une méthode  `install`. S'il s'agit d'une fonction, elle sera utilisée comme méthode d'installation. Cette méthode d'installation sera appelée avec Vue en tant qu'argument.

  Quand cette méthode est appelée avec le même plugin plusieurs fois, le plugin ne sera installée qu'une seule fois.

- **Voir aussi :** [Plugins](../guide/plugins.html).

<h3 id="Vue-mixin">Vue.mixin( mixin )</h3>

- **Arguments :**
  - `{Object} mixin`

- **Utilisation :**

  Applique une mixin globale, qui affecte toutes les instances de Vue créées par la suite. Cela peut être utilisé par les créateurs de plugins pour injecter un composant personnalisé dans les composants. **Non recommandé dans le code applicatif**.

- **Voir aussi :** [Mixins globales](../guide/mixins.html#Global-Mixin)

<h3 id="Vue-compile">Vue.compile( template )</h3>

- **Arguments :**
  - `{string} template`

- **Utilisation :**

  Compile une string template en une fonction de rendu. **Disponible uniquement sur la version standalone.**

  ``` js
  var res = Vue.compile('<div><span>{{ msg }}</span></div>')

  new Vue({
    data: {
      msg: 'hello'
    },
    render: res.render,
    staticRenderFns: res.staticRenderFns
  })
  ```

- **Voir aussi :** [Fonctions de rendu](../guide/render-function.html)
 
<h3 id="Vue-version">Vue.version</h3>

- **Details**: Provides the installed version of Vue as a string. This is especially useful for community plugins and components, where you might use different strategies for different versions.

- **Usage**:

```js
var version = Number(Vue.version.split('.')[0])

if (version === 2) {
  // Vue v2.x.x
} else if (version === 1) {
  // Vue v1.x.x
} else {
  // Unsupported versions of Vue
}
```

## Options / Data

### data

- **Type :** `Object | Function`

- **Restriction :** accepte uniquement une fonction lorsqu'utilisé dans une définition de composant.

- **Détails :**

  L'objet de données pour l'instance de Vue. Vue va de manière récursive convertir ses propriétés en des accesseurs/mutateurs (*getter/setters*) afin de les rendre "réactives". **L'objet doit être un simple objet de base**: les objets natifs tels que les API du navigateur et les propriétés issues du prototype sont ignorées. Une règle d'or est que la donnée doit juste être de la donnée - il n'est pas recommandé d'observer des objets ayant leur propre comportement avec états.

  Une fois observé, vous ne pouvez plus ajouter de propriétés réactives à l'objet de données racine. C'est pourquoi il est recommandé de déclarer dès le départ toutes les propriétés réactives à la racine de l'objet de données, avant de créer l'instance.

  Après que l'instance ait été créée, l'objet de données initial peut être accédé via `vm.$data`. L'instance de Vue servira également de proxy pour toutes les propriétés trouvées dans l'objet de données, donc `vm.a` sera l'équivalent de `vm.$data.a`.

  Les propriétés commençant par `_` ou `$` ne seront **pas** proxyfiées par l'instance de Vue car elles pourraient entrer en conflit avec certaines propriétés internes et méthodes d'API de Vue. Vous devrez y accéder via `vm.$data._property`.

  Lors de la définition d'un **composant**, la propriété `data` doit être déclarée en tant que fonction retournant l'objet de données initial, car il y aura plusieurs instances créées utilisant la même définition. Si nous utilisons un objet classique pour `data`, le même objet sera **partagé par référence** à toutes les instances créées! En fournissant une fonction `data` , chaque fois qu'une nouvelle instance est créée, nous l'appelons simplement afin de récupérer une copie fraîche des données initiales.

  Si nécessaire, un clône profond de l'objet original peut être obtenu en passant `vm.$data` à travers `JSON.parse(JSON.stringify(...))`.

- **Exemple :**

  ``` js
  var data = { a: 1 }

  // création directe d'instance
  var vm = new Vue({
    data: data
  })
  vm.a // -> 1
  vm.$data === data // -> true

  // data doit être une fonction lorsqu'utilisée dans Vue.extend()
  var Component = Vue.extend({
    data: function () {
      return { a: 1 }
    }
  })
  ```

  <p class="tip">Notez que __vous ne devriez pas utiliser de fonctions fléchées pour la propriété `data`__ (exemple: `data: () => { return { a: this.myProp }}`). La raison est que les fonctions fléchées sont liées au contexte parent, donc `this` ne correspondra pas à l'instance de Vue et  `this.myProp` vaudra `undefined`.</p>

- **Voir aussi :** [Réactivité en détail](../guide/reactivity.html).

### props

- **Type :** `Array<string> | Object`

- **Détails :**

  Une liste ou un objet décrivant les attributs exposés par le composant afin de passer des données depuis le composant parent. Ce paramètre a une syntaxe simple basée sur un tableau (`Array`) et une syntaxe alternative basée sur un `Object` qui permet une configuration avancée telle qu'une vérification de typage, des contrôles de validation personnalisés et des valeurs par défaut.

- **Exemple :**

  ``` js
  // syntaxe simple
  Vue.component('props-demo-simple', {
    props: ['size', 'myMessage']
  })

  // syntaxe avancée avec validation
  Vue.component('props-demo-advanced', {
    props: {
      // juste une vérification de type
      height: Number,
      // vérification du type ainsi que d'autres validations
      age: {
        type: Number,
        default: 0,
        required: true,
        validator: function (value) {
          return value >= 0
        }
      }
    }
  })
  ```

- **Voir aussi :** [Attributs](../guide/components.html#Props)

### propsData

- **Type :** `{ [key: string]: any }`

- **Restriction :** utilisé uniquement si l'instance est créée via `new`.

- **Détails :**

  Passe des valeurs d'attribut à l'instance durant sa création. Cette propriété a pour but principal de faciliter les tests unitaires.

- **Example:**

  ``` js
  var Comp = Vue.extend({
    props: ['msg'],
    template: '<div>{{ msg }}</div>'
  })

  var vm = new Comp({
    propsData: {
      msg: 'hello'
    }
  })
  ```

### computed

- **Type :** `{ [key: string]: Function | { get: Function, set: Function } }`

- **Détails :**

  Les propriétés calculées qui seront ajoutées à l'instance de Vue. Tous les accesseurs (*getters*) et mutateurs (*setters*) ont leur contexte `this` automatiquement lié à l'instance de Vue.

  <p class="tip">Notez que __vous ne devriez pas utiliser de fonctions fléchées pour définir une propriété calculée__ (exemple: `aDouble: () => this.a * 2`). La raison est que les fonctions fléchées sont liées au contexte parent, donc `this` ne correspondra pas à l'instance de Vue et `this.a` vaudra `undefined`.</p>

  Les propriétés calculées sont mises en cache, et réévaluées uniquement lorsque leurs dépendances réactives changent. Notez que si une certaine dépendance est en dehors de la portée de l'instance (et donc non réactive), la propriété calculée ne sera __pas__ mise à jour.

- **Exemple :**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    computed: {
      // accesseur uniquement, on a juste besoin d'une fonction
      aDouble: function () {
        return this.a * 2
      },
      // accesseur et mutateur à la fois
      aPlus: {
        get: function () {
          return this.a + 1
        },
        set: function (v) {
          this.a = v - 1
        }
      }
    }
  })
  vm.aPlus   // -> 2
  vm.aPlus = 3
  vm.a       // -> 2
  vm.aDouble // -> 4
  ```

- **Voir aussi :**
  - [Propriétés calculées](../guide/computed.html)

### methods

- **Type :** `{ [key: string]: Function }`

- **Détails :**

  Les méthodes qui seront ajoutées à l'instance de Vue. Vous pouvez accéder à ces méthodes directement depuis l'instance VM ou les utiliser à travers des expressions de directives. Toutes les méthodes ont leur contexte d'appel `this` automatiquement assigné à l'instance de Vue.
  
  <p class="tip">Notez que __vous ne devriez pas utiliser de fonctions fléchées pour définir une méthode__ (exemple: `plus: () => this.a++`). La raison est que les fonctions fléchées sont liées au contexte parent, donc `this` ne correspondra pas à l'instance de Vue et `this.a` vaudra `undefined`.</p>

- **Exemple :**

  ```js
  var vm = new Vue({
    data: { a: 1 },
    methods: {
      plus: function () {
        this.a++
      }
    }
  })
  vm.plus()
  vm.a // 2
  ```

- **Voir aussi :** [Méthodes et gestion d'évènements](../guide/events.html)

### watch

- **Type :** `{ [key: string]: string | Function | Object }`

- **Détails :**

  Un objet où les clés sont des expressions à surveiller et où la valeur associée est la fonction de callback exécutée quand cette expression change. On parle alors d'observateur ou *watcher* pour décrire ce lien. La valeur peut également être une `String` correspondant au nom d'une méthode de l'instance, ou un objet avec des options avancées. L'instance de Vue appelera `$watch()` pour chaque clé de l'objet à l'initialisation.

- **Exemple :**

  ``` js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3
    },
    watch: {
      a: function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // nom d'une méthode
      b: 'someMethod',
      // observateur profond (deep watcher)
      c: {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      }
    }
  })
  vm.a = 2 // -> new: 2, old: 1
  ```
  
  <p class="tip">Notez que __vous ne devriez pas utiliser de fonctions fléchées pour définir un observateur__ (exemple: `searchQuery: newValue => this.updateAutocomplete(newValue)`). La raison est que les fonctions fléchées sont liées au contexte parent, donc `this` ne correspondra pas à l'instance de Vue et `this.updateAutocomplete` vaudra `undefined`.</p>

- **Voir aussi :** [Méthodes d'instance - vm.$watch](#vm-watch)

## Options / DOM

### el

- **Type :** `string | HTMLElement`

- **Restriction :** uniquement respecté quand l'instance est créée via `new`.

- **Détails :**

  Fournit à l'instance de Vue un élément existant du DOM sur lequel se monter. Cela peut être une `String` représentant un sélecteur CSS ou une référence à un `HTMLElement`.

  Une fois l'instance montée, l'élément correspondant sera accessible via `vm.$el`.

  Si cette option est disponible à l'instanciation, l'instance sera immédiatement compilée; sinon, l'utilisateur devra explicitement appeler `vm.$mount()` pour démarrer manuellement la compilation.

  <p class="tip">L'élément fourni sert seulement de point de montage. Contrairement à Vue 1.x, l'élément monté sera remplacé par le DOM généré par Vue dans tous les cas. C'est pourquoi il n'est pas recommandé de monter l'instance racine sur `<html>` ou `<body>`.</p>

- **Voir aussi :** [Diagramme du Cycle de Vie](../guide/instance.html#Lifecycle-Diagram)

### template

- **Type :** `string`

- **Détails :**

  A string template to be used as the markup for the Vue instance. The template will **replace** the mounted element. Any existing markup inside the mounted element will be ignored, unless content distribution slots are present in the template.

  If the string starts with `#` it will be used as a querySelector and use the selected element's innerHTML as the template string. This allows the use of the common `<script type="x-template">` trick to include templates.

  <p class="tip">From a security perspective, you should only use Vue templates that you can trust. Never use user-generated content as your template.</p>

- **Voir aussi :**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Content Distribution](../guide/components.html#Content-Distribution-with-Slots)

### render

  - **Type :** `Function`

  - **Détails :**

    An alternative to string templates allowing you to leverage the full programmatic power of JavaScript. The render function receives a `createElement` method as it's first argument used to create `VNode`s.

    If the component is a functional component, the render function also receives an extra argument `context`, which provides access to contextual data since functional components are instance-less.

  - **Voir aussi :**
    - [Render Functions](../guide/render-function)

## Options / Lifecycle Hooks

All lifecycle hooks automatically have their `this` context bound to the instance, so that you can access data, computed properties, and methods. This means __you should not use an arrow function to define a lifecycle method__ (e.g. `created: () => this.fetchTodos()`). The reason is arrow functions bind the parent context, so `this` will not be the Vue instance as you expect and `this.fetchTodos` will be undefined.

### beforeCreate

- **Type :** `Function`

- **Détails :**

  Called synchronously after the instance has just been initialized, before data observation and event/watcher setup.

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### created

- **Type :** `Function`

- **Détails :**

  Called synchronously after the instance is created. At this stage, the instance has finished processing the options which means the following have been set up: data observation, computed properties, methods, watch/event callbacks. However, the mounting phase has not been started, and the `$el` property will not be available yet.

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeMount

- **Type :** `Function`

- **Détails :**

  Called right before the mounting begins: the `render` function is about to be called for the first time.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### mounted

- **Type :** `Function`

- **Détails :**

  Called after the instance has just been mounted where `el` is replaced by the newly created `vm.$el`. If the root instance is mounted to an in-document element, `vm.$el` will also be in-document when `mounted` is called.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### beforeUpdate

- **Type :** `Function`

- **Détails :**

  Called when the data changes, before the virtual DOM is re-rendered and patched.

  You can perform further state changes in this hook and they will not trigger additional re-renders.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### updated

- **Type :** `Function`

- **Détails :**

  Called after a data change causes the virtual DOM to be re-rendered and patched.

  The component's DOM will have been updated when this hook is called, so you can perform DOM-dependent operations here. However, in most cases you should avoid changing state inside the hook. To react to state changes, it's usually better to use a [computed property](#computed) or [watcher](#watch) instead.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### activated

- **Type :** `Function`

- **Détails :**

  Called when a kept-alive component is activated.

  **This hook is not called during server-side rendering.**

- **Voir aussi :**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### deactivated

- **Type :** `Function`

- **Détails :**

  Called when a kept-alive component is deactivated.

  **This hook is not called during server-side rendering.**

- **Voir aussi :**
  - [Built-in Components - keep-alive](#keep-alive)
  - [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### beforeDestroy

- **Type :** `Function`

- **Détails :**

  Called right before a Vue instance is destroyed. At this stage the instance is still fully functional.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

### destroyed

- **Type :** `Function`

- **Détails :**

  Called after a Vue instance has been destroyed. When this hook is called, all directives of the Vue instance have been unbound, all event listeners have been removed, and all child Vue instances have also been destroyed.

  **This hook is not called during server-side rendering.**

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

## Options / Assets

### directives

- **Type :** `Object`

- **Détails :**

  A hash of directives to be made available to the Vue instance.

- **Voir aussi :**
  - [Custom Directives](../guide/custom-directive.html)
  - [Assets Naming Convention](../guide/components.html#Assets-Naming-Convention)

### filters

- **Type :** `Object`

- **Détails :**

  A hash of filters to be made available to the Vue instance.

- **Voir aussi :**
  - [`Vue.filter`](#Vue-filter)

### components

- **Type :** `Object`

- **Détails :**

  A hash of components to be made available to the Vue instance.

- **Voir aussi :**
  - [Components](../guide/components.html)

## Options / Misc

### parent

- **Type :** `Vue instance`

- **Détails :**

  Specify the parent instance for the instance to be created. Establishes a parent-child relationship between the two. The parent will be accessible as `this.$parent` for the child, and the child will be pushed into the parent's `$children` array.

  <p class="tip">Use `$parent` and `$children` sparingly - they mostly serve as an escape-hatch. Prefer using props and events for parent-child communication.</p>

### mixins

- **Type :** `Array<Object>`

- **Détails :**

  The `mixins` option accepts an array of mixin objects. These mixin objects can contain instance options just like normal instance objects, and they will be merged against the eventual options using the same option merging logic in `Vue.extend()`. e.g. If your mixin contains a created hook and the component itself also has one, both functions will be called.

  Mixin hooks are called in the order they are provided, and called before the component's own hooks.

- **Example:**

  ``` js
  var mixin = {
    created: function () { console.log(1) }
  }
  var vm = new Vue({
    created: function () { console.log(2) },
    mixins: [mixin]
  })
  // -> 1
  // -> 2
  ```

- **Voir aussi :** [Mixins](../guide/mixins.html)

### name

- **Type :** `string`

- **Restriction :** only respected when used as a component option.

- **Détails :**

  Allow the component to recursively invoke itself in its template. Note that when a component is registered globally with `Vue.component()`, the global ID is automatically set as its name.

  Another benefit of specifying a `name` option is debugging. Named components result in more helpful warning messages. Also, when inspecting an app in the [vue-devtools](https://github.com/vuejs/vue-devtools), unnamed components will show up as `<AnonymousComponent>`, which isn't very informative. By providing the `name` option, you will get a much more informative component tree.

### extends

- **Type :** `Object | Function`

- **Détails :**

  Allows declaratively extending another component (could be either a plain options object or a constructor) without having to use `Vue.extend`. This is primarily intended to make it easier to extend between single file components.

  This is similar to `mixins`, the difference being that the component's own options takes higher priority than the source component being extended.

- **Example:**

  ``` js
  var CompA = { ... }

  // extend CompA without having to call Vue.extend on either
  var CompB = {
    extends: CompA,
    ...
  }
  ```

### delimiters

- **Type :** `Array<string>`

- **default:** `{% raw %}["{{", "}}"]{% endraw %}`

- **Détails :**

  Change the plain text interpolation delimiters. **This option is only available in the standalone build.**

- **Example:**

  ``` js
  new Vue({
    delimiters: ['${', '}']
  })

  // Delimiters changed to ES6 template string style
  ```

### functional

- **Type :** `boolean`

- **Détails :**

  Causes a component to be stateless (no `data`) and instanceless (no `this` context). They are simply a `render` function that returns virtual nodes making them much cheaper to render.

- **Voir aussi :** [Functional Components](../guide/render-function.html#Functional-Components)

## Instance Properties

### vm.$data

- **Type :** `Object`

- **Détails :**

  The data object that the Vue instance is observing. The Vue instance proxies access to the properties on its data object.

- **Voir aussi :** [Options - data](#data)

### vm.$el

- **Type :** `HTMLElement`

- **Read only**

- **Détails :**

  The root DOM element that the Vue instance is managing.

### vm.$options

- **Type :** `Object`

- **Read only**

- **Détails :**

  The instantiation options used for the current Vue instance. This is useful when you want to include custom properties in the options:

  ``` js
  new Vue({
    customOption: 'foo',
    created: function () {
      console.log(this.$options.customOption) // -> 'foo'
    }
  })
  ```

### vm.$parent

- **Type :** `Vue instance`

- **Read only**

- **Détails :**

  The parent instance, if the current instance has one.

### vm.$root

- **Type :** `Vue instance`

- **Read only**

- **Détails :**

  The root Vue instance of the current component tree. If the current instance has no parents this value will be itself.

### vm.$children

- **Type :** `Array<Vue instance>`

- **Read only**

- **Détails :**

  The direct child components of the current instance. **Note there's no order guarantee for `$children`, and it is not reactive.** If you find yourself trying to use `$children` for data binding, consider using an Array and `v-for` to generate child components, and use the Array as the source of truth.

### vm.$slots

- **Type :** `{ [name: string]: ?Array<VNode> }`

- **Read only**

- **Détails :**

  Used to programmatically access content [distributed by slots](../guide/components.html#Content-Distribution-with-Slots). Each [named slot](../guide/components.html#Named-Slots) has its own corresponding property (e.g. the contents of `slot="foo"` will be found at `vm.$slots.foo`). The `default` property contains any nodes not included in a named slot.

  Accessing `vm.$slots` is most useful when writing a component with a [render function](../guide/render-function.html).

- **Example:**

  ```html
  <blog-post>
    <h1 slot="header">
      About Me
    </h1>

    <p>Here's some page content, which will be included in vm.$slots.default, because it's not inside a named slot.</p>

    <p slot="footer">
      Copyright 2016 Evan You
    </p>

    <p>If I have some content down here, it will also be included in vm.$slots.default.</p>.
  </blog-post>
  ```

  ```js
  Vue.component('blog-post', {
    render: function (createElement) {
      var header = this.$slots.header
      var body   = this.$slots.default
      var footer = this.$slots.footer
      return createElement('div', [
        createElement('header', header),
        createElement('main', body),
        createElement('footer', footer)
      ])
    }
  })
  ```

- **Voir aussi :**
  - [`<slot>` Component](#slot-1)
  - [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)
  - [Render Functions: Slots](../guide/render-function.html#Slots)

### vm.$scopedSlots

> New in 2.1.0

- **Type :** `{ [name: string]: props => VNode | Array<VNode> }`

- **Read only**

- **Détails :**

  Used to programmatically access [scoped slots](../guide/components.html#Scoped-Slots). For each slot, including the `default` one, the object contains a corresponding function that returns VNodes.

  Accessing `vm.$scopedSlots` is most useful when writing a component with a [render function](../guide/render-function.html).

- **Voir aussi :**
  - [`<slot>` Component](#slot-1)
  - [Scoped Slots](../guide/components.html#Scoped-Slots)
  - [Render Functions: Slots](../guide/render-function.html#Slots)

### vm.$refs

- **Type :** `Object`

- **Read only**

- **Détails :**

  An object that holds child components that have `ref` registered.

- **Voir aussi :**
  - [Child Component Refs](../guide/components.html#Child-Component-Refs)
  - [ref](#ref)

### vm.$isServer

- **Type :** `boolean`

- **Read only**

- **Détails :**

  Whether the current Vue instance is running on the server.

- **Voir aussi :** [Server-Side Rendering](../guide/ssr.html)

## Instance Methods / Data

<h3 id="vm-watch">vm.$watch( expOrFn, callback, [options] )</h3>

- **Arguments :**
  - `{string | Function} expOrFn`
  - `{Function} callback`
  - `{Object} [options]`
    - `{boolean} deep`
    - `{boolean} immediate`

- **Retourne :** `{Function} unwatch`

- **Utilisation :**

  Watch an expression or a computed function on the Vue instance for changes. The callback gets called with the new value and the old value. The expression only accepts simple dot-delimited paths. For more complex expression, use a function instead.

<p class="tip">Note: when mutating (rather than replacing) an Object or an Array, the old value will be the same as new value because they reference the same Object/Array. Vue doesn't keep a copy of the pre-mutate value.</p>

- **Example:**

  ``` js
  // keypath
  vm.$watch('a.b.c', function (newVal, oldVal) {
    // do something
  })

  // function
  vm.$watch(
    function () {
      return this.a + this.b
    },
    function (newVal, oldVal) {
      // do something
    }
  )
  ```

  `vm.$watch` returns an unwatch function that stops firing the callback:

  ``` js
  var unwatch = vm.$watch('a', cb)
  // later, teardown the watcher
  unwatch()
  ```

- **Option: deep**

  To also detect nested value changes inside Objects, you need to pass in `deep: true` in the options argument. Note that you don't need to do so to listen for Array mutations.

  ``` js
  vm.$watch('someObject', callback, {
    deep: true
  })
  vm.someObject.nestedValue = 123
  // callback is fired
  ```

- **Option: immediate**

  Passing in `immediate: true` in the option will trigger the callback immediately with the current value of the expression:

  ``` js
  vm.$watch('a', callback, {
    immediate: true
  })
  // callback is fired immediately with current value of `a`
  ```

<h3 id="vm-set">vm.$set( object, key, value )</h3>

- **Arguments :**
  - `{Object} object`
  - `{string} key`
  - `{any} value`

- **Retourne :** the set value.

- **Utilisation :**

  This is the **alias** of the global `Vue.set`.

- **Voir aussi :** [Vue.set](#Vue-set)

<h3 id="vm-delete">vm.$delete( object, key )</h3>

- **Arguments :**
  - `{Object} object`
  - `{string} key`

- **Utilisation :**

  This is the **alias** of the global `Vue.delete`.

- **Voir aussi :** [Vue.delete](#Vue-delete)

## Instance Methods / Events

<h3 id="vm-on">vm.$on( event, callback )</h3>

- **Arguments :**
  - `{string} event`
  - `{Function} callback`

- **Utilisation :**

  Listen for a custom event on the current vm. Events can be triggered by `vm.$emit`. The callback will receive all the additional arguments passed into these event-triggering methods.

- **Example:**

  ``` js
  vm.$on('test', function (msg) {
    console.log(msg)
  })
  vm.$emit('test', 'hi')
  // -> "hi"
  ```

<h3 id="vm-once">vm.$once( event, callback )</h3>

- **Arguments :**
  - `{string} event`
  - `{Function} callback`

- **Utilisation :**

  Listen for a custom event, but only once. The listener will be removed once it triggers for the first time.

<h3 id="vm-off">vm.$off( [event, callback] )</h3>

- **Arguments :**
  - `{string} [event]`
  - `{Function} [callback]`

- **Utilisation :**

  Remove event listener(s).

  - If no arguments are provided, remove all event listeners;

  - If only the event is provided, remove all listeners for that event;

  - If both event and callback are given, remove the listener for that specific callback only.

<h3 id="vm-emit">vm.$emit( event, [...args] )</h3>

- **Arguments :**
  - `{string} event`
  - `[...args]`

  Trigger an event on the current instance. Any additional arguments will be passed into the listener's callback function.

## Instance Methods / Lifecycle

<h3 id="vm-mount">vm.$mount( [elementOrSelector] )</h3>

- **Arguments :**
  - `{Element | string} [elementOrSelector]`
  - `{boolean} [hydrating]`

- **Retourne :** `vm` - the instance itself

- **Utilisation :**

  If a Vue instance didn't receive the `el` option at instantiation, it will be in "unmounted" state, without an associated DOM element. `vm.$mount()` can be used to manually start the mounting of an unmounted Vue instance.

  If `elementOrSelector` argument is not provided, the template will be rendered as an off-document element, and you will have to use native DOM API to insert it into the document yourself.

  The method returns the instance itself so you can chain other instance methods after it.

- **Example:**

  ``` js
  var MyComponent = Vue.extend({
    template: '<div>Hello!</div>'
  })

  // create and mount to #app (will replace #app)
  new MyComponent().$mount('#app')

  // the above is the same as:
  new MyComponent({ el: '#app' })

  // or, render off-document and append afterwards:
  var component = new MyComponent().$mount()
  document.getElementById('app').appendChild(component.$el)
  ```

- **Voir aussi :**
  - [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)
  - [Server-Side Rendering](../guide/ssr.html)

<h3 id="vm-forceUpdate">vm.$forceUpdate()</h3>

- **Utilisation :**

  Force the Vue instance to re-render. Note it does not affect all child components, only the instance itself and child components with inserted slot content.

<h3 id="vm-nextTick">vm.$nextTick( [callback] )</h3>

- **Arguments :**
  - `{Function} [callback]`

- **Utilisation :**

  Defer the callback to be executed after the next DOM update cycle. Use it immediately after you've changed some data to wait for the DOM update. This is the same as the global `Vue.nextTick`, except that the callback's `this` context is automatically bound to the instance calling this method.

  > New in 2.1.0: returns a Promise if no callback is provided and Promise is supported in the execution environment.

- **Example:**

  ``` js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // modify data
        this.message = 'changed'
        // DOM is not updated yet
        this.$nextTick(function () {
          // DOM is now updated
          // `this` is bound to the current instance
          this.doSomethingElse()
        })
      }
    }
  })
  ```

- **Voir aussi :**
  - [Vue.nextTick](#Vue-nextTick)
  - [Async Update Queue](../guide/reactivity.html#Async-Update-Queue)

<h3 id="vm-destroy">vm.$destroy()</h3>

- **Utilisation :**

  Completely destroy a vm. Clean up its connections with other existing vms, unbind all its directives, turn off all event listeners.

  Triggers the `beforeDestroy` and `destroyed` hooks.

  <p class="tip">In normal use cases you shouldn't have to call this method yourself. Prefer controlling the lifecycle of child components in a data-driven fashion using `v-if` and `v-for`.</p>

- **Voir aussi :** [Lifecycle Diagram](../guide/instance.html#Lifecycle-Diagram)

## Directives

### v-text

- **Expects:** `string`

- **Détails :**

  Updates the element's `textContent`. If you need to update the part of `textContent`, you should use `{% raw %}{{ Mustache }}{% endraw %}` interpolations.

- **Example:**

  ```html
  <span v-text="msg"></span>
  <!-- same as -->
  <span>{{msg}}</span>
  ```

- **Voir aussi :** [Data Binding Syntax - interpolations](../guide/syntax.html#Text)

### v-html

- **Expects:** `string`

- **Détails :**

  Updates the element's `innerHTML`. **Note that the contents are inserted as plain HTML - they will not be compiled as Vue templates**. If you find yourself trying to compose templates using `v-html`, try to rethink the solution by using components instead.

  <p class="tip">Dynamically rendering arbitrary HTML on your website can be very dangerous because it can easily lead to [XSS attacks](https://en.wikipedia.org/wiki/Cross-site_scripting). Only use `v-html` on trusted content and **never** on user-provided content.</p>

- **Example:**

  ```html
  <div v-html="html"></div>
  ```
- **Voir aussi :** [Data Binding Syntax - interpolations](../guide/syntax.html#Raw-HTML)

### v-show

- **Expects:** `any`

- **Utilisation :**

  Toggle's the element's `display` CSS property based on the truthy-ness of the expression value.

  This directive triggers transitions when its condition changes.

- **Voir aussi :** [Conditional Rendering - v-show](../guide/conditional.html#v-show)

### v-if

- **Expects:** `any`

- **Utilisation :**

  Conditionally render the element based on the truthy-ness of the expression value. The element and its contained directives / components are destroyed and re-constructed during toggles. If the element is a `<template>` element, its content will be extracted as the conditional block.

  This directive triggers transitions when its condition changes.

<p class="tip">Quand utilisé avec v-if, v-for a une plus grande priorité par rapport à v-if. Voir le <a href="../guide/list.html#v-for-with-v-if">guide sur le rendu de listes</a> pour plus de détails.</p>

- **Voir aussi :** [Conditional Rendering - v-if](../guide/conditional.html)

### v-else

- **Does not expect expression**

- **Restriction :** previous sibling element must have `v-if` or `v-else-if`.

- **Utilisation :**

  Denote the "else block" for `v-if` or a `v-if`/`v-else-if` chain.

  ```html
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **Voir aussi :**
  - [Conditional Rendering - v-else](../guide/conditional.html#v-else)

### v-else-if

> New in 2.1.0

- **Expects:** `any`

- **Restriction :** previous sibling element must have `v-if` or `v-else-if`.

- **Utilisation :**

  Denote the "else if block" for `v-if`. Can be chained.

  ```html
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- **Voir aussi :** [Conditional Rendering - v-else-if](../guide/conditional.html#v-else-if)

### v-for

- **Expects:** `Array | Object | number | string`

- **Utilisation :**

  Render the element or template block multiple times based on the source data. The directive's value must use the special syntax `alias in expression` to provide an alias for the current element being iterated on:

  ``` html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatively, you can also specify an alias for the index (or the key if used on an Object):

  ``` html
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, key, index) in object"></div>
  ```

  The default behavior of `v-for` will try to patch the elements in-place without moving them. To force it to reorder elements, you need to provide an ordering hint with the `key` special attribute:

  ``` html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  <p class="tip">When used together with v-if, v-for has a higher priority than v-if. See the <a href="../guide/list.html#v-for-with-v-if">list rendering guide</a> for details.</p>

  The detailed usage for `v-for` is explained in the guide section linked below.

- **Voir aussi :**
  - [List Rendering](../guide/list.html)
  - [key](../guide/list.html#key)

### v-on

- **Shorthand:** `@`

- **Expects:** `Function | Inline Statement`

- **Argument:** `event (required)`

- **Modifiers:**
  - `.stop` - call `event.stopPropagation()`.
  - `.prevent` - call `event.preventDefault()`.
  - `.capture` - add event listener in capture mode.
  - `.self` - only trigger handler if event was dispatched from this element.
  - `.{keyCode | keyAlias}` - only trigger handler on certain keys.
  - `.native` - listen for a native event on the root element of component.
  - `.once` - trigger handler at most once.

- **Utilisation :**

  Attaches an event listener to the element. The event type is denoted by the argument. The expression can either be a method name or an inline statement, or simply omitted when there are modifiers present.

  When used on a normal element, it listens to **native DOM events** only. When used on a custom element component, it also listens to **custom events** emitted on that child component.

  When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.

- **Example:**

  ```html
  <!-- method handler -->
  <button v-on:click="doThis"></button>

  <!-- inline statement -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>

  <!-- stop propagation -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <input @keyup.enter="onEnter">

  <!-- key modifier using keyCode -->
  <input @keyup.13="onEnter">

  <!-- the click event will be triggered at most once -->
  <button v-on:click.once="doThis"></button>
  ```

  Listening to custom events on a child component (the handler is called when "my-event" is emitted on the child):

  ```html
  <my-component @my-event="handleThis"></my-component>

  <!-- inline statement -->
  <my-component @my-event="handleThis(123, $event)"></my-component>

  <!-- native event on component -->
  <my-component @click.native="onClick"></my-component>
  ```

- **Voir aussi :**
  - [Methods and Event Handling](../guide/events.html)
  - [Components - Custom Events](../guide/components.html#Custom-Events)

### v-bind

- **Shorthand:** `:`

- **Expects:** `any (with argument) | Object (without argument)`

- **Argument:** `attrOrProp (optional)`

- **Modifiers:**
  - `.prop` - Bind as a DOM property instead of an attribute. ([what's the difference?](http://stackoverflow.com/questions/6003819/properties-and-attributes-in-html#answer-6004028))
  - `.camel` - transform the kebab-case attribute name into camelCase. (supported since 2.1.0)

- **Utilisation :**

  Dynamically bind one or more attributes, or a component prop to an expression.

  When used to bind the `class` or `style` attribute, it supports additional value types such as Array or Objects. See linked guide section below for more details.

  When used for prop binding, the prop must be properly declared in the child component.

  When used without an argument, can be used to bind an object containing attribute name-value pairs. Note in this mode `class` and `style` does not support Array or Objects.

- **Example:**

  ```html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc">

  <!-- shorthand -->
  <img :src="imageSrc">

  <!-- with inline string concatenation -->
  <img :src="'/path/to/images/' + fileName">

  <!-- class binding -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">

  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- DOM attribute binding with prop modifier -->
  <div v-bind:text-content.prop="text"></div>

  <!-- prop binding. "prop" must be declared in my-component. -->
  <my-component :prop="someThing"></my-component>

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  The `.camel` modifier allows camelizing a `v-bind` attribute name when using in-DOM templates, e.g. the SVG `viewBox` attribute:

  ``` html
  <svg :view-box.camel="viewBox"></svg>
  ```

  `.camel` is not needed if you are using string templates, or compiling with `vue-loader`/`vueify`.

- **Voir aussi :**
  - [Class and Style Bindings](../guide/class-and-style.html)
  - [Components - Component Props](../guide/components.html#Props)

### v-model

- **Expects:** varies based on value of form inputs element or output of components

- **Limited to:**
  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **Modifiers:**
  - [`.lazy`](../guide/forms.html#lazy) - listen to `change` events instead of `input`
  - [`.number`](../guide/forms.html#number) - cast input string to numbers
  - [`.trim`](../guide/forms.html#trim) - trim input

- **Utilisation :**

  Create a two-way binding on a form input element or a component. For detailed usage and other notes, see the Guide section linked below.

- **Voir aussi :**
  - [Form Input Bindings](../guide/forms.html)
  - [Components - Form Input Components using Custom Events](../guide/components.html#Form-Input-Components-using-Custom-Events)

### v-pre

- **Does not expect expression**

- **Utilisation :**

  Skip compilation for this element and all its children. You can use this for displaying raw mustache tags. Skipping large numbers of nodes with no directives on them can also speed up compilation.

- **Example:**

  ```html
  <span v-pre>{{ this will not be compiled }}</span>
   ```

### v-cloak

- **Does not expect expression**

- **Utilisation :**

  This directive will remain on the element until the associated Vue instance finishes compilation. Combined with CSS rules such as `[v-cloak] { display: none }`, this directive can be used to hide un-compiled mustache bindings until the Vue instance is ready.

- **Example:**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  The `<div>` will not be visible until the compilation is done.

### v-once

- **Does not expect expression**

- **Détails :**

  Render the element and component **once** only. On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. This can be used to optimize update performance.

  ```html
  <!-- single element -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- the element have children -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- component -->
  <my-component v-once :comment="msg"></my-component>
  <!-- v-for directive -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

- **Voir aussi :**
  - [Data Binding Syntax - interpolations](../guide/syntax.html#Text)
  - [Components - Cheap Static Components with v-once](../guide/components.html#Cheap-Static-Components-with-v-once)

## Special Attributes

### key

- **Expects:** `string`

  The `key` special attribute is primarily used as a hint for Vue's virtual DOM algorithm to identify VNodes when diffing the new list of nodes against the old list. Without keys, Vue uses an algorithm that minimizes element movement and tries to patch/reuse elements of the same type in-place as much as possible. With keys, it will reorder elements based on the order change of keys, and elements with keys that are no longer present will always be removed/destroyed.

  Children of the same common parent must have **unique keys**. Duplicate keys will cause render errors.

  The most common use case is combined with `v-for`:

  ``` html
  <ul>
    <li v-for="item in items" :key="item.id">...</li>
  </ul>
  ```

  It can also be used to force replacement of an element/component instead of reusing it. This can be useful when you want to:

  - Properly trigger lifecycle hooks of a component
  - Trigger transitions

  For example:

  ``` html
  <transition>
    <span :key="text">{{ text }}</span>
  </transition>
  ```

  When `text` changes, the `<span>` will always be replaced instead of patched, so a transition will be triggered.

### ref

- **Expects:** `string`

  `ref` is used to register a reference to an element or a child component. The reference will be registered under the parent component's `$refs` object. If used on a plain DOM element, the reference will be that element; if used on a child component, the reference will be component instance:

  ``` html
  <!-- vm.$refs.p will be the DOM node -->
  <p ref="p">hello</p>

  <!-- vm.$refs.child will be the child comp instance -->
  <child-comp ref="child"></child-comp>
  ```

  When used on elements/components with `v-for`, the registered reference will be an Array containing DOM nodes or component instances.

  An important note about the ref registration timing: because the refs themselves are created as a result of the render function, you cannot access them on the initial render - they don't exist yet! `$refs` is also non-reactive, therefore you should not attempt to use it in templates for data-binding.

- **Voir aussi :** [Child Component Refs](../guide/components.html#Child-Component-Refs)

### slot

- **Expects:** `string`

  Used on content inserted into child components to indicate which named slot the content belongs to.

  For detailed usage, see the guide section linked below.

- **Voir aussi :** [Named Slots](../guide/components.html#Named-Slots)

## Built-In Components

### component

- **Props:**
  - `is` - string | ComponentDefinition | ComponentConstructor
  - `inline-template` - boolean

- **Utilisation :**

  A "meta component" for rendering dynamic components. The actual component to render is determined by the `is` prop:

  ```html
  <!-- a dynamic component controlled by -->
  <!-- the `componentId` property on the vm -->
  <component :is="componentId"></component>

  <!-- can also render registered component or component passed as prop -->
  <component :is="$options.components.child"></component>
  ```

- **Voir aussi :** [Dynamic Components](../guide/components.html#Dynamic-Components)

### transition

- **Props:**
  - `name` - string, Used to automatically generate transition CSS class names. e.g. `name: 'fade'` will auto expand to `.fade-enter`, `.fade-enter-active`, etc. Defaults to `"v"`.
  - `appear` - boolean, Whether to apply transition on initial render. Defaults to `false`.
  - `css` - boolean, Whether to apply CSS transition classes. Defaults to `true`. If set to `false`, will only trigger JavaScript hooks registered via component events.
  - `type` - string, Specify the type of transition events to wait for to determine transition end timing. Available values are `"transition"` and `"animation"`. By default, it will automatically detect the type that has a longer duration.
  - `mode` - string, Controls the timing sequence of leaving/entering transitions. Available modes are `"out-in"` and `"in-out"`; defaults to simultaneous.
  - `enter-class` - string
  - `leave-class` - string
  - `appear-class` - string
  - `enter-to-class` - string
  - `leave-to-class` - string
  - `appear-to-class` - string
  - `enter-active-class` - string
  - `leave-active-class` - string
  - `appear-active-class` - string

- **Events:**
  - `before-enter`
  - `before-leave`
  - `before-appear`
  - `enter`
  - `leave`
  - `appear`
  - `after-enter`
  - `after-leave`
  - `after-appear`
  - `enter-cancelled`
  - `leave-cancelled` (`v-show` only)
  - `appear-cancelled`

- **Utilisation :**

  `<transition>` serve as transition effects for **single** element/component. The `<transition>` does not render an extra DOM element, nor does it show up in the inspected component hierarchy. It simply applies the transition behavior to the wrapped content inside.

  ```html
  <!-- simple element -->
  <transition>
    <div v-if="ok">toggled content</div>
  </transition>

  <!-- dynamic component -->
  <transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </transition>

  <!-- event hooking -->
  <div id="transition-demo">
    <transition @after-enter="transitionComplete">
      <div v-show="ok">toggled content</div>
    </transition>
  </div>
  ```

  ``` js
  new Vue({
    ...
    methods: {
      transitionComplete: function (el) {
        // for passed 'el' that DOM element as the argument, something ...
      }
    }
    ...
  }).$mount('#transition-demo')
  ```

- **Voir aussi :** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### transition-group

- **Props:**
  - `tag` - string, defaults to `span`.
  - `move-class` - overwrite CSS class applied during moving transition.
  - exposes the same props as `<transition>` except `mode`.

- **Events:**
  - exposes the same events as `<transition>`.

- **Utilisation :**

  `<transition-group>` serve as transition effects for **multiple** elements/components. The `<transition-group>` renders a real DOM element. By default it renders a `<span>`, and you can configure what element is should render via the `tag` attribute.

  Note every child in a `<transition-group>` must be **uniquely keyed** for the animations to work properly.

  `<transition-group>` supports moving transitions via CSS transform. When a child's position on screen has changed after an updated, it will get applied a moving CSS class (auto generated from the `name` attribute or configured with the `move-class` attribute). If the CSS `transform` property is "transition-able" when the moving class is applied, the element will be smoothly animated to its destination using the [FLIP technique](https://aerotwist.com/blog/flip-your-animations/).

  ```html
  <transition-group tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </transition-group>
  ```

- **Voir aussi :** [Transitions: Entering, Leaving, and Lists](../guide/transitions.html)

### keep-alive

- **Props:**
  - `include` - string or RegExp. Only components matched by this will be cached.
  - `exclude` - string or RegExp. Any component matched by this will not be cached.

- **Utilisation :**

  When wrapped around a dynamic component, `<keep-alive>` caches the inactive component instances without destroying them. Similar to `<transition>`, `<keep-alive>` is an abstract component: it doesn't render a DOM element itself, and doesn't show up in the component parent chain.

  When a component is toggled inside `<keep-alive>`, its `activated` and `deactivated` lifecycle hooks will be invoked accordingly.

  Primarily used with preserve component state or avoid re-rendering.

  ```html
  <!-- basic -->
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>

  <!-- multiple conditional children -->
  <keep-alive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </keep-alive>

  <!-- used together with <transition> -->
  <transition>
    <keep-alive>
      <component :is="view"></component>
    </keep-alive>
  </transition>
  ```

- **`include` and `exclude`**

  > New in 2.1.0

  The `include` and `exclude` props allow components to be conditionally cached. Both props can either be a comma-delimited string or a RegExp:

  ``` html
  <!-- comma-delimited string -->
  <keep-alive include="a,b">
    <component :is="view"></component>
  </keep-alive>

  <!-- regex (use v-bind) -->
  <keep-alive :include="/a|b/">
    <component :is="view"></component>
  </keep-alive>
  ```

  The match is first checked on the component's own `name` option, then its local registration name (the key in the parent's `components` option) if the `name` option is not available. Anonymous components cannot be matched against.

  <p class="tip">`<keep-alive>` does not work with functional components because they do not have instances to be cached.</p>

- **Voir aussi :** [Dynamic Components - keep-alive](../guide/components.html#keep-alive)

### slot

- **Props:**
  - `name` - string, Used for named slot.

- **Utilisation :**

  `<slot>` serve as content distribution outlets in component templates. `<slot>` itself will be replaced.

  For detailed usage, see the guide section linked below.

- **Voir aussi :** [Content Distribution with Slots](../guide/components.html#Content-Distribution-with-Slots)

## VNode Interface

- Please refer to the [VNode class declaration](https://github.com/vuejs/vue/blob/dev/src/core/vdom/vnode.js).

## Server-Side Rendering

- Please refer to the [vue-server-renderer package documentation](https://github.com/vuejs/vue/tree/dev/packages/vue-server-renderer).
