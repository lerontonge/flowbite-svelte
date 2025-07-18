<script lang="ts">
  import Button from "$lib/buttons/Button.svelte";
  import Tooltip from "$lib/tooltip/Tooltip.svelte";
  import { DesktopPcOutline, MobilePhoneOutline, TabletOutline } from "flowbite-svelte-icons";
  import { mount, onMount } from "svelte";
  import { twJoin, twMerge } from "tailwind-merge";
  import ExampleDarkMode from "./ExampleDarkMode.svelte";
  import ExampleHelper from "./ExampleHelper.svelte";
  import ExampleRtl from "./ExampleRTL.svelte";
  import GitHub from "./icons/GitHub.svelte";

  // Define the props properly with TypeScript
  type Props = {
    src?: any;
    meta?: any;
    example?: any;
    code?: any;
    codeString?: string; // New prop for string code
    divClass?: string;
  };

  function normalizeIndentation(code: string): string {
    if (!code) return code;

    const lines = code.split("\n");

    while (lines.length && lines[0].trim() === "") {
      lines.shift();
    }
    while (lines.length && lines[lines.length - 1].trim() === "") {
      lines.pop();
    }

    if (!lines.length) return "";

    const nonEmptyLines = lines.filter((line) => line.trim().length > 0);
    const indentations = nonEmptyLines.map((line) => {
      const match = line.match(/^(\s*)/);
      return match ? match[1].length : 0;
    });

    const minIndent = Math.min(...indentations);

    const trimmedLines = lines.map((line) => (line.trim() === "" ? "" : line.length >= minIndent ? line.slice(minIndent) : line));

    return trimmedLines.join("\n").trim(); // ← Fix: Add .trim() here
  }

  // Action to normalize rendered code content
  function normalizeRenderedCode(node: HTMLPreElement) {
    // Wait for the content to be rendered
    setTimeout(() => {
      if (!codeString && node.textContent) {
        const normalized = normalizeIndentation(node.textContent);
        if (normalized !== node.textContent) {
          node.textContent = normalized;
        }
      }
    }, 0);
  }

  // Use typed props
  let {
    src = undefined,
    meta = { hideOutput: false, hideSource: false, hideResponsiveButtons: false },
    example,
    code,
    codeString = undefined, // New prop
    divClass = "relative w-full mx-auto bg-linear-to-r p-6 bg-white dark:bg-gray-900"
  }: Props = $props();

  type NotificationDirection = "ltr" | "rtl" | "auto";

  // State variables
  let browserSupport = $state(false);
  let codeEl: HTMLElement | undefined = $state(undefined);
  let codeResponsiveContent: HTMLDivElement | undefined = $state(undefined);
  let path: URL | undefined = $state(undefined);
  let showExpandButton = $state(false);
  let expand = $state(false);
  let dark = $state(false);
  let independentDarkMode = $state(false);
  let independentDark = $state(false);
  let rtl: NotificationDirection = $state("auto");
  let responsiveDevice: keyof typeof responsiveSize = $state("desktop");
  let iframe: HTMLIFrameElement | undefined = $state(undefined);
  let iframeLoad = $state(false);
  let copy_text = "Copy";
  let documentObserver: MutationObserver | undefined = $state(undefined);

  const responsiveSize = {
    mobile: "max-w-sm",
    tablet: "max-w-lg",
    desktop: ""
  };

  const gitHub = new URL("https://github.com/themesberg/flowbite-svelte/blob/main/src/routes/");

  function checkDarkMode(): void {
    if (document && document.documentElement) {
      const isDark = document.documentElement.classList.contains("dark");

      if (independentDarkMode) {
        independentDarkMode = false;
        independentDark = false;
      }

      dark = isDark;
      syncIframeDarkMode();
    }
  }

  function syncIframeDarkMode(): void {
    if (iframe && iframe.contentDocument) {
      if (dark) {
        iframe.contentDocument.documentElement.classList.add("dark");
      } else {
        iframe.contentDocument.documentElement.classList.remove("dark");
      }
    }
  }

  function toggleDarkMode(): void {
    independentDarkMode = true;
    independentDark = !independentDark;
    dark = independentDark;
    syncIframeDarkMode();
  }

  function init(node: HTMLElement) {
    browserSupport = !!window?.navigator?.clipboard;
    dark = document.documentElement.classList.contains("dark");

    documentObserver = new MutationObserver((mutations) => {
      mutations.forEach((mutation) => {
        if (mutation.attributeName === "class") {
          checkDarkMode();
        }
      });
    });

    documentObserver.observe(document.documentElement, {
      attributes: true,
      attributeFilter: ["class"]
    });

    setTimeout(() => find_sections(node), 0);

    return {
      destroy() {
        if (documentObserver) {
          documentObserver.disconnect();
        }
      }
    };
  }

  function find_sections(node: HTMLElement): void {
    const sections = [...node.ownerDocument.querySelectorAll("#mainContent > :where(h2, h3) > [id]")];
    const sectionData = sections.map((x: Element) => ({
      id: x.id,
      top: x.parentElement?.offsetTop ?? Infinity,
      obj: x
    }));

    const filteredSections = sectionData.filter((x) => x.top < (node?.offsetTop ?? Infinity)).filter((x) => x.id);
    const section = filteredSections.slice(-1).shift();

    if (section) {
      const pathname = new URL(node.baseURI).pathname;
      path = new URL(pathname.slice(1) + ".md", gitHub);
      path.hash = section.id.replaceAll("_", "-").replaceAll("/", "").toLowerCase();
    }
  }

  const copyToClipboard = async (e: MouseEvent): Promise<void> => {
    if (!codeEl) return;

    let textToCopy: string;

    // Use codeString if available, otherwise fall back to innerText
    if (codeString) {
      textToCopy = normalizeIndentation(codeString);
    } else {
      const REG_HEX = /&#x([a-fA-F0-9]+);/g;
      textToCopy = codeEl.innerText.replace(REG_HEX, function (_match: string, group1: string) {
        const num = parseInt(group1, 16);
        return String.fromCharCode(num);
      });
    }

    if (window?.navigator?.clipboard) {
      await window.navigator.clipboard.writeText(textToCopy);
    }

    const button = e.target as HTMLButtonElement | null;
    button?.blur();

    const lastChild = button?.lastChild as Text | null;
    if (lastChild) {
      lastChild.textContent = "Copied";
      setTimeout(() => (lastChild.textContent = "Copy"), 3000);
    }
  };

  function checkOverflow(el: HTMLElement): void {
    const isOverflowingY = el.clientHeight < el.scrollHeight;
    showExpandButton = isOverflowingY;
    el.firstElementChild?.classList.add("-mb-8");
  }

  const injectContent = (): void => {
    if (!iframe) return;
    if (!iframe.contentDocument) return;
    if (!iframe.contentWindow) return;

    iframeLoad = true;

    const externalCss = document.querySelectorAll('head link[href*="https://"][rel="stylesheet"], head style');
    const internalCss = Array.from(document.styleSheets).filter((el) => el.href?.includes(document.location.hostname));

    const extractInlineCss = internalCss.reduce((acc, el) => {
      acc += Array.from(el.cssRules)
        .map((rule) => rule.cssText)
        .join(" ");
      return acc;
    }, "");

    const styleTag = document.createElement("style");
    styleTag.innerHTML = extractInlineCss;

    const headContent = Array.from(externalCss).reduce((acc, el) => (acc += el.outerHTML), "");

    iframe.contentDocument.head.insertAdjacentHTML("beforeend", `${headContent}${styleTag.outerHTML}` || "");

    mount(ExampleHelper, {
      target: iframe.contentDocument.body,
      props: {
        snippet: example,
        class: twMerge(divClass, meta?.class ?? "")
      }
    });

    syncIframeDarkMode();
    updateHeightContent();

    if (iframe.contentDocument?.body.firstChild) {
      const resizeObserver = new ResizeObserver(updateHeightContent);
      resizeObserver.observe(iframe.contentDocument.body.firstElementChild as Element);
    }
  };

  const updateHeightContent = (): void => {
    if (!codeResponsiveContent) return;
    if (!iframe?.contentDocument?.body?.firstElementChild) return;

    const element = iframe.contentDocument.body.firstElementChild as HTMLElement;
    const height = element.offsetHeight || 0;
    codeResponsiveContent.style.height = `${height}px`;
  };

  onMount(() => {
    dark = document.documentElement.classList.contains("dark");

    setTimeout(() => {
      if (iframe && !iframeLoad) {
        iframe.dispatchEvent(new Event("load"));
      }
    }, 500);
  });

  $effect(() => {
    if (!iframe) return;
    if (!iframe.contentDocument) return;

    iframe.contentDocument.documentElement.dir = rtl || "";
  });
</script>

<div class="code-example my-8" use:init>
  {#if !meta?.hideOutput}
    <div class="w-full rounded-t-xl border border-gray-200 bg-gray-50 p-4 dark:border-gray-600 dark:bg-gray-700">
      <div class="grid {meta.hideResponsiveButtons ? 'grid-cols-2' : 'grid-cols-2 sm:grid-cols-3'}">
        {#if path}
          <Button size="xs" color="alternative" class="hover:text-primary-600 w-fit gap-2 dark:bg-gray-900" href={"" + path} target="_blank" rel="noreferrer">
            <GitHub size="sm" />Edit on GitHub
          </Button>
          {#if !meta?.hideResponsiveButtons}
            <div class="hidden justify-center gap-x-2 sm:flex">
              <Button size="xs" color="alternative" class="dark:bg-gray-900" onclick={() => (responsiveDevice = "desktop")}>
                <DesktopPcOutline size="sm" />
              </Button>
              <Button size="xs" color="alternative" class="dark:bg-gray-900" onclick={() => (responsiveDevice = "tablet")}>
                <TabletOutline size="sm" />
              </Button>
              <Button size="xs" color="alternative" class="dark:bg-gray-900" onclick={() => (responsiveDevice = "mobile")}>
                <MobilePhoneOutline size="sm" />
              </Button>
            </div>
          {/if}
          <div class="ms-auto flex">
            <ExampleDarkMode onclick={() => toggleDarkMode()} {dark} />
            <ExampleRtl bind:rtl />
          </div>
        {/if}
      </div>
    </div>

    <div class="code-preview-wrapper">
      <div class="code-preview flex border-x border-gray-200 bg-white bg-linear-to-r p-0 dark:border-gray-600 dark:bg-gray-900" class:dark dir={rtl}>
        <div class="code-responsive-wrapper w-full">
          <div class="code-responive-content {twJoin(!meta.hideResponsiveButtons && 'mx-auto', responsiveSize[responsiveDevice])}" bind:this={codeResponsiveContent}>
            {#if !meta.hideResponsiveButtons}
              <iframe bind:this={iframe} class="h-full w-full" title="iframe-code-content" onload={injectContent}></iframe>
            {:else}
              <div class={twMerge(divClass, meta.class)}>
                {@render example()}
              </div>
            {/if}
          </div>
        </div>
      </div>
    </div>
  {/if}
  {#if !meta?.hideSource}
    <div class="code-syntax-wrapper">
      <div class="code-syntax relative border-x border-y border-gray-200 dark:border-gray-600">
        <div class="grid w-full grid-cols-2 rounded-t-md border-b border-gray-200 bg-gray-50 dark:border-gray-600 dark:bg-gray-700">
          <ul class="flex text-center text-sm font-medium text-gray-500 dark:text-gray-400">
            <li>
              <span class="inline-block w-full border-e border-gray-200 bg-gray-100 p-2 px-3 text-gray-800 dark:border-gray-600 dark:bg-gray-800 dark:text-white">Svelte</span>
            </li>
          </ul>
          <div class="flex justify-end">
            {#if browserSupport}
              <button onclick={(e) => copyToClipboard(e)} type="button" class="hover:text-primary-700 copy-to-clipboard-button flex items-center border-s border-gray-200 bg-gray-100 px-3 py-2 text-xs font-medium text-gray-600 dark:border-gray-600 dark:bg-gray-800 dark:text-gray-400 dark:hover:text-white">
                <svg class="me-2 h-4 w-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z" />
                </svg>
                {copy_text}
              </button>
              <Tooltip placement="bottom-end">Copy to clipboard.</Tooltip>
            {/if}
          </div>
        </div>
        <div class="relative">
          <div class="overflow-hidden" class:max-h-72={!expand} tabindex="-1" use:checkOverflow>
            <div class="highlight">
              <pre bind:this={codeEl} class="language-svelte -mt-2! rounded-none!" use:normalizeRenderedCode>{#if codeString}<code>{normalizeIndentation(codeString)}</code>{:else}{@render code?.()}{/if}</pre>
            </div>
          </div>
          {#if showExpandButton && !expand}
            <button onclick={() => (expand = !expand)} data-expand-code="" type="button" class="hover:text-primary-700 absolute start-0 bottom-0 w-full border-t border-gray-200 bg-gray-100 px-5 py-2.5 text-sm font-medium text-gray-900 hover:bg-gray-100 dark:border-gray-600 dark:bg-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">Expand code</button>
          {/if}
        </div>
      </div>
    </div>
  {/if}
</div>
