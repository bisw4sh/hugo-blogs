---
title: "MDX Tooling"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["mdx", "markdown", "blogs"]
categories:
  - Markdown
  - Blogs
author: ["biswash"]
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Using markdown to render html is quite an interesting idea, how to do it with github flavored markdown server both statically with react & dynamically with node backend server"
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "/" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/bisw4sh/hugo-blogs/blob/main/content/mdx-tooling.md"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

# Understanding the use of mdx with toolset

## 1. We need to configure `@mdx-js/rollup` , this could be different for different toolset(i'm using vite; which uses **rollup for production build**).

```ts
//vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import mdx from "@mdx-js/rollup";

export default defineConfig(async () => {
  return {
    plugins: [react(), mdx()],
  };
});
```

## 2. We need the provider of MDX to wrap the app to apply the imported `*.mdx`

```ts
//App.ts
import { MDXProvider } from "@mdx-js/react";

function App() {
  return (
    <>
      <MDXProvider>
        <TestMarkdown />
      </MDXProvider>
    </>
  );
}

export default App;
```

**_Note: This doesnot support code higlighting and table; we need rehype-highlight, remark-gfm respectively for that_**

## 3. Add `remark-gfm`, (Github flavored markdown) .

```ts
//vite.config.ts
import remarkGfm from "remark-gfm";

export default defineConfig(async () => {
  return {
    plugins: [
      react(),
      mdx({
        remarkPlugins: [remarkGfm],
      }),
    ],
  };
});
```

## 4. Now to add code block syntax highlighting, it could be done in 2 ways; runtime and compile time.

- Compile time : we need `rehype` [guide](https://mdxjs.com/guides/syntax-highlighting/).
- Run time : `react-syntax-highlighter`

- For `react-syntax-highlighter`

```ts
interface CodeProps {
  className?: string;
  children?: React.ReactNode;
}

// Custom syntax highlighting component
function CodeBlock({ className, children, ...props }: CodeProps) {
  const match = /language-(\w+)/.exec(className || "");
  return match ? (
    <SyntaxHighlighter
      language={match[1]}
      PreTag="div"
      {...props}
      style={atelierCaveDark}
      showLineNumbers
    >
      {String(children).trim()}
    </SyntaxHighlighter>
  ) : (
    <code className={className} {...props}>
      {children}
    </code>
  );
}
```

```ts
<BlogComponent components={{ code: CodeBlock }} />
```

> Above guide is to have syntax highlighting(runtime), gfm, mdx in react.

### MDX from node.js (remote location)

- We need to evaluate the markdown at runtime, could be done with [showdown](https://github.com/showdownjs/showdown) in nodejs backend. We could also use [mdx-bundler](https://github.com/kentcdodds/mdx-bundler) by Kent C. Dodds. We can now use same files for frontend & backend.

- Backend

```ts
const filePath = path.join(
  __dirname,
  "public",
  "mdx-files",
  `${req.params.slug}.mdx`
);
const mdxContent = readFileSync(filePath, "utf-8");

const { code, frontmatter } = await bundleMDX({
  source: mdxContent,
  mdxOptions(options) {
    options.remarkPlugins = [...(options.remarkPlugins ?? []), remarkGfm];
    options.development = false;
    return options;
  },
});

res.json({ code, frontmatter });
```

- Frontend

```ts
const MDXContent = useMemo(() => {
  if (fetchedPost?.code) {
    try {
      // This evaluates the code and returns a React component
      return getMDXComponent(fetchedPost.code);
    } catch (error) {
      console.error("Error creating MDX component:", error);
      return null;
    }
  }
  return null;
}, [fetchedPost]);
```

```ts
<MDXContent components={{ code: CodeBlock }} />
```
