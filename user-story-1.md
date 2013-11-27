## 1. Show the content of a game
> **Goal**: _As a user, I want to see the content of a game_

_**Keywords**: custom element, template, binding, filter function_

1. Create a new Polymer application named `game_store_codelab` and explore it
  - Fill the _Application name_ and select _Web application (using the polymer library)_  
    ![Project creation](/docs/img/project-creation.png)
  - `pubspec.yaml` includes a new dependency
  
    ```YAML
    dependencies:
      polymer: any
    ```
  - `build.dart` is runned after a file is saved, and displays Polymer warnings from the linter
  - `clickcounter.html` and `clickcounter.dart` is a custom element named `click-counter`
  
    ```HTML
    <polymer-element name="click-counter" attributes="count">
      <template>
        <!-- Custom element body -->
      </template>
      <script type="application/dart" src="clickcounter.dart"></script>
    </polymer-element>
    ```
    
    ```Dart
    import 'package:polymer/polymer.dart';

    @CustomTag('click-counter')
    class ClickCounter extends PolymerElement {
      ClickCounter.created() : super.created();
      // ...
    }
    ```
  - `game_store_codelab.html` imports `click-counter` element to used it and initializes Dart and Polymer
  
    ```HTML
    <head>
      <!-- import the click-counter -->
      <link rel="import" href="clickcounter.html">
      <script type="application/dart">export 'package:polymer/init.dart';</script>
      <script src="packages/browser/dart.js"></script>
    </head>
    
    <body>   
      <click-counter count="5"></click-counter>
    </body>
    ```

2. Copy all files from the _[template](./template)_ folder into the _web_ directory of your project
3. Create a new custom element `x-game`
  - Create `game.html` and `game.dart` files taking `click-counter` element as an example
  - Copy the `GAME_TEMPLATE` html blocks from the templates into the body of your custom element
  - Import and use it in your `index.html` file

    ```HTML
    <section class="container">
      <x-game></x-game>
    </section>
    ```
  - Run `index.html` in Dartium and you should see something like this ([Hints](#user-story-1-hints)):  
    ![x-game first import](docs/img/x-game-first-import-style.png)
4. Congrats! You created your first custom element! But it's all static, let's do some data bindings :)
  - Create a file `models.dart` with the class `Game`:
    ```Dart
    library game_store.model;
    
    class Game {
      int id;
      String name;
      String genre;
      String description;
      String image;
      int rating;
      
      // Constructors
      Game(this.id, this.name, this.genre, this.description, this.image, this.rating);
      Game.sample() : this(null, "Game name", "Game genre", "Game description", "chess.jpg", 4);
    }
    ```
  - Add a `game` attribute in the `x-game` class:

    ```Dart
    import 'package:polymer/polymer.dart';
    import 'models.dart';
    
    @CustomTag('x-game')
    class XGame extends PolymerElement {
      Game game = new Game.sample();
      // ...
    }
    ```
  - Bind `game` fields into the `x-game` template ([Hints](#user-story-1-hints)):
    - Game name should be uppercased
    - Rating should be transformed into &#9733; characters (`"\u2605"`)

    ![x-game binding](docs/img/x-game-binding.png)

<a name="user-story-1-hints"></a>
> **Hints:**
> 
> - Don't forget to add needed tags in `index.html` header
> - To apply styles from the document to the contents of a custom element, add this getter in its dart class: `bool get applyAuthorStyles => true;`
> - Use a filter function to uppercase the game name, defined in the custom element class (See [Polymer expressions][5])
> - Use also a filter function to transform the rating integer to &#9733; characters (See [List.generate](https://api.dartlang.org/docs/channels/stable/latest/dart_core/List.html#generate) and [List.join](https://api.dartlang.org/docs/channels/stable/latest/dart_core/List.html#join))