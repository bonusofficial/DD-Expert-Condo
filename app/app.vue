<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
    <PopupContract v-if="!$route.path.startsWith('/admin')" />
  </div>
</template>
