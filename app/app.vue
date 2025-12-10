<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtPage></NuxtPage>
  </div>
</template>


<style>
:root {
  --color-bg-primary: #1a1b26;
  --color-bg-secondary: #24283b;
  --color-bg-selection: #414868;
  --color-text-primary: #c0caf5;
  --color-text-unmuted: #565f89;
  --color-text-focus: #7aa2f7;
  --color-success: #9ece6a;
  --color-failure: #f7768e;
  --color-warning: #e0af68;
}

body {
  font-family: Inter, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, sans-serif;
  background: var(--color-bg-primary);
  color: var(--color-text-primary);
}
</style>