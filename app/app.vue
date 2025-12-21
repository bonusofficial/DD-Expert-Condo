<template>
  <div class="app-wrapper">
    <Navbar />
    <Banner />
    
    <!-- Home Page Sections -->
    <PropertyCategories />
    <ServicesSection />
    <FeaturedProperties />
    <VideoShowcase />
    <AdsPartners />
    <Announcements />
    <PartnersSection />

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
