<template>
  <client-only>
    <main>
      <LazyLayoutNavbar />
      <Nuxt />
      <LazyLayoutFooter />
    </main>
  </client-only>
</template>

<script>
export default {
  name: "defaultPage"
}
</script>

<style lang="scss" scoped></style>
