This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

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
