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
