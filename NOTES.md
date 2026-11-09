# Lab 1 – Notes

## 1. In which file are the `<html>` and `<body>` tags?
In `app/layout.tsx`, the root layout.

## 2. Which file renders the page at `/`?
`app/page.tsx`.

## 3. Which folder is for images and other static files, and how do you reference them?
The `public/` folder. Files there are served from the root URL, so `public/next.svg` is available at `/next.svg` (for example `<img src="/next.svg" />`).

## 4. What is `tsconfig.json` doing in your project?
It's the TypeScript configuration: compiler options (strict mode, JSX, module resolution), which files are included, and the `@/*` path alias.

## 5. What does `@/*` mean in `tsconfig.json`? Write an import that uses it.
It is an alias for the project root, so you don't need long relative paths like `../../`.
Example: `import { getMovies } from "@/lib/movies";`

## 6. What does `app/layout.tsx` import from `next/font/google`, and what does Next.js do with it at build time?
It imports the `Geist` and `Geist_Mono` fonts. At build time Next.js downloads the font files and serves them from our own app, so the browser makes no request to Google and the layout doesn't shift while fonts load.

## 7. What's the difference between `layout.tsx` and `page.tsx`?
`layout.tsx` wraps pages and is shared: it stays mounted when you navigate between its pages. `page.tsx` is the unique content of one route and is what makes the route accessible.

## 8. Where would you put a navbar that appears on every page?
In `app/layout.tsx`, inside `<body>`, above `{children}`.

## 9. What is `globals.css` for, and where is it imported?
It holds the global styles for the whole app and imports Tailwind CSS. It is imported in `app/layout.tsx`.

## 10. What is `AGENTS.md`, and does your app need it to run?
It's a file of instructions for AI coding agents, so they write up-to-date Next.js code. The app does not need it to run.

# Lab 2 – Notes

## 1. In the page source, are the movie titles present for /movies? For /movies-client? Why?
For /movies, yes: it is a Server Component, the server fetches the data and sends complete HTML. For /movies-client, no: the source only contains "Loading movies...". The data is fetched by useEffect, which only runs in the browser after the JavaScript has loaded.

## 2. Without force-dynamic, you run npm run build again tomorrow, then npm start. What does "Rendered at" show, and why?
It shows tomorrow's build time. The page is static: it is rendered once at build time, so the timestamp is the moment of the last build and never changes until the next build.

## 3. Give one situation where force-dynamic is still the right choice, and why.
When the page must always show fresh or per-request data, for example live stock prices, a live score or a page depending on the request (cookies, search params). A static page would show outdated data until the next build.

## 4. A classmate sees "[movies] rendered 6 movies" in the browser console and says the code runs in the browser. What do you answer, and how can you prove it?
It runs on the server. In development Next.js copies server logs into the browser console (marked "Server") as a debugging aid. Proof: the log appears in the terminal running Next.js on every request, the titles are in the HTML source, the page still shows the movies with JavaScript disabled, and in production (npm start) the log only appears in the terminal.

## 5. Which version would you choose for a public movie catalogue that changes once a week, and why?
The static server-rendered version (/movies without force-dynamic, or with ISR such as revalidate). The HTML is ready instantly for every visitor, titles are in the source so it is good for SEO, and since data changes only weekly we only need to rebuild or revalidate occasionally.