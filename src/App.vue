<template>
  <div class="min-h-screen bg-gray-50 dark:bg-gray-900">
    <!-- Navigation -->
    <Nav />

    <!-- Main Content -->
    <main class="pt-16">
      <div class="mx-auto max-w-7xl px-4 py-8 lg:px-8 sm:px-6">
        <router-view />
      </div>
    </main>

    <!-- Footer -->
    <Footer />
  </div>
</template>
