---
layout: componentLayout
title: Svelte Text - Flowbite
breadcrumb_title: Svelte Text
component_title: Text
dir: Typography
description: Learn how to customize text-related styles and properties such as font size, font style, text decoration, font weight and more
thumnailSize: w-64
---

<script>
  import { CompoAttributesViewer, GitHubCompoLinks, toKebabCase } from '../../utils'
  import { A, Heading } from '$lib'

  const components = 'P, Span'

  // lib files
  // const libFilesArray = [
  //   import.meta.glob("$lib/typography/P.svelte"),
  //   import.meta.glob("$lib/typography/Span.svelte"),
  // ];
  // const libFiles = { ...libFilesArray[0], ...libFilesArray[1] };
</script>

Get started with a collection of text customization examples to learn how to update the size, font weight, style, decoration and spacing of inline text elements.

## Font size

Use this example to set the font size of inline text elements using the `size` prop.

```svelte example class="flex flex-wrap items-center space-x-4 rtl:space-x-reverse"
<script>
  import { P } from "flowbite-svelte";
</script>

<P size="xs">Aa</P>
<P size="sm">Aa</P>
<P size="base">Aa</P>
<P size="lg">Aa</P>
<P size="xl">Aa</P>
<P size="2xl">Aa</P>
<P size="3xl">Aa</P>
<P size="4xl">Aa</P>
<P size="5xl">Aa</P>
<P size="6xl">Aa</P>
<P size="7xl">Aa</P>
<P size="8xl">Aa</P>
<P size="9xl">Aa</P>
```

## Font weight

This example can be used to the font weight of an inline text element using the `weight` prop.

```svelte example class="flex flex-wrap items-center space-x-4 rtl:space-x-reverse" hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P size="4xl" weight="thin">Aa</P>
<P size="4xl" weight="extralight">Aa</P>
<P size="4xl" weight="light">Aa</P>
<P size="4xl" weight="normal">Aa</P>
<P size="4xl" weight="medium">Aa</P>
<P size="4xl" weight="semibold">Aa</P>
<P size="4xl" weight="bold">Aa</P>
<P size="4xl" weight="extrabold">Aa</P>
<P size="4xl" weight="black">Aa</P>
```

## Text color

Use the `class` prop to set the color of the inline text.

```svelte example class="flex flex-wrap items-center space-x-4 rtl:space-x-reverse" hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P class="text-blue-700 dark:text-blue-500">This text is in the blue color.</P>
<P class="text-green-700 dark:text-green-500">This text is in the green color.</P>
<P class="text-red-700 dark:text-red-500">This text is in the red color.</P>
<P class="text-purple-700 dark:text-purple-500">This text is in the purple color.</P>
<P class="text-teal-700 dark:text-teal-500">This text is in the teal color.</P>
```

## Letter spacing

Increase or decrease the spacing between letters using the `space` prop.

```svelte example class="flex flex-wrap items-center rtl:space-x-reverse" hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P space="tighter">Flowbite app will help you improve yourself by analysing your everyday life.</P>
<P space="tight">Flowbite app will help you improve yourself by analysing your everyday life.</P>
<P space="normal">Flowbite app will help you improve yourself by analysing your everyday life.</P>
<P space="wide">Flowbite app will help you improve yourself by analysing your everyday life.</P>
<P space="wider">Flowbite app will help you improve yourself by analysing your everyday life.</P>
<P space="widest">Flowbite app will help you improve yourself by analysing your everyday life.</P>
```

## Text decoration

### Underline

Update the text decoration style using the `underline` and `class` props.

```svelte example
<script>
  import { P, Span } from "flowbite-svelte";
</script>

<P>
  Track work across the enterprise through an open, collaborative platform. <Span underline>Link issues across Jira</Span> and ingest data from other <Span underline class="decoration-blue-500 decoration-double">software development</Span> tools, so your IT support and operations teams have richer contextual information to rapidly respond to <Span underline class="decoration-green-500 decoration-dotted">requests</Span>, <Span underline class="decoration-red-500 decoration-dashed decoration-4">incidents</Span>, and <Span underline class="decoration-sky-500 decoration-wavy">changes</Span>.
</P>
```

### Line through

Set a strikethrough line on a text element using the `line-through` class.

```svelte example
<script>
  import { Span } from "flowbite-svelte";
</script>

<Span class="line-through">$109</Span><Span class="ms-3">$79</Span>
```

### Uppercase

Force uppercase characters for a given portion of text using the uppercase class.

```svelte example
<script>
  import { P, Span } from "flowbite-svelte";
</script>

<P>The crypto <Span class="uppercase">identity</Span> primitive.</P>
```

## Font style

Set italic or non italic styles with the props.

### Italic

Use the `italic` prop to set italic font style to a text element.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P italic>The crypto identity primitive.</P>
```

### Normal

Text elements by default are non-italic.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P>The crypto identity primitive.</P>
```

## Line Height

Set the height between lines using the `height` prop.

### Leading normal

Use the `height="normal"` (default) prop to set default line height.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P size="3xl" height="normal" class="max-w-lg" weight="semibold">The Al-powered app will help you improve yourself by analysing your everyday life.</P>
```

### Leading relaxed

Use the `height="relaxed"` prop to increase the space between lines.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P size="3xl" height="relaxed" class="max-w-lg" weight="semibold">The Al-powered app will help you improve yourself by analysing your everyday life.</P>
```

### Leading loose

Use the `height="loose"` prop to set a large amount of space between text lines.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P size="3xl" height="loose" class="max-w-lg" weight="semibold">The Al-powered app will help you improve yourself by analysing your everyday life.</P>
```

## Text Align

### Left

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P align="left">Get started with an enterprise-level, profesionally designed, fully responsive, and HTML semantic set of web pages, sections and over 400+ components crafted with the utility classes from Tailwind CSS and based on the Flowbite component library</P>
```

### Center

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P align="center">Get started with an enterprise-level, profesionally designed, fully responsive, and HTML semantic set of web pages, sections and over 400+ components crafted with the utility classes from Tailwind CSS and based on the Flowbite component library</P>
```

### Right

Use the `align="right"` prop to align the text element to the right side of the page.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P align="right">Get started with an enterprise-level, profesionally designed, fully responsive, and HTML semantic set of web pages, sections and over 400+ components crafted with the utility classes from Tailwind CSS and based on the Flowbite component library</P>
```

### Text justify

Use the `justify` prop to justify the text content.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P justify>Get started with an enterprise-level, profesionally designed, fully responsive, and HTML semantic set of web pages, sections and over 400+ components crafted with the utility classes from Tailwind CSS and based on the Flowbite component library</P>
<P>Get started with an enterprise-level, profesionally designed, fully responsive, and HTML semantic set of web pages, sections and over 400+ components crafted with the utility classes from Tailwind CSS and based on the Flowbite component library</P>
```

## Whitespace

Configure the whitespace behaviour of inline text elements using the `whitespace` prop.

### Normal

Use the `whitespace="normal"` prop to set the default whitespace behaviour.

```svelte example hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P whitespace="normal">This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text.</P>
```

### Nowrap

Use the `whitespace="nowrap"` prop to prevent text being added to a new line when the full width has been reached.

```svelte example class="overflow-y-scroll" hideScript
<script>
  import { P } from "flowbite-svelte";
</script>

<P whitespace="nowrap">This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text. This is some text.</P>
```

### Pre line

Use the `whitespace="preline'` prop to add whitespace exactly how it has been set from the source code.

<!-- prettier-ignore -->
```svelte example hideScript
<script>
  import { P } from 'flowbite-svelte'
</script>

<P whitespace="preline">
  This is some text. This is some text. This is some text. 
  This is some text. This is some text. This is some text. 
  This is some text. This is some text. This is some text.
</P>
```

## Text Decoration Style

Update the text decoration style using the `underline` and `class` props.

```svelte example hideScript
<script>
  import { P, Span } from "flowbite-svelte";
</script>

<P>
  Track work across the enterprise through an open, collaborative platform. <Span underline>Link issues across Jira</Span> and ingest data from other <Span underline class="decoration-blue-500 decoration-double">software development</Span> tools, so your IT support and operations teams have richer contextual information to rapidly respond to <Span underline class="decoration-green-500 decoration-dotted">requests</Span>, <Span underline class="decoration-red-500 decoration-dashed decoration-4">incidents</Span>, and <Span underline class="decoration-sky-500 decoration-wavy">changes</Span>.
</P>
```

## Component data

The component has the following props, type, and default values. See [types page](/docs/pages/typescript) for type information.

<CompoAttributesViewer {components}/>

## References

<GitHubCompoLinks {components}/>
