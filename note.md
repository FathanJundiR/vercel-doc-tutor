# Run

```bash
pnpm dev
```

# Optimizing Font

- Layour shift terjadi ketika awalnya web menggunakan font bawaan komputer lalu menggantinya dengan font custom kita.
- Layout shift akan mempengaruhi the text size, spacing, or layout
- Nextjs mengatasi ini dengan mendownload font dan asset custom lainnya ke dalam web kita saat build time, jadi bukan ngedownload saat web diakses

## Adding Primary Font

- Buat sebuah file di `app/ui`, misal `font.ts`

```typescript
import { Inter } from "next/font/google";

export const inter = Inter({ subsets: ["latin"] });
```

import ke `/app/layout.tsx`

```ts
import "@/app/ui/global.css";
import { inter } from "@/app/ui/fonts";

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

# Optimizing Image

Masukin image manual lewat HTML kita harus mengatur hal berikut:

- Ensure your image is responsive on different screen sizes.
- Specify image sizes for different devices.
- Prevent layout shift as the images load.
- Lazy load images that are outside the user's viewport.

## \<Image\> Component

- Ini merupakan component dari Next.js
- Component ini ada image optimization seperti:
  - Preventing layout shift automatically when images are loading.
  - Resizing images to avoid shipping large images to devices with a smaller viewport.
  - Lazy loading images by default (images load as they enter the viewport).
  - Serving images in modern formats, like WebP
    and AVIF, when the browser supports it.

contoh:

```jsx
<Image
  src="/hero-desktop.png"
  width={1000}
  height={780}
  className="hidden md:block"
  alt="Screenshots of the dashboard project showing desktop version"
/>
```

# Routing

- Next menggunakan file-syste routing, routing menggunakan folder.
  ![routing](image.png)
- Di setiap routing/folder, kita bisa mengatur ui lewat `layout.tsx` dan `page.tsx`
- `page.tsx` meng-export react component dan file ini **WAJIb** ada agar route bisa diakses.
  ![page](image-1.png)

By having a special name for page files, Next.js allows you to colocate UI components, test files, and other related code with your routes. Only the content inside the page file will be publicly accessible. For example, the /ui and /lib folders are colocated inside the /app folder along with your routes.

- `layout.tx` kek skeleton UI dari halaman web.
  One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering:
  ![alt text](image-2.png)

# Navigate

- Untuk navigasi, gunakan `<Link>`agar ga refresh pas di-click

## Show Active Links

A common UI pattern is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called usePathname() that you can use to check the path and implement this pattern.

Since usePathname()
is a hook, you'll need to turn nav-links.tsx into a Client Component. Add React's "use client" directive to the top of the file, then import usePathname() from next/navigation:

**/app/ui/dashboard/nav-links.tsx**

```tsx
"use client";

import {
  UserGroupIcon,
  HomeIcon,
  InboxIcon,
} from "@heroicons/react/24/outline";
import Link from "next/link";
import { usePathname } from "next/navigation";
// ...
```
