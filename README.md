# Interview Questions

## Table of context

1. Metafields, Meta Objects, GraphQL
2. Webhooks, Resource Pickup
3. Shopify CLI, App Bridge, Polaris
4. Import & Export XLS in Node
5. React, Function Components, Hooks, State & Props
6. Context API, Redux Toolkit
7. Breadcrumb

<hr/>

<br/>

> What are Shopify Metafields?

Metafields allow merchants to store additional structured data for Shopify resources like products, variants, collections, and customers. They help extend Shopify’s default fields with custom data.

> What are the main components of a metafield?

- Namespace - A way to group related metafields (e.g., custom).
- Key - A unique identifier within the namespace (e.g., size).
- Value - The data stored (e.g., Large).
- Value Type - The data type (e.g., string, integer, json)

> What are some use cases for metafields?

Storing additional product details (e.g., fabric type, weight).
Custom fields for customers (e.g., loyalty points).
Extra attributes for orders (e.g., gift message, estimated delivery date).

> What are Meta Objects in Shopify?

Meta Objects allow merchants to create structured custom data entities that can be referenced across multiple Shopify resources, enabling richer customization beyond simple metafields.

How do Meta Objects differ from Metafields?

|   Feature    |                   Metafields                    |                 Meta Objects                  |
| :----------: | :---------------------------------------------: | :-------------------------------------------: |
| Data Storage | Attached to a specific resource (e.g., product) |         Standalone custom data entity         |
|  Structure   |                 Key-value pairs                 |         More complex structured data          |
|   Use Case   |               Simple extra fields               | Reusable content types like FAQs, size charts |

> Example Queries:

```
mutation {
  productUpdate(
    product: {
        id: "gid://shopify/Product/9916733522229",
        metafields: [
            {
                namespace: "bakery",
                key: "isHeavy",
                value: "true",
                type: "single_line_text_field",
            }
        ]
    }
  ) {
    product {
      metafield(namespace: "bakery", key: "isHeavy") {
        key
        value
      }
      metafields(first: 10) {
        edges {
          node {
            key
            value
          }
        }
      }
    }
  }
}
```

> https://shopify.dev/docs/apps/build/custom-data

> What is Shopify CLI?

Shopify CLI is a command-line tool that streamlines the creation, testing, and management of Shopify apps and themes, automating common development tasks to enhance developer efficiency.

> What are the key features of Shopify CLI?

- App creation: Quickly generate Shopify apps (shopify app init).
- Theme development: Work on Shopify themes.
- Tunnel support: Use ngrok to expose local apps for testing.
- Shopify Functions: Deploy custom backend logic.

> What is Shopify App Bridge?

Shopify App Bridge is a JavaScript library that helps apps integrate seamlessly with Shopify Admin, POS, and Checkout by handling authentication, navigation, and UI components.

> What are some key features of Shopify App Bridge?

- Authentication: Manages OAuth login flow.
- Navigation: Redirects users within the Shopify Admin.
- Modal & Toasts: Creates modals, pop-ups, and success/error messages.

> How do you redirect users within Shopify Admin using App Bridge?

```
import { useAppBridge } from "@shopify/app-bridge-react";
import { Redirect } from "@shopify/app-bridge/actions";

function RedirectToOrders() {
  const app = useAppBridge();
  const redirect = Redirect.create(app);

  return <button onClick={() => redirect.dispatch(Redirect.Action.APP, "/orders")}>Go to Orders</button>;
}
```

> What is Shopify Polaris?

Polaris is Shopify’s design system, providing reusable React components and guidelines to maintain UI consistency in Shopify apps.

> How do you use Polaris components in a React app?

```
import { Card, Page } from "@shopify/polaris";

function MyComponent() {
  return (
    <Page title="Dashboard">
      <Card sectioned>
        <p>Welcome to my Shopify app!</p>
      </Card>
    </Page>
  );
}
```

> What are some commonly used Polaris components?

- Page - Defines a structured Shopify Admin page.
- Card - A container for grouping content.
- Button - Standard Shopify buttons.
- Toast - Displays temporary success/error messages.
- Modal – Pop-up modals

> What are webhooks in Shopify?

Webhooks in Shopify are HTTP callbacks that notify an app when specific events occur in a store, such as order creation, product updates, or customer changes.

Authentication using HMAC(Hash-based Message Authentication Code)

```
const hash = crypto.createHmac("sha256", secret).update(body).digest("base64");
```

> What is Shopify Resource Pickup?

Shopify’s Resource Pickup allows customers to buy online and pick up their order from a physical store location instead of getting it shipped

```
{
  order(id: "gid://shopify/Order/6447534309685") {
    fulfillmentOrders(first: 50) {
      edges {
        node {
          deliveryMethod {
            methodType
            presentedName
          }
        }
      }
    }
  }
}
```
If methodType is "PICK_UP", the order is for pickup. (Others: LOCAL, NONE, PICKUP_POINT, SHIPPING, RETAIL)

> How do you read an Excel file in a Node.js app?

```
const xlsx = require("xlsx");

const workbook = xlsx.readFile("data.xlsx");
const sheetName = workbook.SheetNames[0];
const data = xlsx.utils.sheet_to_json(workbook.Sheets[sheetName]);

console.log(data);
```

> How do you write data to an Excel file in Node.js?

```
const xlsx = require("xlsx");
const fs = require("fs");

const data = [
  { Name: "John Doe", Age: 30, Email: "john@example.com" },
  { Name: "Jane Smith", Age: 25, Email: "jane@example.com" }
];

const worksheet = xlsx.utils.json_to_sheet(data);
const workbook = xlsx.utils.book_new();
xlsx.utils.book_append_sheet(workbook, worksheet, "Sheet1");

xlsx.writeFile(workbook, "output.xlsx");
console.log("Excel file created: output.xlsx");
```

> What are some common use cases for Excel import/export in Shopify apps?

- Bulk product updates: Import/export product details from Shopify.
- Order reports: Generate Excel reports for store orders.
- Customer data migration: Import customer lists into Shopify.

<hr/>
<br/>

> What is React?

React is a JavaScript library for building user interfaces, mainly used for developing single-page applications (SPAs) with a component-based architecture.

- Fast rendering with the Virtual DOM.
- Reusable components improve maintainability.
- Strong ecosystem with libraries like Redux, React Router.

> What are the main features of React?

- Component-based architecture
- Virtual DOM for efficient updates
- Unidirectional data flow
- Hooks for state management
- React Router for navigation

> What is a function component in React?

A function component is a JavaScript function that returns JSX. It does not have lifecycle methods like class components but can use hooks.

> What are React hooks?

Hooks allow us to use React features such as state and lifecycle methods in functional component.

> Difference between function and class component

|   Feature   |   Function Component   |              Class Component               |
| :---------: | :--------------------: | :----------------------------------------: |
|   Syntax    |     Function-based     |             Uses class keyword             |
|    State    |   Uses useState hook   |              Uses this.state               |
|  Lifecycle  | Uses hooks (useEffect) | Uses lifecycle methods (componentDidMount) |
| Performance |     More optimized     |              Slightly heavier              |

> What are commonly used hooks in React?

- useState – Manages state inside a function component
- useEffect – Handles side effects (API calls, subscriptions)
- useContext – Accesses values from React Context
- useReducer – Alternative to useState, useful for complex state logic
- useRef – Creates a reference to DOM elements or persists values between renders


> What is the difference between state and props?
### State:
- Internal data of a component
- Can be modified using setState or useState
- Local to the component
### Props:
- Data passed from parent to child
- Read-only
- Passed between components

> What is Context API in React?

Context API provides a way to share state globally without prop drilling

- Create context
- Provide context value
- Consume context

```
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function ThemedComponent() {
  const theme = useContext(ThemeContext);
  return <p>Current Theme: {theme}</p>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedComponent />
    </ThemeContext.Provider>
  );
}

```

> What is Redux Toolkit?

Redux Toolkit (RTK) is a simplified way to manage global state in React applications, reducing boilerplate compared to traditional Redux.

- install redux toolkit
```
npm install @reduxjs/toolkit react-redux
```
- create a slice
```
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./counterSlice";

const store = configureStore({ reducer: { counter: counterReducer } });
export default store;
```
- setup store
```
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./counterSlice";

const store = configureStore({ reducer: { counter: counterReducer } });
export default store;
```
- use in component
```
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement } from "./counterSlice";

function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
    </div>
  );
}
```

> What are the key features of Redux Toolkit?

- createSlice – Simplifies reducers and actions.
- configureStore – Configures store with built-in middleware.
- createAsyncThunk – Handles asynchronous requests.

> What is a breadcrumb in web applications?

A breadcrumb is a navigation UI pattern that shows the user’s current location in an app.
Use React Router to track the navigation path.
```
import { Link, useLocation } from "react-router-dom";

function Breadcrumb() {
  const location = useLocation();
  const paths = location.pathname.split("/").filter((x) => x);

  return (
    <nav>
      {paths.map((path, index) => (
        <span key={index}>
          <Link to={"/" + paths.slice(0, index + 1).join("/")}>{path}</Link> /
        </span>
      ))}
    </nav>
  );
}
```
