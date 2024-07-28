# Part 1

Faster than npm or yarn

```jsx
npm install -g pnpm
```

mkdir ->

```jsx
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

- **`/app`**: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
- **`/app/lib`**: Contains functions used in your application, such as reusable utility functions and data fetching functions.
- **`/app/ui`**: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
- **`/public`**: Contains all the static assets for your application, such as images.
- **Config Files**: You'll also notice config files such as `next.config.js` at the root of your application. Most of these files are created and pre-configured when you start a new project using `create-next-app`. You will not need to modify them in this course.

### [**Placeholder data**](https://nextjs.org/learn/dashboard-app/getting-started#placeholder-data)

When you're building user interfaces, it helps to have some placeholder data. If a database or API is not yet available, you can:

- Use placeholder data in JSON format or as JavaScript objects.
- Use a 3rd party service like [mockAPI](https://mockapi.io/).

For this project, we've provided some placeholder data in `app/lib/placeholder-data.ts`. Each JavaScript object in the file represents a table in your database. For example, for the invoices table:

For now, take a look at the `/app/lib/definitions.ts` file. Here, we manually define the types that will be returned from the database. For example, the invoices table has the following types:

```jsx
pnpm i
pnpm dev
```

# Part 2

If you look inside the `/app/ui` folder, you'll see a file called `global.css`. You can use this file to add CSS rules to **all** the routes in your application - such as CSS reset rules, site-wide styles for HTML elements like links, and more.

You can import `global.css` in any component in your application, but it's usually good practice to add it to your top-level component. In Next.js, this is the [root layout](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#root-layout-required) (more on this later).

### Tailwind

classnames, inline styles

### Css modules

Inside `/app/ui`, create a new file called `home.module.css` and add the following CSS rules:

```jsx
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

[CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support) allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions as well.

### clsx for toggling classnames

There may be cases where you may need to conditionally style an element based on state or some other condition.

[`clsx`](https://www.npmjs.com/package/clsx) is a library that lets you toggle class names easily. We recommend taking a look at [documentation](https://github.com/lukeed/clsx) for more details, but here's the basic usage:

- Suppose that you want to create an `InvoiceStatus` component which accepts `status`. The status can be `'pending'` or `'paid'`.
- If it's `'paid'`, you want the color to be green. If it's `'pending'`, you want the color to be gray.

You can use `clsx` to conditionally apply the classes, like this:

```jsx
/app/ui/invoices/status.tsx

import clsx from 'clsx';

export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
```

# Part 3

## [**Why optimize fonts?**](https://nextjs.org/learn/dashboard-app/optimizing-fonts-images#why-optimize-fonts)

Fonts play a significant role in the design of a website, but using custom fonts in your project can affect performance if the font files need to be fetched and loaded.

[Cumulative Layout Shift](https://vercel.com/blog/how-core-web-vitals-affect-seo) is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

Next.js automatically optimizes fonts in the application when you use the `next/font` module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

In your `/app/ui` folder, create a new file called `fonts.ts`. You'll use this file to keep the fonts that will be used throughout your application.

Import the `Inter` font from the `next/font/google` module - this will be your primary font. Then, specify what [subset](https://fonts.google.com/knowledge/glossary/subsetting) you'd like to load. In this case, `'latin'`:

## Image Optimization

Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the `next/image` component to automatically optimize your images.

Let's use the `<Image>` component. If you look inside the `/public` folder, you'll see there are two images: `hero-desktop.png` and `hero-mobile.png`. These two images are completely different, and they'll be shown depending if the user's device is a desktop or mobile.

You'll also notice the class `hidden` to remove the image from the DOM on mobile screens, and `md:block` to show the image on desktop screens.

**`md:block`**: This class overrides the `hidden` class and makes the element visible again (with `display: block;`) on medium and larger screens. In Tailwind CSS, `md` corresponds to a minimum screen width of 768 pixels (which is typically considered the start of tablet sizes).

There's a lot more to learn about these topics, including optimizing remote images and using local font files. If you'd like to dive deeper into fonts and images, see:

- [Image Optimization Docs](https://nextjs.org/docs/app/building-your-application/optimizing/images)
- [Font Optimization Docs](https://nextjs.org/docs/app/building-your-application/optimizing/fonts)
- [Improving Web Performance with Images (MDN)](https://developer.mozilla.org/en-US/docs/Learn/Performance/Multimedia)
- [Web Fonts (MDN)](https://developer.mozilla.org/en-US/docs/Learn/CSS/Styling_text/Web_fonts)
- [How Core Web Vitals Affect SEO](https://vercel.com/blog/how-core-web-vitals-affect-seo)

# Part 4

`page.tsx` is a special Next.js file that exports a React component, and it's required for the route to be accessible. In your application, you already have a page file: `/app/page.tsx` - this is the home page associated with the route `/`.

Create a new folder called `dashboard` inside `/app`. Then, create a new `page.tsx` file inside the `dashboard` folder with the following content:

By having a special name for `page` files, Next.js allows you to [colocate](https://nextjs.org/docs/app/building-your-application/routing#colocation) UI components, test files, and other related code with your routes. Only the content inside the `page` file will be publicly accessible. For example, the `/ui` and `/lib` folders are *colocated* inside the `/app` folder along with your routes.

pnpm add next@canary

### Layout

Dashboards have some sort of navigation that is shared across multiple pages. In Next.js, you can use a special layout.tsx file to create UI that is shared between multiple pages. Let's create a layout for the dashboard pages!

Inside the /dashboard folder, add a new file called layout.tsx and paste the following code:

/app/dashboard/layout.tsx

```
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
```

A few things are going on in this code, so let's break it down:

First, you're importing the <SideNav /> component into your layout. Any components you import into this file will be part of the layout.

The <Layout /> component receives a children prop. This child can either be a page or another layout. In your case, the pages inside /dashboard will automatically be nested inside a <Layout /> like so:

## Partial rendering

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering:

A root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata (you'll learn more about metadata in a later chapter).

# Part 5

- How to use the next/link component.

- How to show an active link with the usePathname() hook.

- How navigation works in Next.js.

In Next.js, you can use the <Link /> Component to link between pages in your application. Link allows you to do client-side navigation with JavaScript.

To use the <Link /> component, open /app/ui/dashboard/nav-links.tsx, and import the Link component from next/link. Then, replace the 'a' tag with Link:

- Hace que no se haga un full refresh, haciendo que la app se vea mas suave al tocar un link

### Code splitting

To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

router.prefetch(): The useRouter hook can be used to prefetch routes programmatically.

Pattern: Showing active links
A common UI pattern is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called usePathname() that you can use to check the path and implement this pattern.

Since usePathname() is a hook, you'll need to turn nav-links.tsx into a Client Component. Add React's "use client" directive to the top of the file, then import usePathname() from next/navigation:

```
'use client';

import {
  UserGroupIcon,
  HomeIcon,
  InboxIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
// ...
```

# Part 6

- Push your project to GitHub.

- Set up a Vercel account and link your GitHub repo for instant previews and deployments.

- Create and link your project to a Postgres database.

- Seed the database with initial data.

By connecting your GitHub repository, whenever you push changes to your main branch, Vercel will automatically redeploy your application with no configuration needed. When opening pull requests, you'll also have instant previews which allow you to catch deployment errors early and share a preview of your project with team members for feedback.

If you have created a pull or merge request when using Git, the generated preview URL will be available as a comment from the Vercel bot. This URL will always reflect the latest deployment changes.

pnpm i @vercel/postgres

Inside of /app, there's a folder called seed. Uncomment this file. This folder contains a Next.js Route Handler, called route.ts, that will be used to seed your database. This creates a server-side endpoint that you can access in the browser to start populating your database.

Don't worry if you don't understand everything the code is doing, but to give you an overview, the script uses SQL to create the tables, and the data from placeholder-data.ts file to populate them after they've been created.

Ensure your local development server is running with pnpm run dev and navigate to localhost:3000/seed in your browser. When finished, you will see a message "Database seeded successfully" in the browser. Once completed, you can delete this file.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

# Part 7

- Learn about some approaches to fetching data: APIs, ORMs, SQL, etc.

- How Server Components can help you access back-end resources more securely.

- What network waterfalls are.

- How to implement parallel data fetching using a JavaScript Pattern.

In Next.js, you can create API endpoints using Route Handlers.

There are a few cases where you have to write database queries:

- When creating your API endpoints, you need to write logic to interact with your database.

- If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.

### React server components

By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

- Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching. You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
- Server Components execute on the server, so you can keep expensive data fetches and logic on the server and only send the result to the client.
- As mentioned before, since Server Components execute on the server, you can query the database directly without an additional API layer.

- The Vercel Postgres SDK provides protection against SQL injections.

### Problems...

However... there are two things you need to be aware of:

1. The data requests are unintentionally blocking each other, creating a request waterfall.

- For example, we need to wait for fetchRevenue() to execute before fetchLatestInvoices() can start running, and so on.

- A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

- In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time. For example, in data.ts, we're using Promise.all() in the fetchCardData() function

- However, there is one disadvantage of relying only on this JavaScript pattern: what happens if one data request is slower than all the others?

2. By default, Next.js prerenders routes to improve performance, this is called Static Rendering. So if your data changes, it won't be reflected in your dashboard.

# Part 8

- What static rendering is and how it can improve your application's performance.

- What dynamic rendering is and when to use it.

- Different approaches to make your dashboard dynamic.

- Simulate a slow data fetch to see what happens.

## Static rendering

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or when revalidating data.

Benefits:

- Faster Websites - Prerendered content can be cached and globally distributed. This ensures that users around the world can access your website's content more quickly and reliably.
- Reduced Server Load - Because the content is cached, your server does not have to dynamically generate content for each user request.
- SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page. It might not be a good fit for a dashboard that has personalized data which is regularly updated.

The opposite of static rendering is dynamic rendering.

## Dinamic rendering

With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). There are a couple of benefits of dynamic rendering:

- Real-Time Data - Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
- User-Specific Content - It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.
- Request Time Information - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.

Here, you've added an artificial 3-second delay to simulate a slow data fetch. The result is that now your whole page is blocked from showing UI to the visitor while the data is being fetched. Which brings us to a common challenge developers have to solve:

With dynamic rendering, your application is only as fast as your slowest data fetch.

# Part 9, streaming

- What streaming is and when you might use it.

- How to implement streaming with loading.tsx and Suspense.

- What loading skeletons are.

- What route groups are, and when you might use them.

- Where to place Suspense boundaries in your application.

### What is streaming?

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

There are two ways you implement streaming in Next.js:

At the page level, with the loading.tsx file.
For specific components, with <Suspense>.

A few things are happening here:

- loading.tsx is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.
- Since <SideNav> is static, it's shown immediately. The user can interact with <SideNav> while the dynamic content is loading.
- The user doesn't have to wait for the page to finish loading before navigating away (this is called interruptable navigation).

### Loading Skeletons

Adding loading skeletons
A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading. Any UI you add in loading.tsx will be embedded as part of the static file, and sent first. Then, the rest of the dynamic content will be streamed from the server to the client.

## Route groups

Right now, your loading skeleton will apply to the invoices and customers pages as well.

Since loading.tsx is a level higher than /invoices/page.tsx and /customers/page.tsx in the file system, it's also applied to those pages.

We can change this with Route Groups. Create a new folder called /(overview) inside the dashboard folder. Then, move your loading.tsx and page.tsx files inside the folder:

Now, the loading.tsx file will only apply to your dashboard overview page.

Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path. So /dashboard/(overview)/page.tsx becomes /dashboard.

Here, you're using a route group to ensure loading.tsx only applies to your dashboard overview page. However, you can also use route groups to separate your application into sections (e.g. (marketing) routes and (shop) routes) or by teams for larger applications.

## Streaming a component

So far, you're streaming a whole page. But you can also be more granular and stream specific components using React Suspense.

Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

If you remember the slow data request, fetchRevenue(), this is the request that is slowing down the whole page. Instead of blocking your whole page, you can use Suspense to stream only this component and immediately show the rest of the page's UI.

To do so, you'll need to move the data fetch to the component, let's update the code to see what that'll look like

## Wrapper pattern

now you need to wrap the <Card> components in Suspense. You can fetch data for each individual card, but this could lead to a popping effect as the cards load in, this can be visually jarring for the user.

So, how would you tackle this problem?

To create more of a staggered effect, you can group the cards using a wrapper component. This means the static <SideNav/> will be shown first, followed by the cards, etc.

- You could stream the whole page like we did with loading.tsx... but that may lead to a longer loading time if one of the components has a slow data fetch.
- You could stream every component individually... but that may lead to UI popping into the screen as it becomes ready.
- You could also create a staggered effect by streaming page sections. But you'll need to create wrapper components.

Where you place your suspense boundaries will vary depending on your application. In general, it's good practice to move your data fetches down to the components that need it, and then wrap those components in Suspense. But there is nothing wrong with streaming the sections or the whole page if that's what your application needs.

# Part 10, partial rendering

- Es como una combinacion entre static y dynamic rendering

However, most routes are not fully static or dynamic. For example, consider an ecommerce site. You might want to statically render the majority of the product information page, but you may want to fetch the user's cart and recommended products dynamically, this allows you show personalized content to your users.

Next.js 14 introduced an experimental version of Partial Prerendering – a new rendering model that allows you to combine the benefits of static and dynamic rendering in the same route. For example:

When a user visits a route:

- A static route shell that includes the navbar and product information is served, ensuring a fast initial load.
- The shell leaves holes where dynamic content like the cart and recommended products will load in asynchronously.
- The async holes are streamed in parallel, reducing the overall load time of the page.

How does Partial Prerendering work?
Partial Prerendering uses React's Suspense (which you learned about in the previous chapter) to defer rendering parts of your application until some condition is met (e.g. data is loaded).

The Suspense fallback is embedded into the initial HTML file along with the static content. At build time (or during revalidation), the static content is prerendered to create a static shell. The rendering of dynamic content is postponed until the user requests the route.

Implementing Partial Prerendering
Enable PPR for your Next.js app by adding the ppr option to your next.config.mjs file

```
/** @type {import('next').NextConfig} */

const nextConfig = {
  experimental: {
    ppr: 'incremental',
  },
};

export default nextConfig;
```

Next, add the experimental_ppr segment config option to your dashboard layout

That's it. You may not see a difference in your application in development, but you should notice a performance improvement in production. Next.js will prerender the static parts of your route and defer the dynamic parts until the user requests them.

The great thing about Partial Prerendering is that you don't need to change your code to use it. As long as you're using Suspense to wrap the dynamic parts of your route, Next.js will know which parts of your route are static and which are dynamic.

### Optimizing summary

- Created a database in the same region as your application code to reduce latency between your server and database.
- Fetched data on the server with React Server Components. This allows you to keep expensive data fetches and logic on the server, reduces the client-side JavaScript bundle, and prevents your database secrets from being exposed to the client.
- Used SQL to only fetch the data you needed, reducing the amount of data transferred for each request and the amount of JavaScript needed to transform the data in-memory.
- Parallelize data fetching with JavaScript - where it made sense to do so.
- Implemented Streaming to prevent slow data requests from blocking your whole page, and to allow the user to start interacting with the UI without waiting for everything to load.
- Move data fetching down to the components that need it, thus isolating which parts of your routes should be dynamic.

# Part 11, Search and pagination

- Learn how to use the Next.js APIs: useSearchParams, usePathname, and useRouter.

- Implement search and pagination using URL search params.

1. <Search/> allows users to search for specific invoices.
2. <Pagination/> allows users to navigate between pages of invoices.
3. <Table/> displays the invoices.

Your search functionality will span the client and the server. When a user searches for an invoice on the client, the URL params will be updated, data will be fetched on the server, and the table will re-render on the server with the new data.

There are a couple of benefits of implementing search with URL params:

- Bookmarkable and Shareable URLs: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
- Server-Side Rendering and Initial Load: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
- Analytics and Tracking: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

## Search functionality provided by next

- useSearchParams- Allows you to access the parameters of the current URL. For example, the search params for this URL /dashboard/invoices?page=1&query=pending would look like this: {page: '1', query: 'pending'}.
- usePathname - Lets you read the current URL's pathname. For example, for the route /dashboard/invoices, usePathname would return '/dashboard/invoices'.
- useRouter - Enables navigation between routes within client components programmatically. There are multiple methods you can use.

### useRouter()

- router.push(href: string, { scroll: boolean }): Perform a client-side navigation to the provided route. Adds a new entry into the browser’s history stack.
- router.replace(href: string, { scroll: boolean }): Perform a client-side navigation to the provided route without adding a new entry into the browser’s history stack.
- router.refresh(): Refresh the current route. Making a new request to the server, re-fetching data requests, and re-rendering Server Components. The client will merge the updated React Server Component payload without losing unaffected client-side React (e.g. useState) or browser state (e.g. scroll position).
- router.prefetch(href: string): Prefetch the provided route for faster client-side transitions.
- router.back(): Navigate back to the previous route in the browser’s history stack.
- router.forward(): Navigate forwards to the next page in the browser’s history stack.

Here's a quick overview of the implementation steps:

1. Capture the user's input.
2. Update the URL with the search params.
3. Keep the URL in sync with the input field.
4. Update the table to reflect the search query.

1 - Capture the user's input
Go into the <Search> Component (/app/ui/search.tsx), and you'll notice:

"use client" - This is a Client Component, which means you can use event listeners and hooks.
<input> - This is the search input.
Create a new handleSearch function, and add an onChange listener to the <input> element. onChange will invoke handleSearch whenever the input value changes.

2 - Update the URL with the search params
Import the useSearchParams hook from 'next/navigation', and assign it to a variable:

```
  function handleSearch(term: string) {
    const params = new URLSearchParams(searchParams);
    if (term) {
      params.set("query", term);
    } else {
      params.delete("query");
    }
  }
```

Now that you have the query string. You can use Next.js's useRouter and usePathname hooks to update the URL.

Import useRouter and usePathname from 'next/navigation', and use the replace method from useRouter() inside handleSearch:

3 - Keeping the URL and input in sync
To ensure the input field is in sync with the URL and will be populated when sharing, you can pass a
defaultValue to input by reading from searchParams:

```
<input
  className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
  placeholder={placeholder}
  onChange={(e) => {
    handleSearch(e.target.value);
  }}
  defaultValue={searchParams.get('query')?.toString()}
/>
```

### defaultValue vs. value / Controlled vs. Uncontrolled

If you're using state to manage the value of an input, you'd use the value attribute to make it a controlled component. This means React would manage the input's state.

However, since you're not using state, you can use defaultValue. This means the native input will manage its own state. This is okay since you're saving the search query to the URL instead of state.

### When to use the useSearchParams() hook vs. the searchParams prop?

You might have noticed you used two different ways to extract search params. Whether you use one or the other depends on whether you're working on the client or the server.

<Search> is a Client Component, so you used the useSearchParams() hook to access the params from the client.

<Table> is a Server Component that fetches its own data, so you can pass the searchParams prop from the page to the component.
As a general rule, if you want to read the params from the client, use the useSearchParams() hook as this avoids having to go back to the server.

## Debouncing (optimization)

You're updating the URL on every keystroke, and therefore querying your database on every keystroke! This isn't a problem as our application is small, but imagine if your application had thousands of users, each sending a new request to your database on each keystroke.

Debouncing is a programming practice that limits the rate at which a function can fire. In our case, you only want to query the database when the user has stopped typing.

How Debouncing Works:

1. Trigger Event: When an event that should be debounced (like a keystroke in the search box) occurs, a timer starts.
2. Wait: If a new event occurs before the timer expires, the timer is reset.
3. Execution: If the timer reaches the end of its countdown, the debounced function is executed.

You can implement debouncing in a few ways, including manually creating your own debounce function. To keep things simple, we'll use a library called use-debounce.

Install use-debounce

```
pnpm i use-debounce
```

## Pagination

# Part 12, Mutating data

- What React Server Actions are and how to use them to mutate data.

- How to work with forms and Server Components.

- Best practices for working with the native formData object, including type validation.

- How to revalidate the client cache using the revalidatePath API.

- How to create dynamic route segments with specific IDs.

## What are Server Actions?

React Server Actions allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

Security is a top priority for web applications, as they can be vulnerable to various threats. This is where Server Actions come in. They offer an effective security solution, protecting against different types of attacks, securing your data, and ensuring authorized access. Server Actions achieve this through techniques like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.

## Using forms with Server Actions

In React, you can use the action attribute in the <form> element to invoke actions. The action will automatically receive the native FormData object, containing the captured data.

```
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';

    // Logic to mutate data...
  }

  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

## Next.js with Server Actions

Server Actions are also deeply integrated with Next.js caching. When a form is submitted through a Server Action, not only can you use the action to mutate data, but you can also revalidate the associated cache using APIs like revalidatePath and revalidateTag.

### Creating a server action

```
'use server';
```

By adding the 'use server', you mark all the exported functions within the file as Server Actions. These server functions can then be imported and used in Client and Server components.

You can also write Server Actions directly inside Server Components by adding "use server" inside the action. But for this course, we'll keep them all organized in a separate file.

Then, in your <Form> component, import the createInvoice from your actions.ts file. Add a action attribute to the <form> element, and call the createInvoice action.

```
import { customerField } from '@/app/lib/definitions';
import Link from 'next/link';
import {
  CheckIcon,
  ClockIcon,
  CurrencyDollarIcon,
  UserCircleIcon,
} from '@heroicons/react/24/outline';
import { Button } from '@/app/ui/button';
import { createInvoice } from '@/app/lib/actions';

export default function Form({
  customers,
}: {
  customers: customerField[];
}) {
  return (
    <form action={createInvoice}>
      // ...
  )
}
```

Good to know: In HTML, you'd pass a URL to the action attribute. This URL would be the destination where your form data should be submitted (usually an API endpoint).

However, in React, the action attribute is considered a special prop - meaning React builds on top of it to allow actions to be invoked.

Behind the scenes, Server Actions create a POST API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

actions.ts:

```
'use server';

export async function createInvoice(formData: FormData) {
  const rawFormData = {
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  };
  // Test it out:
  console.log(rawFormData);
}
```

Tip: If you're working with forms that have many fields, you may want to consider using the entries() method with JavaScript's Object.fromEntries(). For example:

```
const rawFormData = Object.fromEntries(formData.entries())
```

## Validations, zod

To handle type validation, you have a few options. While you can manually validate types, using a type validation library can save you time and effort. For your example, we'll use Zod, a TypeScript-first validation library that can simplify this task for you.

Storing values in cents
It's usually good practice to store monetary values in cents in your database to eliminate JavaScript floating-point errors and ensure greater accuracy.

Let's convert the amount into cents:

```
// ...
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });
  const amountInCents = amount * 100;
}
```

### Revalidate (cache)

Next.js has a Client-side Router Cache that stores the route segments in the user's browser for a time. Along with prefetching, this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.

Since you're updating the data displayed in the invoices route, you want to clear this cache and trigger a new request to the server. You can do this with the revalidatePath function from Next.js:

Once the database has been updated, the /dashboard/invoices path will be revalidated, and fresh data will be fetched from the server.

At this point, you also want to redirect the user back to the /dashboard/invoices page. You can do this with the redirect function from Next.js

## Updating

1. Create a Dynamic Route Segment with the invoice id
   Next.js allows you to create Dynamic Route Segments when you don't know the exact segment name and want to create routes based on data. This could be blog post titles, product pages, etc. You can create dynamic route segments by wrapping a folder's name in square brackets. For example, [id], [post] or [slug].

In your /invoices folder, create a new dynamic route called [id], then a new route called edit with a page.tsx file.

Notice how it's similar to your /create invoice page, except it imports a different form (from the edit-form.tsx file). This form should be pre-populated with a defaultValue for the customer's name, invoice amount, and status. To pre-populate the form fields, you need to fetch the specific invoice using id.

In addition to searchParams, page components also accept a prop called params which you can use to access the id. Update your <Page> component to receive the prop:

4 - Pass the id to the Server Action
Lastly, you want to pass the id to the Server Action so you can update the right record in your database. You cannot pass the id as an argument like so:

/app/ui/invoices/edit-form.tsx

```
// Passing an id as argument won't work
<form action={updateInvoice(id)}>
```

Instead, you can pass id to the Server Action using JS bind. This will ensure that any values passed to the Server Action are encoded.

/app/ui/invoices/edit-form.tsx

```

// ...
import { updateInvoice } from '@/app/lib/actions';

export default function EditInvoiceForm({
  invoice,
  customers,
}: {
  invoice: InvoiceForm;
  customers: CustomerField[];
}) {
  const updateInvoiceWithId = updateInvoice.bind(null, invoice.id);

  return (
    <form action={updateInvoiceWithId}>
      <input type="hidden" name="id" value={invoice.id} />
    </form>
  );
}

```

Note: Using a hidden input field in your form also works (e.g. <input type="hidden" name="id" value={invoice.id} />). However, the values will appear as full text in the HTML source, which is not ideal for sensitive data like IDs.

# Part 13, Handling errors

- How to use the special error.tsx file to catch errors in your route segments, and show a fallback UI to the user.

- How to use the notFound function and not-found file to handle 404 errors (for resources that don’t exist).

## Handling all errors with error.tsx

The error.tsx file can be used to define a UI boundary for a route segment. It serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users.

There are a few things you'll notice about the code above:

- "use client" - error.tsx needs to be a Client Component.
  It accepts two props:
- error: This object is an instance of JavaScript's native Error object.
- reset: This is a function to reset the error boundary. When executed, the function will try to re-render the route segment.

### Handling 404 errors with the notFound function

Another way you can handle errors gracefully is by using the notFound function. While error.tsx is useful for catching all errors, notFound can be used when you try to fetch a resource that doesn't exist.

For example, visit http://localhost:3000/dashboard/invoices/2e94d1ed-d220-449f-9f11-f0bbceed9645/edit.

This is a fake UUID that doesn't exist in your database.

You'll immediately see error.tsx kicks in because this is a child route of /invoices where error.tsx is defined.

However, if you want to be more specific, you can show a 404 error to tell the user the resource they're trying to access hasn't been found.

```
// Goes into possible notFound info (query db)

import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
import { updateInvoice } from '@/app/lib/actions';
import { notFound } from 'next/navigation';

export default async function Page({ params }: { params: { id: string } }) {
  const id = params.id;
  const [invoice, customers] = await Promise.all([
    fetchInvoiceById(id),
    fetchCustomers(),
  ]);

  if (!invoice) {
    notFound();
  }

  // ...
}
```

Perfect! <Page> will now throw an error if a specific invoice is not found. To show an error UI to the user. Create a not-found.tsx file inside the /edit folder.

```
import Link from 'next/link';
import { FaceFrownIcon } from '@heroicons/react/24/outline';

export default function NotFound() {
  return (
    <main className="flex h-full flex-col items-center justify-center gap-2">
      <FaceFrownIcon className="w-10 text-gray-400" />
      <h2 className="text-xl font-semibold">404 Not Found</h2>
      <p>Could not find the requested invoice.</p>
      <Link
        href="/dashboard/invoices"
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
      >
        Go Back
      </Link>
    </main>
  );
}
```

That's something to keep in mind, notFound will take precedence over error.tsx, so you can reach out for it when you want to handle more specific errors!

To learn more about error handling in Next.js, check out the following documentation:

Error Handling
error.js API Reference
notFound() API Reference
not-found.js API Reference

# Part 14, Improving accesibility (server form validation)

- How to use eslint-plugin-jsx-a11y with Next.js to implement accessibility best practices.

- How to implement server-side form validation.

- How to use the React useActionState hook to handle form errors, and display them to the user.

## Using the ESLint accessibility plugin in Next.js

Next.js includes the eslint-plugin-jsx-a11y plugin in its ESLint config to help catch accessibility issues early. For example, this plugin warns if you have images without alt text, use the aria-\* and role attributes incorrectly, and more.

Optionally, if you would like to try this out, add next lint as a script in your package.json file:

```
"scripts": {
    "build": "next build",
    "dev": "next dev",
    "start": "next start",
    "lint": "next lint"
},
```

Improving form accessibility
There are three things we're already doing to improve accessibility in our forms:

- Semantic HTML: Using semantic elements (<input>, <option>, etc) instead of <div>. This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user, making the form easier to navigate and understand.
- Labelling: Including <label> and the htmlFor attribute ensures that each form field has a descriptive text label. This improves AT support by providing context and also enhances usability by allowing users to click on the label to focus on the corresponding input field.
- Focus Outline: The fields are properly styled to show an outline when they are in focus. This is critical for accessibility as it visually indicates the active element on the page, helping both keyboard and screen reader users to understand where they are on the form. You can verify this by pressing tab.

## Client-Side validation

There are a couple of ways you can validate forms on the client. The simplest would be to rely on the form validation provided by the browser by adding the required attribute to the <input> and <select> elements in your forms.

## Server-Side validation

By validating forms on the server, you can:

Ensure your data is in the expected format before sending it to your database.
Reduce the risk of malicious users bypassing client-side validation.
Have one source of truth for what is considered valid data.
In your create-form.tsx component, import the useActionState hook from react. Since useActionState is a hook, you will need to turn your form into a Client Component using "use client" directive

Inside your Form Component, the useActionState hook:

- Takes two arguments: (action, initialState).
- Returns two values: [state, formAction] - the form state, and a function to be called when the form is submitted.

### Cuando hay un input (de cualquier tipo)

```
aria-describedby="x-error"
```

```
// En este caso para el error de status
  <div id="status-error" aria-live="polite" aria-atomic="true">
    {state.errors?.status &&
      state.errors.status.map((error: string) => (
        <p className="mt-2 text-sm text-red-500" key={error}>
          {error}
        </p>
      ))}
  </div>
```

- aria-describedby="customer-error": This establishes a relationship between the select element and the error message container. It indicates that the container with id="customer-error" describes the select element. Screen readers will read this description when the user interacts with the select box to notify them of errors.
- id="customer-error": This id attribute uniquely identifies the HTML element that holds the error message for the select input. This is necessary for aria-describedby to establish the relationship.
- aria-live="polite": The screen reader should politely notify the user when the error inside the div is updated. When the content changes (e.g. when a user corrects an error), the screen reader will announce these changes, but only when the user is idle so as not to interrupt them.
