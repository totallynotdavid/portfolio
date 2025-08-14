# David's portfolio [![deploy](https://github.com/totallynotdavid/totallynotdavid.github.io/actions/workflows/deploy.yml/badge.svg)](https://github.com/totallynotdavid/totallynotdavid.github.io/actions/workflows/deploy.yml)

This repo contains the source code for my personal website. The site is built with
[Astro](https://astro.build/), [Tailwind CSS](https://tailwindcss.com/) and MDX. You can
view the live site at [totallynotdavid.github.io](https://totallynotdavid.github.io).

The site is a place for my writing and a playground for building things I find
interesting.

## Getting Started

To run the project locally, you'll need Bun. If you don't have Bun, you can install it
from [bun.sh](https://bun.sh/) or use npm/pnpm/yarn as alternatives.

First, clone the repository to your local machine:

```bash
git clone https://github.com/totallynotdavid/totallynotdavid.github.io.git
cd totallynotdavid.github.io
```

Install dependencies and run the development server:

```bash
bun install
bun dev
```

The site will be available at `http://localhost:4321`.

## Custom MDX Components

This site uses custom interactive components within MDX content. Here's how to use each
one:

### Annotation

It lets you to add contextual side notes to your text, similar to margin notes in academic
papers or books. On desktop, comments appear alongside the text with an animated SVG
bracket connecting them. On mobile devices, the layout adapts to display comments beneath
the annotated text.

To use the Annotation component, import it in your MDX file and wrap the text you want to
annotate. The main content goes in the default slot, while the annotation goes in the set
slot.

```mdx
import Annotation from '@/components/mdx/Annotation.astro';

<Annotation>
  New students are left out. They can't access years of accumulated knowledge that could
  help them prepare.
  <Fragment
    slot='comment'
    set:text='Relationships between upperclassmen and underclassmen can be problematic'
  />
</Annotation>
```

You can set:

- The `commentMaxWidth` to control the maximum width of the comment box in pixels
  (defaults to 260).
- The `mobileBreakpoint` prop determines the screen width threshold for switching to
  mobile layout (defaults to 768px).

If you prefer to disable the drawing animation, set `disableAnimation` to true.

### MediaEmbed

It lets you embed images, videos, and iframes. The component implements lazy loading,
displays skeleton placeholders during loading, and includes error handling with retry
functionality. It automatically detects media types based on file extensions, so you
typically don't need to specify the type manually.

For images, especially local assets, you should import the image file directly. The setup
makes use of the Astro <Image/> and <Picture/> components for optimal loading and
responsiveness when needed.

```mdx
---
import heroImage from '@/assets/pics.jpeg';
---

import MediaEmbed from '@/components/mdx/MediaEmbed.astro';

<MediaEmbed
  src={heroImage}
  alt='A descriptive alt text for the image'
  caption='An optional caption that appears below the image.'
  aspectRatio='square'
/>
```

For video content:

```mdx
<MediaEmbed
  src='/videos/my-cool-video.mp4'
  caption='A video of something cool.'
  controls
  loop
/>
```

The component supports various configuration options:

- The `aspectRatio` prop accepts values like '16/9', '4/3', or 'square' to prevent layout
  shift during loading.
- Set `priority` to true for above-the-fold content that should load immediately.

Standard HTML video attributes like `controls`, `loop`, `muted`, and `autoplay` are also
supported.

### Callout

Adds a visually distinct link block that draws attention to important external resources.
It's particularly useful for highlighting references, related content, or calls to action
within your writing.

```mdx
import Callout from '@/components/mdx/Callout.astro';

<Callout href='https://caefisica.com'>
  <p>
    Check us out. <span class='pl-1 text-gray-500'>Visit caefisica.com</span>
  </p>
</Callout>
```

The component requires only an `href` prop specifying the destination URL. The content can
include any HTML or Markdown elements you need.

### Mark

It applies a subtle highlighter effect to text using CSS gradients for a more natural
appearance than solid background colors.

```mdx
import Mark from '@/components/mdx/Mark.astro';

You need to <Mark>self-study effectively</Mark> to succeed in this career.
```

You can customize the highlight color by passing a `color` prop with any valid CSS color
value, such as 'rgba(245, 40, 145, 0.8)'. The component uses the `<mark>` HTML element by
default, but you can change this by setting the `as` prop to a different element like
`<span>` if needed.
