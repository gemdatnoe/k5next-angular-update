## Installation

* Theme mittels npm `npm install k5next-theme-v2` installieren.
* Folgenden Code in die globale styles.scss hinzufügen:
  ```scss
  @import '@gemdatnoe/angular-material-theme/src/styling/theme';
  
## Custom Angular Theme erstellen
In Angular kann ein Custom Theme erstellen werden, um das Erscheinungsbild der Anwendung zu steuern.
Folgende Bereiche können mit einem Custom Theme definiert werden:

* **Palette:** Eine Custom Palette kann erstellt werden, indem eine Sass-Map definiert wird.
Diese Map sollte Hues für 50 und für jeden Hunderterschritt zwischen 100 und 900 enthalten. Darüber hinaus muss eine Kontrast-Map festlegt werden, die Kontrastfarben für jede Hue angibt. Der [Material palette generator](https://material.io/design/color/the-color-system.html#tools-for-picking-colors) kann verwendet werden, um bei der Auswahl der Hues zu helfen.

  ```scss
  $k5next-petrol: (
    50: #e3edf0,
    100: #bad3d9,
    200: #8cb5c0,
    300: #5d97a6,
    400: #3b8193,
    500: #186b80,
    600: #156378,
    700: #11586d,
    800: #0e4e63,
    900: #083c50,
    A100: #85d7ff,
    A200: #52c7ff,
    A400: #1fb7ff,
    A700: #05aeff,
    contrast: (
      50: #000000,
      100: #000000,
      200: #000000,
      300: #000000,
      400: #ffffff,
      500: #ffffff,
      600: #ffffff,
      700: #ffffff,
      800: #ffffff,
      900: #ffffff,
      A100: #000000,
      A200: #000000,
      A400: #000000,
      A700: #000000,
    ),
  );
  ```

* **Farben:** Die Primär-, Akzent- und Warnfarbe können definiert werden: 
  ```scss
  $custom-primary: mat.define-palette($k5next-petrol);
  $custom-accent: mat.define-palette(mat.$grey-palette);
  $custom-warn: mat.define-palette(mat.$red-palette);

  $custom-theme: mat-light-theme($custom-primary, $custom-accent, $custom-warn);

  // Verwendung des Themes in Angular Material-Komponenten
  @include angular-material-theme($custom-theme);
  ``` 
* **Typography:** Schriftart und -größe können definiert werden:
  ```scss
  $custom-typography: mat.define-typography-config(
    $font-family: "Source Sans Pro, sans-serif",
  );

  $custom-theme: mat-light-theme($custom-primary, $custom-accent, $custom-warn, $custom-typography);

  // Verwendung des Themes in Angular Material-Komponenten
  @include angular-material-theme($custom-theme);
  ```

* **Component-Sytle:** Custom Styles für individuelle Komponenten können definiert werden:
  ```scss
  // Beispiel MatButton-Komponente
  .mat-button {
    background-color: $custom-primary;
    color: white;

    &:hover {
      background-color: darken($custom-primary, 10%);
    }
  }
  ```

* **Global Styling:** Global Styles, die auf die gesamte Anwendung angewendet werden, können definiert werden:
  ```scss
  body {
    font-family: 'Roboto, sans-serif';
    background-color: $custom-background;
    color: $custom-text;
  }
  ```

Um ein Custom Angular Theme zu erstellen, muss in der Regel eine SCSS-Datei erstellen werden, die die gewünschten Styles enthält. 
Die Farben und Typography können von der Main SCSS-Datei getrennt werden.
Jedoch müssen sie in diese importiert werden, damit in der Main UI nur eine Datei importiert werden kann.

### Mixins
Angular Material bietet nützliche Mixins für die Definition von Farben, Schriften usw. an. 
Mixins sind wiederverwendbare Code-Snippets in SCSS, die in verschiedene Selektoren eingefügt werden können. 
Sie werden durch den Befehl `@mixin` definiert und durch `@include` in anderen Selektoren verwendet. 
Über `@mixin` wird in diesem Fall das Custom Theme definiert. 
Über `@include`  wird das Theme in die `styles.scss` in der Main UI inkludiert (siehe [Installation](#installation)).
  

## Custom Angular Theme sharen
Um die Custom Angular Theme auf mehreren Applikationen anzuwenden, muss sie als npm-Package gepublished werden. 
Danach kann das npm-Package in die Anwendung installiert werden.

Über Github Action kann das Package gepublished werden.
Davor muss zuerst der Scope und die Registry für das Publishen definiert werden. 
In der `package.json` wird der `publishConfig` Key spezifiziert:

```json
  "repository": {
    "type": "git",
    "url": "https://github.com/TomBolen/AngularTheme.git"
    },
    "publishConfig": {
      "registry": "https://npm.pkg.github.com/@gemdatnoe"
    }
```

Im Ordner `./github/workflows` wird die File [`npm-publish-github-packages.yml`](https://github.com/gemdatnoe/AngularTheme/blob/main/.github/workflows/npm-publish-github-packages.yml) erstellt. 
Diese übernimmt das Builden und Publishen des npm-Package.
Der Workflow wird getriggert, wenn ein neuer Release erstellt wird und die Package wird auf Github Packages gepublished.