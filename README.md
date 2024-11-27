This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

# Chapter 1: Getting Started

## Creating a new project

To create a Next.js app, open your terminal, cd into the folder you'd like to keep your project, and run the following command:

```bash
npx create-next-app@latest
```

After the prompts, create-next-app will create a folder with your project name and install the required dependencies.

Run 'npm run dev' to start the development server.

```bash
npm run dev
```

Visit http://localhost:3000 to view your application.

Edit theapp/page.tsx file and save it to see the updated result in your browser.

## Folder structure

/app: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.

/app/lib: Contains functions used in your application, such as reusable utility functions and data fetching functions.

/app/ui: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.

/public: Contains all the static assets for your application, such as images.

Config Files: You'll also notice config files such as next.config.js at the root of your application. Most of these files are created and pre-configured when you start a new project using create-next-app.

## Placeholder data

When you're building user interfaces, it helps to have some placeholder data. If a database or API is not yet available, you can:

- Use placeholder data in JSON format or as JavaScript objects.

- Use a 3rd party service like mockAPI.

For this project, we've provided some placeholder data in app/lib/placeholder-data.ts. Each JavaScript object in the file represents a table in your database. For example, for the invoices table:

```bash
# /app/lib/placeholder-data.json
{
    "customers": [
        {
            "customer_id": "1",
            "amount": 15795,
            "date": "2022-12-06",
            "status": "pending"
        },
        {
            "customer_id": "2",
            "amount": 20348,
            "date": "2022-11-14",
            "status": "pending"
        }
    ]
}

```

## TypeScript

Take a look at the /app/lib/definitions.ts file. Here, we manually define the types that will be returned from the database. For example, the invoices table has the following types:

```bash
# /app/lib/definitions.ts
export type Invoice = {
    id: string;
    customer_id: string;
    amount: number;
    date: string;
    status: "pending" | "paid";
};
```

# Chapter 2: CSS Styling

## Global styles

If you look inside the /app/ui folder, you'll see a file called global.css. You can use this file to add CSS rules to all the routes in your application.

You can import global.css in any component in your application, but it's usually good practice to add it to your top-level component.

```bash
# /app/layout.tsx
import "./globals.css";
```

## Tailwind

Tailwind is a CSS framework that speeds up the development process by allowing you to quickly write utility classes directly in your TSX markup.

In Tailwind, you style elements by adding class names. For example, adding the class "text-blue-500" will turn the <h1> text blue:

```bash
<h1 className="text-blue-500">I'm blue!</h1>
```

## CSS Modules

CSS Modules allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions as well.

```bash
# /app/ui/home.module.css
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

## Using the clsx library to toggle class names

clsx is a library that lets you toggle class names easily.

Suppose that you want to create an InvoiceStatus component which accepts status. The status can be 'pending' or 'paid'. If it's 'paid', you want the color to be green. If it's 'pending', you want the color to be gray.

You can use clsx to conditionally apply the classes, like this:

```bash
# /app/ui/invoices/status.tsx
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

# Chapter 3: Optimizing Fonts and Images

## Adding a primary font

In your /app/ui folder, create a new file called fonts.ts. You'll use this file to keep the fonts that will be used throughout your application.

```bash
# /app/ui/fonts.ts
import { Inter } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });
```

Finally, add the font to the <body> element in /app/layout.tsx:

```bash
# /app/layout.tsx

import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

By adding Inter to the <body> element, the font will be applied throughout your application. Here, you're also adding the Tailwind antialiased class which smooths out the font. It's not necessary to use this class, but it adds a nice touch.

## Adding a secondary font

You can also add fonts to specific elements of your application.

In your fonts.ts file, import a secondary font called Lusitana and pass it to the <p> element in your /app/page.tsx file. In addition to specifying a subset like you did before, you'll also need to specify the font weight.

## The <Image> component

The <Image> Component is an extension of the HTML <img> tag, and comes with automatic image optimization, such as:

- Preventing layout shift automatically when images are loading.
- Resizing images to avoid shipping large images to devices with a smaller viewport.
- Lazy loading images by default (images load as they enter the viewport).
- Serving images in modern formats, like WebP and AVIF, when the browser supports it.

Let's use the <Image> component. If you look inside the /public folder, you'll see there are two images: hero-desktop.png and hero-mobile.png. These two images are completely different, and they'll be shown depending if the user's device is a desktop or mobile.

In your /app/page.tsx file, import the component from next/image. Then, add the image under the comment:

```bash
# /app/page.tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';
import Image from 'next/image';

export default function Page() {
  return (
    // ...
    <div className="flex items-center justify-center p-6 md:w-3/5 md:px-28 md:py-12">
      {/* Add Hero Images Here */}
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    </div>
    //...
  );
}
```

Here, you're setting the width to 1000 and height to 760 pixels. It's good practice to set the width and height of your images to avoid layout shift, these should be an aspect ratio identical to the source image.

You'll also notice the class hidden to remove the image from the DOM on mobile screens, and md:block to show the image on desktop screens.

Under the image you've just added, add another <Image> component for hero-mobile.png.

```bash
<Image
    src="/hero-mobile.png"
    width={560}
    height={620}
    className="block md:hidden"
    alt="Screenshot of the dashboard project showing mobile version"
/>
```

# Chapter 4: Creating Layouts and Pages

## Nested routing

Next.js uses file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

You can create separate UIs for each route using layout.tsx and page.tsx files.

page.tsx is a special Next.js file that exports a React component, and it's required for the route to be accessible. In your application, you already have a page file: /app/page.tsx - this is the home page associated with the route /.

To create a nested route, you can nest folders inside each other and add page.tsx files inside them. For example: /app/dashboard/page.tsx is associated with the /dashboard path.

```bash
# /app/dashboard/page.tsx
export default function Page() {
  return <p>Dashboard Page</p>;
}
```

## Route layout

Dashboards have some sort of navigation that is shared across multiple pages. In Next.js, you can use a special layout.tsx file to create UI that is shared between multiple pages. Let's create a layout for the dashboard pages!

Inside the /dashboard folder, add a new file called layout.tsx and paste the following code:

```bash
# /app/dashboard/layout.tsx
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

First, you're importing the SideNav component into your layout. Any components you import into this file will be part of the layout.

The Layout component receives a children prop. This child can either be a page or another layout. In your case, the pages inside /dashboard will automatically be nested inside a Layout.

One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering.

## Root layout

/app/layout.tsx is called a root layout and is required. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your html and body tags, and add metadata.

# Chapter 5: Navigating Between Pages

## Why optimize navigation?

To link between pages, you'd traditionally use the <a> HTML element. At the moment, the sidebar links use <a> elements, but notice what happens when you navigate between the home, invoices, and customers pages on your browser.

Did you see it?

There's a full page refresh on each page navigation!

## The <Link> component

In Next.js, you can use the <Link /> Component to link between pages in your application. <Link> allows you to do client-side navigation with JavaScript.

To use the <Link /> component, open /app/ui/dashboard/nav-links.tsx, and import the Link component from next/link. Then, replace the <a> tag with <Link>:

```bash
# /app/ui/dashboard/nav-links.tsx
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';

// ...

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

You should now be able to navigate between the pages without seeing a full refresh. Although parts of your application are rendered on the server, there's no full page refresh, making it feel like a web app.

## Automatic code-splitting and prefetching

To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

Furthermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

## Pattern: Showing active links

A common UI pattern is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called usePathname() that you can use to check the path and implement this pattern.

Since usePathname() is a hook, you'll need to turn nav-links.tsx into a Client Component. Add React's "use client" directive to the top of the file, then import usePathname() from next/navigation:

```bash
# /app/ui/dashboard/nav-links.tsx
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

Next, assign the path to a variable called pathname inside your <NavLinks /> component:

```bash
# /app/ui/dashboard/nav-links.tsx
export default function NavLinks() {
  const pathname = usePathname();
  // ...
}
```

You can use the clsx library introduced in the chapter on CSS styling to conditionally apply class names when the link is active. When link.href matches the pathname, the link should be displayed with blue text and a light blue background.

Here's the final code for nav-links.tsx:

```bash
# /app/ui/dashboard/nav-links.tsx
<Link
  key={link.name}
  href={link.href}
  className={clsx(
    'flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3',
    {
      'bg-sky-100 text-blue-600': pathname === link.href,
    },
  )}
>
  <LinkIcon className="w-6" />
  <p className="hidden md:block">{link.name}</p>
</Link>
```

# Chapter 6: Setting Up Your Database

In this chapter, you'll be setting up a PostgreSQL database from one of Vercel's marketplace integrations. You'll need to:

1. Create a GitHub repository
2. Create a Vercel account
3. Connect and deploy your project
