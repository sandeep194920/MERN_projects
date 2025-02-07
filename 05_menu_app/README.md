# Project details

[Menu App](https://5-menu-app.netlify.app/)

- This project is about displaying Menu Items based on the selected category. When `All` is selected, all the items are shown, and when `Breakfast` or `Lunch` or `Dinner` is selected, only those items are shown.</span>
  <br>

## What you will learn?

1. How to use clipPath CSS property?
2. How to use Gradient Colors?
3. How to use columnSpacing and rowSpacing in MUI Grid?
4. Hower + Active class changes the styling. How to stop that?
5. How to display images in react when we use image links?
6. `<Grid item>` vs `<Grid container>` vs `<Grid container item>`.
7. How to keep image column and description column in same row in Grid container of MenuItem?
8. How to use theme prop in sx?
9. How to make first letter uppercase?
10. UNIT TEST: How to get text content from HTML array elements when we do `getAllByRole`?
11. UNIT TEST: How to test `<Menu/>`?
12. UNIT TEST: How to get code coverage for unit tests?

---

### <span style="color:orange">1. How to use clipPath CSS property?</span>

<br>

In this project, the header has a shape that can be achieved using this clip path css prop. You can get it's CSS here and turn it into Javascript [Clip Path Website](https://bennettfeely.com/clippy/)

---

### <span style="color:orange">2. Gradient Colors</span>

<br>

You can generate beautiful gradients in these sites that can be applied for all the projects

[To generate 1-color, 2-color or 3-color gradients](https://mycolor.space/gradient)

[To get pre-built gradients](https://cssgradient.io/gradient-backgrounds/)

---

### <span style="color:orange">3. ColumnSpacing in Grid</span>

<br>

While I was working in this project, I added spacing prop on the Grid container and it applied space on both x and y axes. To counter this I had to wrap my Grid in another Grid and did some adjustments.

Later, I realised that we could control spacing veritically (using rowSpacing) and horizontally (using columnSpacing) on Grid container which comes in handy lot of times and you don't have to use nested Grid container hopefully.

Notice that, after discovering this columnSpacing, I could comment out the underlying Grid container

```js
<Grid
  container
  sx={{ background: 'lightblue' }}
  justifyContent="center"
  alignItems="center"
  columnSpacing={7}
>
  {/* <Grid
        container
        item
        justifyContent="space-between"
        alignItems="center"
        sx={{ background: 'lightgreen' }}
        sm={8}
        md={6}
        xs={9}

      > */}
  <Grid item>
    <Typography variant="h1">All</Typography>
  </Grid>
  <Grid item>
    <Typography>Breakfast</Typography>
  </Grid>
  <Grid item>
    <Typography>Lunch</Typography>
  </Grid>
  <Grid item>
    <Typography>Dinner</Typography>
  </Grid>
</Grid>
// </Grid>
```

---

### <span style="color:orange">4. Hower + Active class changes the styling. How to stop that?</span>

<br>

- Let's explore the problem what I faced. I was showing active item (selected item) and other items differently

```js
<Typography
  variant="h6"
  sx={
    category === selected
      ? { ...menuStyles.menuItem, ...menuStyles.menuItem.active }
      : menuStyles.menuItem
  }
>
  {category}
</Typography>
```

- When `selected === catgory` I add an active class and if not I add just menuItem class and remove active class

```js
menuItem: {
    borderBottom: `2px solid ${theme.palette.primary.main}`,
    padding: 1,
    cursor: 'pointer',
    textTransform: 'capitalize',
    // When hovered
    '&:hover': {
    borderBottom: `2px solid ${theme.palette.primary.main}`,
    color: theme.palette.primary.main,
    borderRadius: '5px',
    transition: 'all 0.3s',
    },
    active: {
    backgroundColor: theme.palette.primary.main,
    color: 'white',
    borderRadius: '5px',
    border: 'none',
    },
}
```

- The problem I faced here is, when I hover selected element (which also had active class), the text-color changed to primary. Since background also was same color, the text wasn't visible when active class is hovered(selected item)

- To solve this issue, I just changed the color on active hovered element like this

```js
active: {
    backgroundColor: theme.palette.primary.main,
    color: 'white',
    borderRadius: '5px',
    border: 'none',
    // added this hover class on active which actually sets back the color to white which gets overridden by menuItem:hover
    '&:hover': {
        color: 'white',
    },
},
```

---

### <span style="color:orange">5. How to display images in react when we use image links?</span>

<br>

In this project, we are using `data.js` to get our data. In that, we have image link that's like this `./images/item-1.jpeg`

```js
const menu = [
  {
    id: 1,
    title: 'buttermilk pancakes',
    category: 'breakfast',
    price: 15.99,
    img: './images/item-1.jpeg',
    desc: `I'm baby woke mlkshk wolf bitters live-edge blue bottle, hammock freegan copper mug whatever cold-pressed `,
  },
]
```

So where are these images placed? Is this placed in the in one level above in the `images` folder? <strong>Actually NO!</strong>.

If you notice, the path starts with `./images` and in which ever folder you put the `data.js` file this path doesn't change. So what does it mean? It means that we have to put it in a folder such that it will be accessed as './' and one such folder is `public` folder. This folder is exposed to public and you would refer to this folder by saying './'

This need not be named images. You can name it anything. For example, let's say you have `public/assets/img/img-1.jpeg`. Then in your data.js (of course you can put this in any folder within src), you can give this image path and it will work.

#### Gotchas: You cannot import an image if it's in public folder, but directly use it in`<img src='./images/img-1'>`

One gotcha is that, like we did in [3-Tours-App](https://github.com/sandeep194920/MERN_projects/blob/master/3_tours_app/front-end/src/Components/Header/header.js) we can't import images if we place in public folder. In this Tours app, it was possible to import because we placed our images inside src folder and not in public folder(public is outside src folder).

So, we can import img only if it's inside src in any folder. If it's outside we can't import but use directly in JSX `<image src/>`

```js
import img from '../public/images/item-1.jpeg' // This is placed in public folder and will give us error as public folder will not be accessible for imports

//but the below will work. Not importing but using this. This is the technique we generally use when we place our images directly inside our react app and want to use the image link from data file to display, this is the way to go
;<img src="./images/item-1.jpeg" alt="this will work" />
```

---

### <span style="color:orange">6. `<Grid item>` vs `<Grid container>` vs `<Grid container item>`</span>

<br>

`<Grid container></Grid>` is used as a container and inside that, each item can be wrapped by `<Grid item>`

<strong>For example</strong>

```js
<Grid container>
  <Grid item>
    <Typography>Item 1</Typography>
  </Grid>
  <Grid item>
    <Typography>Item 2</Typography>
  </Grid>
  <Grid item>
    <Typography>Item 3</Typography>
  </Grid>
  <Grid item>
    <Typography>Item 4</Typography>
  </Grid>
<Grid>
```

Each Typography is wrapped under `<Grid item>` because, on Grid item, we can specify breakpoints like xs, md and say how much space it needs to consume.

```js
<Grid item xs={8} md={6}>
  <Typography>Item 1</Typography>
</Grid>
```

##### Now what is `<Grid container item></Grid>`

Some times you have a setup like this

```js
<Grid container>

  <Grid item> // This is an item and inside that we again have a container that holds items

    <Grid container direction='column'>
      <Grid item>
        <Typography>Item 1</Typography>
      </Grid>

    </Grid>

  </Grid>

<Grid>
```

In this case <strong>Sometimes</strong> we can combine item and container like this

```js
<Grid container>

  <Grid item> // This is an item and inside that we again have a container that holds items

    <Grid container item direction='column'> //combined
        <Typography>Item 1</Typography>
    </Grid>

  </Grid>

<Grid>
```

<strong>but keep in mind that this container item will be in the new line</strong>

---

### <span style="color:orange">7. How to keep image column and description column in same row in Grid container of MenuItem?</span>

<br>
We need to use `<Grid container wrap="nowrap">` to keep the items within the Grid container in same row without breaking each column into next line on shrinking the screen.

The same problem was solved in this way in [1-show-hide-people README](https://github.com/sandeep194920/MERN_projects/tree/master/1_show_hide_people#how-to-keep-elements-present-in-grid-items-grid-container-in-same-row)

---

### <span style="color:orange">8. How to use theme prop in sx?</span>

<br>

You can use it like this

```js
sx = {{
      background: (theme) => theme.palette.primary.main,
}},
```

By doing this, you don't have to import theme in your component.

---

### 9. How to make first letter uppercase?

<br>

You can define your `sx` like this

```js
desc: {
    pt: 1,
    '&:first-letter': {
      textTransform: 'uppercase',
    },
},
```

Also, this `sx` can also be defined on the `<Grid item>` enclosing typography and need not be on typography itself (this works as well).

```js
<Grid item sx={menuItemStyles.desc}>
  <Typography>{desc}</Typography>
</Grid>
```

This also works

```js
<Grid item>
  <Typography sx={menuItemStyles.desc}>{desc}</Typography>
</Grid>
```

---

### 10. UNIT TEST: How to get text content from HTML array elements when we do `getAllByRole`?

<br>

In `5_menu_app/src/Components/Menu/Menu.test.js` we are testing whether we are getting the right menu tabs (categories). So to test this we do

```js
const menuTabs = within(categories).getAllByRole('heading', { level: 6 })
expect(menuTabs).toHaveLength(4)
```

While this is good and we know we are getting 4 elements, we might also want to know what elements we are getting (text content of each element to be specific)

`getAllByRole('heading', { level: 6 })` gives us the array of HTML nodes. To get the text content, we can do `.textContent` on each item. So we can do like this

```js
const actualMenuitems = within(categories)
  .getAllByRole('heading', { level: 6 })
  .map((item) => item.textContent.toLocaleLowerCase())

console.log(actualMenuItems) //  [ 'all', 'breakfast', 'lunch', 'shakes' ]
```

We need to know if this actualMenuItems is what we are expecting. So let's get expected menu items like this

```js
const expectedMenuItems = [
  'all',
  ...new Set(data.map((item) => item.category)), // this line will give array of duplicate items and then, when we put in Set, it will be unique.
]
```

Now we can compare both these arrays using

```js
expect(actualMenuItems).toEqual(expectedMenuItems)
```

---

### 11. UNIT TEST: How to test `<Menu/>`?

<br>

**We can do an integration test to test**

- When `all` category selected then we should show all items
- When `breakfast` or any other category is selected we should show only those items that belong to that category
  - I am doing this way as shown below. First I render `<Menu\>` which renders all `<MenuItem>` components in a `map`
  - Since `<MenuItem>` is rendered as child of `<Menu\>`, any `data-testid` defined in `<MenuItem>` is also rendered when `<Menu\>` is rendered. See below we are doing `screen.getAllByTestId('menu-item')` and this `menu-item` test ID is present in `Menu Item` component. So we can track how many items are rendered. That's the idea.

```js
test(`'All' category selected`, () => {
  render (<Menu\>)
  // By default 'all' categories are selected
  const allItems = screen.getAllByTestId('menu-item')
  expect(allItems.length).toEqual(expectedData['all'].length) // you could also do .toEqual(data.length)
})
```

---

### 12. UNIT TEST: How to get code coverage for unit tests?

<br>

When we run `npm run test` or `npm test` we get this result

```js
 PASS  src/Components/Header/Header.test.js
 PASS  src/Components/Menu/Menu.test.js

Test Suites: 2 passed, 2 total
Tests:       7 passed, 7 total
Snapshots:   0 total
Time:        2.564 s, estimated 3 s
Ran all test suites.

Watch Usage: Press w to show more.
```

In this we get to know

- How many tests suites ran, how many passed
- How many tests ran, how many passed
- Total time

What if we need more info like

- Files that were tested
- Code coverage - Which all `.js` files are not tested and some more data, then we can run this below

```js
'CI=true npm test -- --env=jsdom --coverage'
```

[Found this here - StackOverflow](https://stackoverflow.com/a/61386453/10824697)

This creates a folder called `coverage` in our project's top level. This folder includes much more details about the tests

Let's put this to our `package.json` file, under scripts

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "test:coverage": "CI=true npm test -- --env=jsdom --coverage",
    "eject": "react-scripts eject"
  },
```

Now we can run our test coverage like this

```js
npm run test:coverage
```

sample output

```js
❯ npm run test:coverage

> 5_menu_app@0.1.0 test:coverage /Users/sandeepamarnath/Coding/React_Projects/5_menu_app
> CI=true npm test -- --env=jsdom --coverage


> 5_menu_app@0.1.0 test /Users/sandeepamarnath/Coding/React_Projects/5_menu_app
> react-scripts test "--env=jsdom" "--coverage"

PASS src/Components/Header/Header.test.js
PASS src/Components/Menu/Menu.test.js
-------------------------|---------|----------|---------|---------|-------------------
File                     | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
-------------------------|---------|----------|---------|---------|-------------------
All files                |   87.09 |      100 |    92.3 |   85.71 |
 src                     |      20 |      100 |       0 |      20 |
  App.js                 |       0 |      100 |       0 |       0 | 6-27
  data.js                |     100 |      100 |     100 |     100 |
  index.js               |       0 |      100 |     100 |       0 | 6-7
 src/Components/Header   |     100 |      100 |     100 |     100 |
  Header.js              |     100 |      100 |     100 |     100 |
 src/Components/Menu     |     100 |      100 |     100 |     100 |
  Menu.js                |     100 |      100 |     100 |     100 |
 src/Components/MenuItem |     100 |      100 |     100 |     100 |
  MenuItem.js            |     100 |      100 |     100 |     100 |
-------------------------|---------|----------|---------|---------|-------------------

Test Suites: 2 passed, 2 total
Tests:       7 passed, 7 total
Snapshots:   0 total
Time:        3.893 s
Ran all test suites.
```
