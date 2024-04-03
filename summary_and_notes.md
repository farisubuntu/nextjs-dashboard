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

---
## Chapter 3: Optimizing Fonts and Images

**Topics:**

- How to add custom fonts with `next/font`.
- How to add images with `next/image`.
- How fonts and images are optimized in Next.js.

**Some Notes:**

- `Fonts` play a significant role in the design of a website, but using custom fonts in your project can affect performance if the font files need to be fetched and loaded.
- `Next.js` automatically optimizes fonts in the application when you use the` next/font` module. It downloads font files at build time and hosts them with your other static assets.

#### Example of Adding a primary font

1. In your `/app/ui` folder, create a new file called `fonts.ts`. You'll use this file to keep the fonts that will be used throughout your application.

2. Import the `Inter` font from the `next/font/google` module - this will be your primary font. Then, specify what [subset](https://fonts.google.com/knowledge/glossary/subsetting) you'd like to load. In this case, `'latin'`.

```jsx
import { Inter } from 'next/font/google';
export const inter = Inter({ subsets: ['latin'] });
```

3. Finally, add the font to the `<body>` element in `/app/layout.tsx`

```jsx
// ...
// By adding Inter to the <body> element, the font will be applied throughout your application.
 <body className={`${inter.className} antialiased`}>{children}</body>
// ...
```
#### Practice: Adding a secondary font

In your `fonts.ts` file, import a secondary font called `Lusitana` and pass it to the `<p>` element in your `/app/page.tsx` file. In addition to specifying a subset like you did before, you'll also need to specify the font **weight**.

---

#### Optimize images

**Notes:**

- `Next.js` can serve static assets, like images, under the top-level `/public` folder. Files inside `/public` can be referenced in your application. For example:

```html
<img
  src="/hero.png"
  alt="Screenshots of the dashboard project showing desktop version"
/>
```

You can use the `next/image` component to automatically optimize your images.

The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimization, such as:

-   Preventing layout shift automatically when images are loading.
-   Resizing images to avoid shipping large images to devices with a smaller viewport.
-   Lazy loading images by default (images load as they enter the viewport).
-   Serving images in modern formats, like [WebP](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#webp) and [AVIF](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Image_types#avif_image), when the browser supports it.

---

**Pracice: Adding the desktop hero image**
Inside `/app/page.tsx` import and use `<Image ... />` component 

> **Note:** It's good practice to set the `width` and `height` of your images to avoid layout shift. Images without dimensions and web fonts are common causes of layout shift due to the browser having to download additional resources.

---


### Chapter 4: Creating Layouts and Pages

**Topics**

- Create the `dashboard` routes using file-system routing.
- Understand the role of folders and files when creating new route segments.
- Create a nested layout that can be shared between multiple dashboard pages.
- Understand what **colocation**, **partial rendering**, and the **root layout** are.

**Notes:**
- Each folder represents a **route segment** that maps to a `URL segment`.
- `page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible.
- To create a nested route, you can nest folders inside each other and add `page.tsx` files inside them. For example:

![Diagram showing how adding a folder called dashboard creates a new route '/dashboard'](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdashboard-route.png&w=3840&q=75)

This means you create a new route segment using a folder, and add a page file inside it.

> **NOTE:** a route is not publicly accessible until a `page.js` or `route.js` file is added to a route segment and, even when a route is made publicly accessible, only the content returned by `page.js` or `route.js` is sent to the client. This means that project files can be safely **colocated** inside route segments in the app directory without accidentally being routable.

- **Private folders** can be created by prefixing a folder with an underscore: `_folderName`. This indicates the folder and all its subfolders is a private implementation detail and should not be considered by the routing system (out of routing)

![An example folder structure using private folders](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fproject-organization-private-folders.png&w=3840&q=75)

**Private folders can be useful for:**
-   Separating UI logic from routing logic.
-   Consistently organizing internal files across a project and the `Next.js` ecosystem.
-   Sorting and grouping files in code editors.
-   Avoiding potential naming conflicts with future `Next.js` file conventions.

### [Route Groups](https://nextjs.org/docs/app/building-your-application/routing/colocation#route-groups)

- Route groups can be created by wrapping a folder in parenthesis: `(folderName)`. This indicates the folder is for organizational purposes and should **not be included** in the route's URL path.

![An example folder structure using route groups](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fproject-organization-route-groups.png&w=3840&q=75)

**Route groups are useful for:**

-   [Organizing routes into groups](https://nextjs.org/docs/app/building-your-application/routing/route-groups#organize-routes-without-affecting-the-url-path) e.g. by site section, intent, or team.
-   Enabling nested layouts in the same route segment level:
    -   [Creating multiple nested layouts in the same segment, including multiple root layouts](https://nextjs.org/docs/app/building-your-application/routing/route-groups#creating-multiple-root-layouts)
    -   [Adding a layout to a subset of routes in a common segment](https://nextjs.org/docs/app/building-your-application/routing/route-groups#opting-specific-segments-into-a-layout)

### [Module Path Aliases](https://nextjs.org/docs/app/building-your-application/routing/colocation#module-path-aliases)

`Next.js` supports [Module Path Aliases](https://nextjs.org/docs/app/building-your-application/configuring/absolute-imports-and-module-aliases) which make it easier to read and maintain imports across deeply nested project files.

```jsx
// app/dashboard/settings/analytics/page.js
// before
import { Button } from '../../../components/button'
 
// after
import { Button } from '@/components/button'
```

## [Project organization strategies](https://nextjs.org/docs/app/building-your-application/routing/colocation#project-organization-strategies)

- There is no "right" or "wrong" way so choose a strategy that works for you and your team and be consistent across the project.

**Here, the high-level of common strategies:**

- *Store project files outside of `app`*: This strategy stores all application code in shared folders in the **root of your project** and keeps the `app` directory purely for routing purposes.

![An example folder structure with project files outside of app](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fproject-organization-project-root.png&w=3840&q=75)

### [](https://nextjs.org/docs/app/building-your-application/routing/colocation#store-project-files-in-top-level-folders-inside-of-app)

- *Store project files in top-level folders inside of `app`*: This strategy stores all application code in shared folders in the **root of the `app` directory**.

![An example folder structure with project files inside app](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fproject-organization-app-root.png&w=3840&q=75)

### [](https://nextjs.org/docs/app/building-your-application/routing/colocation#split-project-files-by-feature-or-route)

- *Split project files by feature or route*: this strategy stores globally shared application code in the root `app` directory and **splits** more specific application code into the route segments that use them.

![An example folder structure with project files split by feature or route](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fproject-organization-app-root-split.png&w=3840&q=75)

---

- In Next.js, you can use a special `layout.tsx` file to create UI that is shared between multiple pages.
- Any components you import into `layout.tsx` file will be part of the layout.
- The `<Layout />` component receives a `children` prop. This child can either be a page or another layout. In your case, the **pages** inside `/dashboard` will automatically be nested inside a `<Layout />` like so:

![Folder structure with dashboard layout nesting the dashboard pages as children](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fshared-layout.png&w=3840&q=75)

- One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called [partial rendering](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#3-partial-rendering)

- Any UI you add to the root layout will be shared across all pages in your application.

- You can use the **root layout** to modify your `<html>` and `<body>` tags, and add `metadata`

---

## Chapter 5: Navigating Between Pages

- How to use the `next/link` component.
- How to show an active link with the `usePathname()` hook.
- How navigation works in `Next.js`.

---

**Notes:**

- `<Link>` allows you to do [client-side navigation](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#how-routing-and-navigation-works) with JavaScript.

**Example of using `<Link>` component:**

```jsx
// Map of links to display in the side navigation.
// Depending on the size of the application, this would be stored in a database.
const links = [
  { name: 'Home', href: '/dashboard', icon: HomeIcon },
  {
    name: 'Invoices',
    href: '/dashboard/invoices',
    icon: DocumentDuplicateIcon,
  },
  { name: 'Customers', href: '/dashboard/customers', icon: UserGroupIcon },
];

export default function NavLinks() {
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
          >
            <LinkIcon className="w-6" />
            <p className="hidden md:block">{link.name}</p>
          </Link>
        );
      })}
    </>
  );
}
```

- To improve the navigation experience, `Next.js` automatically code splits your application by route segments.
- Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.
-  In **production**, whenever `<Link>` components appear in the browser's viewport, `Next.js` automatically **prefetches** the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

- A common `UI pattern` is to show an *active link* to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the `URL`.`Next.js` provides a hook called `usePathname()` that you can use to check the path and implement this pattern.

> Since `usePathname()` is a hook, you'll need to turn `nav-links.tsx` into a *Client Component*. Add React's `"use client"` directive to the top of the file, then import `usePathname()` from `next/navigation`

- You can use the `clsx` library introduced in the chapter on `CSS styling` to conditionally apply class names when the link is active.

---

## Chapter 6: Setting up database

In this chapter...

- Push your project to GitHub.
- Set up a Vercel account and link your GitHub repo for instant previews and deployments.
- Create and link your project to a Postgres database.
- Seed the database with initial data.

---

**After connect to `github` and import the repo `nextjs-dashboard`, click deploy then to create a `Postgres` database:**

- click **Continute to Dashboard** and select **Storage** tab.
- Select **Connect Store** → **Create New** → **Postgres** → **Continue**.
- Accept the terms, assign a name to your database, and ensure your database region is set to **Washington D.C (iad1)** - this is also the [default region](https://vercel.com/docs/functions/serverless-functions/regions#select-a-default-serverless-region) for all new Vercel projects. By placing your database in the same region or close to your application code, you can reduce [latency](https://developer.mozilla.org/en-US/docs/Web/Performance/Understanding_latency) for data requests.

> **Note**: You cannot change the database region once it has been initalized. If you wish to use a different region, you should set it before creating a database.

- Once connected, navigate to the `.env.local` tab, click `Show secret` and `Copy Snippet`. Make sure you reveal the secrets before copying them.

- Navigate to your code editor and rename the `.env.example` file to `.env`. Paste in the copied contents from `Vercel`.

> **Important**: Go to your `.gitignore` file and make sure `.env` is in the ignored files to prevent your database secrets from being exposed when you push to `GitHub`.

- Finally, run `npm i @vercel/postgres` in your terminal to install the [Vercel Postgres SDK](https://vercel.com/docs/storage/vercel-postgres/sdk).

#### Seed your database

Now that your database has been created, let's seed it with some initial data. This will allow you to have some data to work with as you build the dashboard.

In the `/scripts` folder of your project, there's a file called `seed.js`. This script contains the instructions for creating and seeding the `invoices`, `customers`, `user`, `revenue` tables.

- Next, in your `package.json` file, add the following line to your scripts:

```json
"scripts": {
  "build": "next build",
  "dev": "next dev",
  "start": "next start",
  "seed": "node -r dotenv/config ./scripts/seed.js"
},
```
- Now, run `npm run seed`.

> What is 'seeding' in the context of databases?: A: *Populating the database with an initial set of data*


**Let's see what your database looks like. Go back to `Vercel`, and click Data on the `sidenav`.**

![Database screen showing dropdown list with four tables: users, customers, invoices, and revenue](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdatabase-tables.png&w=2048&q=75)

- By selecting each table, you can view its records and ensure the entries align with the data from `placeholder-data.js` file.

> You can switch to the "query" tab to interact with your database using `SQL`.

---



