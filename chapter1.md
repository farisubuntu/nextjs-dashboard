## Sample/Starter `nextjs` app structure:

-   **`/app`**: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
-   **`/app/lib`**: Contains functions used in your application, such as reusable utility functions and data fetching functions.
-   **`/app/ui`**: Contains all the UI components for your application, such as cards, tables, and forms.
-   **`/public`**: Contains all the static assets for your application, such as images.
-   **`/scripts`**: Contains a seeding script that you'll use to populate your database in a later chapter.
-   **Config Files**: config files such as `next.config.js` at the root of your application. Most of these files are created and pre-configured when you start a new project using `create-next-app`. You will not need to modify them in this course.

### Placeholder data

Its a sample data used temprarily when building user interfaces and no `database` or `API` available yet.
You can use:
- placeholder data in `JSON` format or as `Javascript` objects.
- a 3rd party service like [mockAPI - https://mockapi.io/](https://mockapi.io/)


> **If you're a TypeScript developer:**
> 
> -   We're manually declaring the data types, but for better type-safety, we recommend [Prisma](https://www.prisma.io/), which automatically generates types based on your database schema.
> -   Next.js detects if your project uses TypeScript and automatically installs the necessary packages and configuration. Next.js also comes with a [TypeScript plugin](https://nextjs.org/docs/app/building-your-application/configuring/typescript#typescript-plugin) for your code editor, to help with auto-completion and type-safety.

---

## Chapter 2 `CSS` styles

- `global.css` file can be used to add `CSS` rules to all the routes in your application - such as `CSS` reset rules, site-wide styles for `HTML` elements link links, and more
- Its good practice to add(`import`) it to your top-level such as `RootLayout` component. In `Next.js`, this is the root layout, for example :

```js
// /app/layout.tsx
import '@/app/ui/global.css'
export default function RootLayout({
 //...
```

> If you prefer writing traditional `CSS` rules or keeping your styles separate from your `JSX` - `CSS Modules` are a great alternative.

- `CSS Modules` allow you to scope `CSS` to a component by automatically creating unique class names, so you don't have to worry about style collisions as well. for example, create `/app/ui/home.module.css` file with some styles, then inside `/app/page.tsx` file import the styles:

```jsx
import styles from '@/app/ui/home.module.css';
<div className={styles.shape} />;
```

## [Using the `clsx` library to toggle class names](https://nextjs.org/learn/dashboard-app/getting-started#using-the-clsx-library-to-toggle-class-names)

There may be cases where you may need to conditionally style an element based on state or some other condition.

[`clsx`](https://www.npmjs.com/package/clsx) is a library that lets you toggle class names easily. for more see [https://github.com/lukeed/clsx](https://github.com/lukeed/clsx). 

**Example using `clsx` lib** => [https://nextjs.org/learn/dashboard-app/getting-started#using-the-clsx-library-to-toggle-class-names](https://nextjs.org/learn/dashboard-app/getting-started#using-the-clsx-library-to-toggle-class-names)
