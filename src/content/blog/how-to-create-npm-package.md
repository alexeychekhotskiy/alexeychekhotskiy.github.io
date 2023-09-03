---
title: "How to create a NPM package and publish it"
description: "How to Create a npm Package and Publish It on npmjs.com"
pubDate: "Sep 03 2023"
heroImage: "/blog-placeholder-npm.jpg"
---

In this post, I want to break down how to create and set up a simple npm package.

1. Tech Stack
2. Package Functionality
3. Testing a Package Locally
4. Setting up package.json, tsconfig.json, and vite.config.ts
5. Publishing

## Tech Stack

Our npm package will have built-in TypeScript declarations.

I’m going to use Vite as a module bundler, TypeScript, and React to build a simple npm package that allows lazy loading images. Let’s start with:

```
yarn create vite lazyload --template react-ts
```

to initialize a vite project. The project structure will be simple:

## Package Functionality

The package contains only one folder called `components` with the main component named `LazyImage.tsx` with the main logic:

```
import { useState } from "react";
import cls from "./LazyImage.module.css";

interface LazyImageProps {
    placeholder: string;
    img: string;
    alt?: string;
    effect?: string;
}

export const LazyImage: React.FC<LazyImageProps> = (props) => {
    const { placeholder, img, alt } = props;

    const [isLoaded, setIsLoaded] = useState(false);

    return (
        <div
            className={`${cls.placeholder} ${isLoaded ? cls.finished : ""}`}
            style={{ backgroundImage: `url(${placeholder})` }}
        >
            <img
                onLoad={() => setIsLoaded(true)}
                src={img}
                alt={alt}
                loading="lazy"
            />
        </div>
    );
};
```

And `LazyImage.module.css`:

```
.placeholder {
    background-repeat: no-repeat;
    background-size: cover;
}

.placeholder::before {
    content: '';
    position: absolute;
    inset: 0;
    opacity: 0;
    animation: pulsing 1.5s infinite;
    background-color: #fff;
}

@keyframes pulsing {
    0% {
        opacity: 0;
    }
    50% {
        opacity: 0.5;
    }
    100% {
        opacity: 0;
    }
}

.placeholder img {
    display: block;
    width: 100%;
    opacity: 0;
    transition: opacity 250ms ease-in-out;
}

.placeholder.finished::before {
    animation: none;
    content: none;
}

.placeholder.finished img {
    opacity: 1;
    transition: opacity 250ms ease-in-out;
}
```

The entry point of the package is index.ts which simply exports the "LazyImage" component:

```
import { LazyImage } from "./components/LazyImage";

export { LazyImage };
```

## Testing a Package Locally

To test a package locally without publishing every time you make changes, you can use "npm link". It is a command that allows you to locally symlink a package or project you're working on, so that you can use it as if it were installed globally or locally, depending on your needs.

### Create a global symbolic link:

Go to the package directory and run `npm link`.

### Use the linked package:

Go to another project where you want to test the package and run `npm link [package name]`. Package name is the name from the package's package.json file.

### Unlink:

To remove the symbolic link and revert to the regular package dependency, run `npm unlink package-name` inside the project where you're testing the package.

## Setting up package.json, tsconfig.json, and vite.config.ts

This part may be a bit tricky, so I'll provide source codes and explanation of the important parts. Let's start with `tsconfig.json`:

```
{
    "compilerOptions": {
        "declaration": true,
        "outDir": "./dist",
        "target": "ES2020",
        "useDefineForClassFields": true,
        "lib": ["ES2020", "DOM", "DOM.Iterable"],
        "module": "ESNext",
        "skipLibCheck": true,

        /* Bundler mode */
        "moduleResolution": "node",
        "resolveJsonModule": true,
        "isolatedModules": true,
        "jsx": "react-jsx",

        /* Linting */
        "strict": true,
        "noUnusedLocals": true,
        "noUnusedParameters": true,
        "noFallthroughCasesInSwitch": true
    },
    "include": ["src"],
    "references": [{ "path": "./tsconfig.node.json" }]
}
```

In the `tsconfig.json`, `declaration` is used to specify whether TypeScript should generate corresponding `.d.ts` declaration files when compiling your TypeScript code. `outDir` specifies the folder where the TypeScript compiler should output the compiled files.

In your `package.json`:

```
{
    "name": "laziest-react-image",
    "private": false,
    "version": "1.0.2",
    "type": "module",
    "scripts": {
        "dev": "vite",
        "build": "tsc && vite build",
        "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
        "preview": "vite preview"
    },
    "main": "./dist/react-lazy-image.umd.js",
    "types": "./dist/index.d.ts",
    "module": "./dist/react-lazy-image.es.js",
    "repository": {
        "type": "git",
        "url": "https://github.com/alexeychekhotskiy/laziest_react_image.git"
    },
    "dependencies": {
        "react": "^18.2.0",
        "react-dom": "^18.2.0",
        "vite-plugin-css-injected-by-js": "^3.3.0"
    },
    "devDependencies": {
        "@types/node": "^20.5.7",
        "@types/react": "^18.2.15",
        "@types/react-dom": "^18.2.7",
        "@typescript-eslint/eslint-plugin": "^6.0.0",
        "@typescript-eslint/parser": "^6.0.0",
        "@vitejs/plugin-react": "^4.0.3",
        "eslint": "^8.45.0",
        "eslint-plugin-react-hooks": "^4.6.0",
        "eslint-plugin-react-refresh": "^0.4.3",
        "typescript": "^5.0.2",
        "vite": "^4.4.5",
        "vite-plugin-dts": "^3.5.3"
    },
    "description": "This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.",
    "keywords": [
        "react",
        "lazy",
        "image",
        "loading"
    ],
    "author": "Alexey Chekhotskiy",
    "license": "ISC"
}
```

`vite.config.ts`

```
import path from "path";
import { defineConfig } from "vite";
import cssInjectedByJsPlugin from "vite-plugin-css-injected-by-js";
import react from "@vitejs/plugin-react";
import dts from "vite-plugin-dts";

export default defineConfig({
    build: {
        lib: {
            entry: path.resolve("src", "index.ts"),
            name: "react-lazy-image",
            fileName: (format) => `react-lazy-image.${format}.js`,
        },
        rollupOptions: {
            external: ["react", "react-dom"],
            output: {
                globals: {
                    react: "React",
                },
            },
        },
    },
    plugins: [react(), cssInjectedByJsPlugin(), dts()],
});
```

All plugins are needed: `react` for React integration, `cssInjectedByJsPlugin` to add all CSS to JS build. Since we are planning to have TS declarations, we need `dts` to create `d.ts` files during the package building

`entry`: Specifies the entry point for your library, which is the main TypeScript file that should be used as the starting point for bundling.

## Publishing

To publish your package, you need an account on npmjs webiste. After creating an account, run `yarn publish` and follow the instructions.
