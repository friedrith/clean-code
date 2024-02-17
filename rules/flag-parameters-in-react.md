# Flag parameters in React

If you add a boolean property to ad React component to activate/deactive a feature, you are using a flag parameter. Flag parameters are the sign that the component is doing more than one thing. It is better to split the component in two components.

For example:

```tsx
// Bad example
function Button({ isPrimary, ...props }: { isPrimary: boolean }) {
  return (
    <div>
      {isPrimary ? <span>Primary</span> : <span>Secondary</span>}
      <button {...props}>Click me</button>
    </div>
  )
}
```

## Why is it a bad practice?

Flag parameters makes the component

- harder to understand and to maintain: instead of debugging one feature, devs will now have to understand the previous component and your new feature.
- not scalable: every time you add a new feature, you will have to add a new flag parameter. After few months the component may be too large.
- unstable: everytime you add a new feature, you have to touch the component. It can break the component and the whole application.

## How to fix it?

In React, one of the main solution is to use composition pattern. Instead of adding a flag parameter, you can create a new component that will compose the previous component and add the new feature.

The composition will allow you to extend the capabilities of the previous component without modifying it. It will also allow you to reuse the previous component in other contexts.

For example:

```tsx
// Good example
function Button({ children, ...props }: { children: React.ReactNode }) {
  return (
    <div>
      {children}
      <button {...props}>Click me</button>
    </div>
  )
}

// if you need multiple extension points you can use other properties to use composition
function Button({ primary, ...props }: { primary: React.ReactNode }) {
  return (
    <div>
      {primary}
      <button {...props}>Click me</button>
    </div>
  )
}
```

## Resources

- [Keep your React Components maintainable with SOLID & React Composition— CodeCraftsmanship #4](https://medium.com/interaction-dynamics/keep-your-react-components-maintainable-with-solid-react-composition-codecraftsmanship-4-2969834e9ffa)
- [Avoid flag arguments](https://learn.interaction-dynamics.io/docs/code-craftsmanship/clean-code-rules#avoid-flag-arguments)
- [The Flag Parameter Anti-Pattern](https://dev.to/rweisleder/the-flag-parameter-anti-pattern-1j82)
