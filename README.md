# ESLint Tricks

A collection of useful ESLint tricks

## React: Enforce Specific Children Component Types

To enforce the component type of all children in a specific component, use [the `no-restricted-syntax` built-in rule](https://eslint.org/docs/latest/rules/no-restricted-syntax):

`eslint.config.js`

```js
import tseslint from 'typescript-eslint';

export default tseslint.config({
  rules: {
    'no-restricted-syntax': [
      'warn',
      {
        selector:
          "JSXElement[openingElement.name.name='List'] JSXElement:not([openingElement.name.name='ListItem'])",
        message:
          'The List component must contain only <ListItem /> elements as children',
      },
    ],
  },
});
```

Examples of **incorrect** code for this rule config:

```js
<List>
  <div />
  <ListItem />
</List>
```

Examples of **correct** code for this rule config:

```js
<List>
  <ListItem />
  <ListItem />
</List>
```

![Screenshot 2024-04-25 at 11 21 38](https://github.com/karlhorky/eslint-tricks/assets/1935696/81db901d-9f44-4020-83c9-bcf00d5878f8)

- [`typescript-eslint` Playground](https://typescript-eslint.io/play/#ts=5.3.3&fileType=.tsx&code=GYVwdgxgLglg9mABAGRgZygCgJSIN4C%2BAUKJLAiulAJJQCmAtjvsUQDwB8RiibqGXHjzYATGADdEAekFC%2BVWo2my2U-lC7de62cPWKGyrXoX1DMrap3sLRO-YcOgA&eslintrc=N4KABGBEBOCuA2BTAzpAXGUEKQHYHsBaaFAF2gEsBjUxAE0OQE9dSBDAD3TAG1xscAdzbRckADT8BWAdkjJESGvmjdIAKQDKADQCiSALaJWPfAAdjFXAHN9iI6wB0uNkeevEAXgDkAGQrIpN4AumBaeobGpGgEpAAUpha4VraRTi5uGV5%2BAaQAkrQGIQCUElKykEbIyGzWiOjlslAAKgAWiGD%2BgWBU%2BAZm%2BLhRYAaw3b2sbFZgg-BMYAA8XfmFYAD0AHxgivZRyGBs%2B1StFPB0JGKNYAC%2B5cH8t9dAA&tsconfig=N4KABGBEDGD2C2AHAlgGwKYCcDyiAuysAdgM6QBcYoEEkJemy0eAcgK6qoDCAFutAGsylBm3TgwAXxCSgA&tokens=false)
- [AST explorer](https://astexplorer.net/#/gist/d6366a9a786f34682ba023163d3834c8/bc2e151e077502f597ee51eabb718c6fc4ee493b)

## React: Enforce Specific Children Component Types and Order

To enforce both the component type and order of children in a specific component, use [the `no-restricted-syntax` built-in rule](https://eslint.org/docs/latest/rules/no-restricted-syntax) and reference the index of the children by odd numbers starting at `1` (`1` is first child, `3` is second child, `5` is third child, etc):

`eslint.config.js`

```js
import tseslint from 'typescript-eslint';

export default tseslint.config({
  rules: {
    'no-restricted-syntax': [
      'warn',
      {
        selector:
          "JSXElement[openingElement.name.name='TwoColumnLayout']:not([children.1.openingElement.name.name='Meta']), JSXElement[openingElement.name.name='TwoColumnLayout']:not([children.3.openingElement.name.name='main']), JSXElement[openingElement.name.name='TwoColumnLayout']:not([children.5.openingElement.name.name='aside'])",
        message:
          'The TwoColumLayout component must contain only <Meta />, <main> and <aside> elements as children, in exactly this order',
      },
    ],
  },
});
```

Examples of **incorrect** code for this rule config:

```js
<TwoColumnLayout/>

<TwoColumnLayout>
</TwoColumnLayout>

<TwoColumnLayout>
  <main />
  <aside />
</TwoColumnLayout>
```

Examples of **correct** code for this rule config:

```js
<TwoColumnLayout>
  <Meta />
  <main />
  <aside />
</TwoColumnLayout>
```

![Screenshot 2024-04-25 at 10 57 57](https://github.com/karlhorky/eslint-tricks/assets/1935696/95e18960-4e8e-4b41-b0c8-42b86115bf28)

- [`typescript-eslint` Playground](https://typescript-eslint.io/play/#ts=5.3.3&fileType=.tsx&code=GYVwdgxgLglg9mABAFQO5wMJwDYgLZgAyAhgJ5whQAUAlIgN4C%2BAUKJLAogLICmUxtBi2YAeAHzNEiEWkw58RMhSgB6CZOmysuAiXKUJUkSq3zdSg8w0z02hXuWGp0vMRhI1Go8QDOMACY8iJ5GJrZmivpQ6kamOpGOXtK8-MFORq7uaUkivgFBIdJhcvEOlsbqVlXV1UA&eslintrc=N4KABGBEBOCuA2BTAzpAXGUEKQHYHsBaaFAF2gEsBjUxAE0OQE9dSBDAD3TAG1xscAdzbRckADT8BWAdkjJESGvmjdIAKQDKADQCiSALaJWPfAAdjFXAHN9iI6wB0uNkeevEAXgDkAFUH4AML48LAGuAAybEz4sKTeALpoBKQAFDxUABYU8HQkuI4AjI7mljZ2DqTubi5GPgCyiOyJAJTiYFp6hsakpha4VrbdTrWI1V5%2BAcGh4VExcYnJ%2BGkZ2bn5jgDMJf2DFT3j4z4GbFat7Z37JqUD5cNVo0eTQSFhkdGx8Ukp6Vk5ecZHABWHZlIb2A6PUY%2BNjICh0RCtCRSWSQIzIZBsayIdAo2RQXyZRBgfwvGZzT5gKj4AxmfC4HpgAywZCkKn09hWMD0%2BBMMAAHka7DAAHoAHztfknKxisBsXB0AWw%2BGIWWKCGsZByrV-dbGdpcxAcNg0XlgUjZLUqBGqPFgAC%2BKIS-Ed9qAA&tsconfig=N4KABGBEDGD2C2AHAlgGwKYCcDyiAuysAdgM6QBcYoEEkJemy0eAcgK6qoDCAFutAGsylBm3TgwAXxCSgA&tokens=false)
- [AST explorer](https://astexplorer.net/#/gist/0c94bae9f735b6e337c1b49ea348de02/09b550f2f5029dec9787b77635d8e0f7e9cd0554)
