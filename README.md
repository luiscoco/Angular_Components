# Angular Components

## Anatomy of a component

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

## Using components

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

## Importing and using components

Angular supports two ways of making a component available to other components: as a **standalone component** or in an **NgModule**.

## Standalone components

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

## NgModules

Angular code that predates standalone components uses NgModule as a mechanism for importing and using other components. See the full NgModule guide for details.

## Component selectors

Every component defines a **CSS selector** that determines how the component is used:

```typescript
@Component({
  selector: 'profile-photo',
  ...
})
export class ProfilePhoto { }
```

You use a component by creating a matching HTML element in the templates of other components:

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

## Types of selectors

Angular supports a limited subset of basic CSS selector types in component selectors:

**Type selector**:	Matches elements based on their HTML tag name, or node name.

**Attribute selector**¨: Matches elements based on the presence of an HTML attribute and, optionally, an exact value for that attribute.

**Class selector**:	Matches elements based on the presence of a CSS class.

For attribute values, Angular supports matching an exact attribute value with the equals (=) operator. Angular does not support other attribute value operators.

Angular component selectors do not support combinators, including the descendant combinator or child combinator.

Angular component selectors do not support specifying namespaces.


