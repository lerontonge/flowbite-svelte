<script lang="ts">
  import { Listgroup, ListgroupItem, Avatar, Tooltip } from "$lib";
  import { BellOutline, ClockOutline, TrashBinOutline, TrashBinSolid } from "flowbite-svelte-icons";
  let simpleList = ["Profile", "Settings", "Messages", "Download"];

  let links = [
    { name: "Accordions", href: "/docs/components/accordion", current: true },
    { name: "Alerts", href: "/docs/components/alert" },
    { name: "Badges", href: "/docs/components/badge" },
    { name: "Breadcrumbs", href: "/docs/components/breadcrumb", attrs: { target: "_blank" } }
  ];

  let buttons = [
    { name: "Profile", mycustomfield: "data1", current: true },
    { name: "Settings", mycustomfield: "data2" },
    { name: "Messages", mycustomfield: "data3" },
    { name: "Download", mycustomfield: "data4", disabled: true, attrs: { type: "submit" } }
  ];

  import { AdjustmentsHorizontalSolid, DownloadSolid, MessagesSolid, UserCircleSolid } from "flowbite-svelte-icons";
  let icons = [
    { name: "Profile", Icon: UserCircleSolid },
    { name: "Settings", Icon: AdjustmentsHorizontalSolid },
    { name: "Messages", Icon: MessagesSolid },
    { name: "Download", Icon: DownloadSolid }
  ];
</script>

<h1 class="my-4 text-3xl">List Group</h1>
<h2 class="text-2xl">Default list group</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup items={simpleList} class="w-48" />
</div>

<h2 class="text-2xl">List group with links</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup active items={links} class="w-48" />
</div>

<h2 class="text-2xl">List group with buttons</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup active items={buttons} class="w-48" onclick={(e) => alert(Object.entries(e?.detail ?? {}))} />
</div>

<h2 class="text-2xl">List group with icons</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup active items={icons} class="w-48" onclick={console.log} />
</div>

<h2 class="text-2xl">Horizontal list group</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup active items={buttons} horizontal onclick={(e) => alert(Object.entries(e?.detail ?? {}))}></Listgroup>
</div>

<h2 class="text-2xl">Horizontal list with tooltip</h2>

<div class="my-4 flex h-64 items-center justify-center space-x-8 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup horizontal active>
    <ListgroupItem>
      <BellOutline />
    </ListgroupItem>
    <Tooltip>Tooltip bell</Tooltip>
    <ListgroupItem>
      <ClockOutline />
    </ListgroupItem>
    <Tooltip>Tooltip clock</Tooltip>
    <ListgroupItem id="trash">
      <TrashBinOutline />
    </ListgroupItem>
  </Listgroup>
  <Tooltip triggeredBy="#trash">Tooltip trash</Tooltip>

  <Listgroup horizontal active>
    <ListgroupItem id="profile">Profile</ListgroupItem>
    <ListgroupItem id="settings">Settings</ListgroupItem>
    <ListgroupItem id="message">Messages</ListgroupItem>
  </Listgroup>
  <Tooltip triggeredBy="#profile">Tooltip profile</Tooltip>
  <Tooltip triggeredBy="#settings">Tooltip settings</Tooltip>
  <Tooltip triggeredBy="#message">Tooltip messages</Tooltip>
</div>

<h2 class="text-2xl">Advanced</h2>

<div class="my-4 rounded border border-gray-200 p-4 dark:border-gray-600">
  <Listgroup active class="w-48">
    <h3 class="p-1 text-center text-xl font-medium text-gray-900 dark:text-white">User list</h3>
    <ListgroupItem class="gap-2 text-base font-semibold">
      <Avatar src="/images/profile-picture-1.webp" size="xs" />Jese Leos
    </ListgroupItem>
    <ListgroupItem class="gap-2 text-base font-semibold">
      <Avatar src="/images/profile-picture-2.webp" size="xs" />Robert Gouth
    </ListgroupItem>
    <ListgroupItem class="gap-2 text-base font-semibold">
      <Avatar src="/images/profile-picture-3.webp" size="xs" />Bonnie Green
    </ListgroupItem>
    <a href="/" class="flex items-center rounded-b-lg bg-gray-50 p-3 text-sm font-medium text-red-600 hover:bg-gray-100 hover:underline dark:bg-gray-700 dark:text-red-500 dark:hover:bg-gray-600">
      <TrashBinSolid class="ms-1 me-2 h-6 w-6" />
      Delete user
    </a>
  </Listgroup>
</div>
