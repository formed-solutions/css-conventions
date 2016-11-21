# CSS conventions

Conventions for writing consistent CSS for **Formed** products and projects.

The main goals of this document is to provide:

  - standard of code quality across project;
  - provide consistency across project;
  - keep styles readable;
  - keep styles scalable;

## Table of contents

  1. [Code style](#style)
    - [Whitespace](#whitespace)
    - [Formatting & syntax](#syntax)
    - [Commenting](#commenting)
  2. [Naming conventions](#naming)
  3. [Directory structure](#directory)
    - [Base styles](#base)
    - [Components](#components)
    - [Helpers](#helpers)
    - [State](#state)
  4. [Configuration](#configuration)

## 1.<a name="style"></a> Code style

The below guidelines take into consideration that [SASS](http://sass-lang.com/)
is the preprocessor of choice.

### <a name="whitespace"></a> 1.1 Whitespace

  - Use spaces over tabs.
  - Use two (2) spaces for indentation.
  - Dont' use more than one blank line as a separator.
  - Ideally, keep line length to a maximum 80 columns.
  - Strip all end-of-line whitespace.
  - Leave one line of whitepace at end-of-file.

### <a name="syntax"></a> 1.2 Formatting & Syntax

Always use the `.scss` file extension and not the `.sass` on projects that are leveraging the preprocessor.

- Use one selector per line in multi-selector rulesets.
- <sup>*</sup>Include a single space before the opening brace of a ruleset.
- Include one declaration per line in a declaration block.
- Use one level of indentation per declaration.
- Include a single space after the colon of the property name of a declaration.
- Use lowercase and shorthand hex values, e.g. `#fff`.
- Use double quotes, e.g. `background-image: url("image.png")`.
- Quote attribute values in selectors, e.g. `input[type="radio"]`.
- Avoid specifying units for zero-values, e.g. `padding: 0`.
- Include a space after each comma in comma-separated function values or properties.
- <sup>*</sup>Place the closing brace of a ruleset in the same column as the first character of the ruleset.
- Each ruleset should be seperated by one (1) blank line.

Example:

```css
  /** .scss */
  .foo,
  .bar,
  .baz[type="radio"] {
    color: #000;
    display: block;
  }

  .foobar {
    margin: 0;
  }
```

### Declaration order

Declaration properties are grouped by context, with a single line seperating each group, and properties arranged in alphabetical order per group.

Group contexts and order within declaration block are as follows:

 1. layout
 2. box
 3. background
 4. typographical
 5. Other
  - generated content
  - list properties
  - table properties
  - ...

Example:

  ```css
  .selector {
    display: block;
    overflow: hidden;
    position: relative;

    border: 1px solid;
    box-sizing: border-box;
    margin: 10px;
    padding: 10px;

    background: transparent;

    color: #333;
    font-size: 16px;

    list-style: none;
  }
  ```

### Nesting

Never nest more than 3 levels deep in a given declaration block.

```css
  // Bad example
  .menu {
    // declarations

    .menu-container {
      // declarations

      .menu-item {
        // declarations

        &:focus,
        &:hover {}
      }
    }
  }

  // Better

  .menu .menu-container {
    .menu-item {
      // declarations

      &:focus,
      &:hover {
        // declarations
      }
    }
  }
```

### <a name="commenting"></a> 1.3 Commenting

Make a point to describe components, intent, examples, and any limitations.

- Place comments on a new line above content.
- Keep line-length to 80-columns.
- Use comments to break code up into sections.
- Use "sentence case" comments.
- Use consistent indentation.

Example:

```css
  /**
   * Short description or thematic title using Doxygen-style comment format.
   *
   * Longer description for content matter.  Great for providing more details description and explanations.
   */

   /* Section comment block
      -------------------------------------------------------------------- */

   /* Basic comment */
```

## <a name="naming"></a> Naming Conventions

Use structured class names and reliance on purposeful hyphens.

The main architectural divide is b/w **components**, **helpers**, and **state**.

### Components

Syntax: [&lt;ComponentName&gt;][-subComponent][--modifierName]

#### ComponentName

Component names are written in pascal case.  Nothing else in the HTML/CSS(SASS) should use pascal case.

```css
  .Modal {
    display: block
    width: 100%
  }

  .FooComponent { // styles go here. }

  <div class="Modal"></div>
  <div class="FooComponent"></div>
```

#### ComponentName--modifierName

A modifier is a class that will modify the base component.  Typically to alter/adjust presentation of the component (e.g. skinning), for special situations it is permissable to use modifiers to adjust layout or the box-model.  Those instances be sure to include detailed comments as to why.  Modifier names should be writtin in camel case and separeted from component name by two hyphens.  Both the component name and modifier should be included in the HTML.

```css
  /**
   * Grid component
   */

  .Grid {
    display: flex;
  }

  /**
   * Grid modifier
   */

  .Grid--center {
    justify-content: center;
  }

  <div class="Grid Grid--center">...</div>
```

#### ComponentName-subComponent

A sub-component is a class attached to a descendent (HTML) node of a component.  It's responsiblity is for applying presentation directly to the descendent node for the component.  All sub-components are writtin in camel case and are separated from component name by a single hyphen.

It is permissable to modify a sub-component, but should be reserved for special circumstances.  Always try to modify the base component and adjust sub-component accordingly.

```css
  /**
   * Grid component
   */

  .Grid {
    display: flex;
  }

  /**
   * Grid sub-component
   */

  .Grid-cell {
    flex: 0 0 100%;
  }

  <div class="Grid">
    <div class="Grid-cell">...</div>
  </div>
```

#### State

Use the `is-nameOfState` to express change in *state* of the component.  State names should be camelcased and chained to the component name.

```css
  /**
   * Component
   */

  .Dropdown {
    display: block;
  }

  /**
   * Sub-component
   */

  .Dropdown-menu {
    display: none;
  }

  /**
   * State
   */

  .Dropdown-menu {
    .Dropdown.is-open & {
      display: block;
    }
  }

  <div class="Dropdown is-open">
    <div class="Dropdown-menu">...</div>
  </div>
```

### Helpers

Helpers are very low-level structural, layout, and presentational selectors.  Helpers can be applied to any HTML node to augment the layout, box-model, or presentation.  They can be used on components and sub-components.

Syntax: &lt;subject&gt;-&lt;helperName&gt;][--modifierName]

The helper **subject** name is a map to what trait that helper is aiming to augment, should be all lowercase, single-word, and map directly to it's stylesheet.  The **helperName** is camel case and is separated by **subject** and a single hyphen.

Modifying helpers is permissable but should not be used liberally.  Specific use case for modifying a helper would be for targetting helper in a media query block.

```css
  .size-1of2 {
    width: 50% !important;
  }

  .size-1of2--sm {
    @media (min-width: 320px) {
      width: 50% !important;
    }
  }

  <div class="size-1of2 size-1of2--md">...</div>
```

## <a name="directory"></a> 3. Directory structure

The following is a brief description of the directory structure for organizing stylesheets.

### <a name="base"></a> 3.1 Base directory

This directory is for setting application defaults.  All stylesheets should include only element selectors, pseudo classes, or attribute selectors.  It is strictly for defining the default look for how a element should appear across all occurrences throughout the application.

This is where you would define any CSS resets or node normalization.  The use `!important` should be restrained from using in this area.

```css
    // Good

    html {
      font-size: 16px;
    }

    body {
      background-color: #fff;
    }

    h1 {
      font-size: 2rem;
    }

    // Bad

    .h1.selector {
      font-size: 4rem;
    }

    .selector {
      display: inline-block;
    }
```

### <a name="components"></a> 3.2 Components

Components are pieces of your UI. It can be navigation, modals, dialogs, buttons, etc.  All component should follow the appropriate naming convention and placed in their own stylesheet for import.  Components are designed and intended to be standalone piecies of UI.  A `.Grid` is not aware of a `.Button` nor should a `.Modal` ever need to know it has a `.Grid` inside of it.  They are completely independent from one another.

```css
  // Good
  
  // _grid.sass
  .Grid {
    display: flex;
  }

  // _modal.sass

  .Modal {
    position: absolute;
  }

  // Bad
  // _modal.sass */

  .Modal .Grid {
    disply: inline-block;
  }
```

### <a name="helpers"></a> 3.3 Helpers

Helpers map to structural, layout, and presentational traits.  Due to narrowly focus nature of helpers, the use of `!important` is typically used to make certain that styles are applied.

A nice workflow is to use the helpers to prototype out UI or construct UI quickly.  If you find yourself using a set of helpers together often, that is a good signal to ask yourself if you have a component.

### <a name="state"></a> 3.4 State

State is something that overrides or augments existing styles.  Most state styles can be coupled with their components.  In situations where you have a state style that is not necessarily coupled to a component, that would go here.

All state selectors should prepended with `.is-<name>`.

State selectors are typically applied or removed from a HTML node through JavaScript which is another reason to stick to desired naming convention.  That will provide warning to another developer that this selector is (most likely) being used outside of CSS as well.  Update with caution.

### <a name="vendor"></a> 3.5 Vendor

When installing third-party dependencies create a specific `<vendor>.scss` with
the dependency name and import the stylesheet to it.  Then import newly created
vendor stylesheet to main manifest file `main.scss` located in the root of this
directory.

In case where the developer needs to extend or overwrite any styles in the
dependency, those can be placed directly into this file.

## <a name="configuration"></a> Configuration

The SASS variables can be found in the `_config.scss` file.  Developers are encourage to follow these guildelines on when to add a new configuration option:

- the value is repeated twice;
- the value is likely to be updated at least once;

### !default flag

All configuration options should be set with the `!default` flag so they can be overwritten inside the modules.

```css
    $gray-dark: #333 !default
```

