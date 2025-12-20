<template>
  <div class="app-wrapper">
    <Navbar />
    <Banner />

    <main class="main-content">
      <NuxtPage />
    </main>

    <Footer />
    <PopupContract />
  </div>
</template>

<style>
.app-wrapper {
  min-height: 100vh;
  background: #fffef7;
}

.main-content {
  min-height: 50vh;
}
</style>
