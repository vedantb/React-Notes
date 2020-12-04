## BILL TRACKER APP

Here, we’re going to build a bill tracker application.

Every month you get a few bills (rent, internet, electricity, gas) and our goal here is to make a cool application that keeps track of them, written using React.

When the app opens for the first time, it asks you to add your first bill category:

![Bill Category](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/bill-tracker/1-intro/Screen%20Shot%202018-07-04%20at%2010.33.40.png)

After you add a category name, you can add your first bill for that category:

![First Bill](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/bill-tracker/1-intro/Screen%20Shot%202018-07-04%20at%2010.34.43.png)

You add a few

![Enter new bill](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/bill-tracker/1-intro/Screen%20Shot%202018-07-04%20at%2010.34.56.png)

then you can see all the bills together as well:

![Bills View](https://thereactcourse.com/giaKcDqEO00cTaLABN3F/bill-tracker/1-intro/Screen%20Shot%202018-07-04%20at%2010.35.12.png)

You can remove bills by clicking the `x` next to each one.

You can also add new categories by pressing the `+` button on the top right, and clicking one category in the navigation filters bills by category, and re-renders the chart so you will see only the bills that belong to one.

Looks like a good set of screens to handle, and relatively not a lot complicated from the point of view of the implementation difficulty.

## LET'S START

As with any project here, you can either work locally with `create-react-app` or use CodeSandbox.

Let’s talk about the app structure, and define the main components that will compose our Bill Tracker application.

We'll need:
1. One component to enter a new category
2. One component to display the categories list on the top
3. One component to list the bills table
4. One component to display the chart
5. One component to add a new bill

Based on what they should do, let’s give those components a proper name:
1. `AddCategory`
2. `NavBar`
3. `BillsTable`
4. `Chart`
5. `AddBill`

Go on and create an empty `.js` file for each of those components in the `src/components` folder (create that folder as well).

Inside each of those files, add a simple placeholder:
```jsx
import React from 'react'

export default () => {
  return <div />
}
```

You could also just create components as you need them, but this little analysis up front gives you a more organized approach.

