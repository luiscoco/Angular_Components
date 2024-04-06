# Angular Components

## 1. Anatomy of a component

Every component must have:

A **TypeScript class** with behaviors such as handling user input and fetching data from a server

An **HTML template** that controls what renders into the DOM

A **CSS selector** that defines how the component is used in HTML

You provide Angular-specific information for a component by adding a **@Component** decorator on top of the TypeScript class:

```typescript
@Component({
  selector: 'profile-photo',
  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,
})
export class ProfilePhoto { }
```

For full details on writing Angular templates, see the **Templates guide**(https://angular.dev/guide/templates).

The object passed to the @Component decorator is called the **component's metadata**. This includes the **selector**, **template**, and other **properties** described throughout this guide.

Components can optionally include a list of CSS styles that apply to that component's DOM:

```typescript
@Component({
  selector: 'profile-photo',
  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,
  styles: `img { border-radius: 50%; }`,
})
export class ProfilePhoto { }
```

By default, a component's styles only affect elements defined in that component's template. See Styling Components for details on Angular's approach to styling.

You can alternatively choose to write your **template and styles in separate files**:

```typescript
@Component({
  selector: 'profile-photo',
  templateUrl: 'profile-photo.html',
  styleUrl: 'profile-photo.css',
})
export class ProfilePhoto { }
```

This can help separate the concerns of presentation from behavior in your project. You can choose one approach for your entire project, or you decide which to use for each component.

Both **templateUrl** and **styleUrl** are relative to the directory in which the component resides.

## 2. Using components

Every component defines a **CSS selector**:

```typescript
@Component({
  selector: 'profile-photo',
  ...
})
export class ProfilePhoto { }
```

See **Component Selectors** for details about which types of selectors Angular supports and guidance on choosing a selector.

You use a component by creating a matching HTML element in the template of other components:

```typescript
@Component({
  selector: 'user-profile',
  template: `
    <profile-photo />
    <button>Upload a new profile photo</button>`,
  ...,
})
export class UserProfile { }
```

See Importing and using components for details on how to reference and use other components in your template.

Angular creates an instance of the component for every matching HTML element it encounters. 

The DOM element that matches a component's selector is referred to as that component's host element. The contents of a component's template are rendered inside its host element.

The **DOM** rendered by a component, corresponding to that **component's template**, is called that **component's view**.

In composing components in this way, you can think of **your Angular application as a tree of components**.

![image](https://github.com/luiscoco/Angular_Components/assets/32194879/94da428d-fe30-4dfc-944f-5566c3fb4e58)

This tree structure is important to understanding several other Angular concepts, including **dependency injection** and **child queries**.

## 3. Importing and using components

Angular supports two ways of making a component available to other components: as a **standalone component** or in an **NgModule**.

### 3.1. Standalone components

A **standalone component** is a component that sets **standalone: true** in its component metadata. 

Standalone components directly import other components, directives, and pipes used in their templates:

```typescript
@Component({
  standalone: true,
  selector: 'profile-photo',
})
export class ProfilePhoto { }

@Component({
  standalone: true,
  imports: [ProfilePhoto],
  template: `<profile-photo />`
})
export class UserProfile { }
```

Standalone components are directly importable into other standalone components.

The Angular team recommends using standalone components for all new development.

### 3.2. NgModules

Angular code that predates standalone components uses NgModule as a mechanism for importing and using other components. See the full NgModule guide for details.

## 4. Component selectors

Every component defines a **CSS selector** that determines how the component is used:

```typescript
@Component({
  selector: 'profile-photo',
  ...
})
export class ProfilePhoto { }
```

You use a component by creating a matching **HTML element** in the templates of other components:

```typescript
@Component({
  template: `
    <profile-photo />
    <button>Upload a new profile photo</button>`,
  ...,
})
export class UserProfile { }
```

Angular matches selectors statically at compile-time. Changing the DOM at run-time, either via Angular bindings or with DOM APIs, does not affect the components rendered.

An element can match exactly one component selector. If multiple component selectors match a single element, Angular reports an error.

Component selectors are **case-sensitive**.

### 4.1 Types of selectors

Angular supports a limited subset of basic CSS selector types in component selectors:

**Type selector**:	Matches elements based on their **HTML tag name**, or **node name**.

**Attribute selector**¨: Matches elements based on the presence of an **HTML attribute** and, optionally, an exact **value for that attribute**.

**Class selector**:	Matches elements based on the presence of a **CSS class**.

For attribute values, Angular supports matching an exact attribute value with the equals (=) operator. Angular does not support other attribute value operators.

Angular component selectors do not support combinators, including the descendant combinator or child combinator.

Angular component selectors do not support specifying namespaces.

### 4.2. Selectors samples

#### Element Selector

The **element selector** is the most common and straightforward type of selector. 

It allows you to use the component as an HTML element within your templates.

```typescript
@Component({
  selector: 'app-my-component',
  template: `<p>Hello from My Component!</p>`,
})
export class MyComponent {}
```

Usage in HTML:

```html
Copy code
<app-my-component></app-my-component>
```

#### Attribute Selector

You can also define a selector as an attribute. 

This is useful when you want to attach behavior or styling to existing elements without changing the HTML structure significantly.

```typescript
@Component({
  selector: '[appHighlight]',
  template: `<p>This paragraph will be styled with appHighlight.</p>`,
})
export class HighlightComponent {}
```

Usage in HTML:

```html
<p appHighlight>This text will be highlighted.</p>
```

#### Class Selector

Angular components can also be selected by CSS class. 

This is less common but might be useful in scenarios where you want to apply component behavior based on CSS classes.

```typescript
@Component({
  selector: '.app-my-class',
  template: `<p>This component is selected by class!</p>`,
})
export class MyClassComponent {}
Usage in HTML:

```html
<div class="app-my-class"></div>
```

Note: While class selectors can be used, they're generally not recommended for components because Angular adds and removes these classes dynamically, which can interfere with styling and component functionality.

#### Combination Selector

You can combine multiple selectors using a comma to apply a component or directive in different ways. This allows for flexible usage of components across your application.

```typescript
@Directive({
  selector: 'input[appAutoFocus], textarea[appAutoFocus]',
})
export class AutoFocusDirective {
  // Directive logic here
}
```

Usage in HTML:

```html
<input appAutoFocus type="text">
<textarea appAutoFocus></textarea>
```

This directive can be applied to both **input** and **textarea** elements that have the **appAutoFocus attribute**.

**Summary**

**Element selectors** are used to define new elements in your application.

**Attribute selectors** are useful for adding behavior to existing elements without changing their type.

**Class selectors** are less common and not recommended for components due to potential dynamic class manipulation by Angular.

**Combination selectors** offer flexibility in how components or directives can be applied, allowing them to work in multiple contexts.

**Selectors** provide a powerful way to define how and where your components and directives are applied, enabling you to build more modular and reusable Angular applications.

### 4.3. The :not pseudo-class

Angular supports the **:not pseudo-class**. You can append this pseudo-class to any other selector to narrow which elements a component's selector matches. 

For example, you could define a **[dropzone] attribute selector** and prevent matching textarea elements:

```typescript
@Component({
  selector: '[dropzone]:not(textarea)',
  ...
})
export class DropZone { }
```

Angular does not support any other pseudo-classes or pseudo-elements in component selectors.

### 4.4. Combining selectors

You can combine multiple selectors by concatenating them. For example, you can match <button> elements that specify type="reset":

```typescript
@Component({
  selector: 'button[type="reset"]',
  ...
})
export class ResetButton { }
```

You can also define multiple selectors with a comma-separated list:

```typescript
@Component({
  selector: 'drop-zone, [dropzone]',
  ...
})
export class DropZone { }
```

Angular creates a component for each element that matches any of the selectors in the list.

### 4.5. When to use an attribute selector

You should consider an attribute selector when you want to create a component on a standard native element.

For example, if you want to create a custom button component, you can take advantage of the standard <button> element by using an attribute selector:

```typescript
@Component({
  selector: 'button[yt-upload]',
   ...
})
export class YouTubeUploadButton { }
```

```html
<button yt-upload="true">Upload Video</button>
```
## 5. Styling components

Components can optionally include **CSS styles** that apply to that component's DOM:

```typescript
@Component({
  selector: 'profile-photo',
  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,
  styles: ` img { border-radius: 50%; } `,
})
export class ProfilePhoto { }
```

You can also choose to write your styles in separate files:

```typescript
@Component({
  selector: 'profile-photo',
  templateUrl: 'profile-photo.html',
  styleUrl: 'profile-photo.css',
})
export class ProfilePhoto { }
```

When Angular compiles your component, these styles are emitted with your component's JavaScript output.

This means that component styles participate in the JavaScript module system. When you render an Angular component, the framework automatically includes its associated styles, even when lazy-loading a component.

### 5.1  Style scoping

Every component has a **view encapsulation** setting that determines how the framework scopes a component's styles. 

There are three view encapsulation modes: Emulated, ShadowDom, and None. You can specify the mode in the **@Component** decorator:

```typescript
@Component({
  ...,
  encapsulation: ViewEncapsulation.None,
})
export class ProfilePhoto { }
```

### 5.2. ViewEncapsulation.Emulated

By default, Angular uses emulated encapsulation so that a component's styles only apply to elements defined in that component's template. 
 
In this mode, the framework generates a unique HTML attribute for each component instance, adds that attribute to elements in the component's template, and inserts that attribute into the CSS selectors defined in your component's styles.

This mode ensures that a component's styles do not leak out and affect other components. However, global styles defined outside of a component may still affect elements inside a component with emulated encapsulation.

In emulated mode, Angular supports the :host and :host-context pseudo classes without using Shadow DOM. 

During compilation, the framework transforms these pseudo classes into attributes. Angular's emulated encapsulation mode does not support any other pseudo classes related to Shadow DOM, such as ::shadow or ::part.

### 5.3. ::ng-deep

Angular's emulated encapsulation mode supports a custom pseudo class, :ng-deep. 

Applying this pseudo class to a CSS rule disables encapsulation for that rule, effectively turning it into a global style. 

The Angular team strongly discourages new use of ::ng-deep. These APIs remain exclusively for backwards compatibility.

### 5.4. ViewEncapsulation.ShadowDom

This mode scopes styles within a component by using the web **standard Shadow DOM API**. 

When enabling this mode, Angular attaches a shadow root to the component's host element and renders the component's template and styles into the corresponding shadow tree.

This mode strictly guarantees that only that component's styles apply to elements in the component's template. 

Global styles cannot affect elements in a shadow tree and styles inside the shadow tree cannot affect elements outside of that shadow tree.

Enabling ShadowDom encapsulation, however, impacts more than style scoping. 

Rendering the component in a shadow tree affects event propagation, interaction with the <slot> API, and how browser developer tools show elements. 

Always understand the full implications of using Shadow DOM in your application before enabling this option.

### 5.5. ViewEncapsulation.None

This mode **disables all style encapsulation** for the component. 

Any styles associated with the component behave as global styles.

### 5.6. Defining styles in templates

You can use the **<style>** element in a component's template to define additional styles. 

The component's view encapsulation mode applies to styles defined this way.

Angular does not support bindings inside of style elements.

### 5.7. Referencing external style files

Component templates can use the **<link>** element to **reference CSS files**. 

Additionally, your CSS may use the **@importat-rule** to reference CSS files. 

Angular treats these references as external styles. 

External styles are not affected by emulated view encapsulation.



