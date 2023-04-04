# Boy scout rule

Leave the campground cleaner than you found it.

Given the following function:

```ts
function renderTotalPrice(prices: number[]) {
  // TODO: rename s to sum
  const s = prices.reduce((sum, price) => sum + price, 0)

  return `${s} €`
}
```

When you are asked to always display 2 digits after decimal, then the common behavior is to change only the needed lines:

```ts
function renderTotalPrice(prices: number[]) {
  // TODO: rename s to sum
  const s = prices.reduce((sum, price) => sum + price, 0)

  return `${Number(s).toFixed(2)} €`
}
```

But since each new feature added inexorably degrades code quality, the general code quality will drop.

We suggest the alternative solution: improving the code around while adding your feature.

```ts
function renderTotalPrice(prices: number[]) {
  const sum = prices.reduce((sum, price) => sum + price, 0)

  return `${Number(sum).toFixed(2)} €`
}
```

By applying the boy scout rule, the general code quality will improve or at least be stable.
