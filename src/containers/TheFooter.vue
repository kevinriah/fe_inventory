<template>
  <CFooter :fixed="false">
    <div>
      <a href="https://coreui.io" target="_blank">B</a>
      <span class="ml-1">&copy; {{new Date().getFullYear()}} TOKODITA</span>
    </div>
    <div class="mfs-auto">
      <span class="mr-1" target="_blank">Powered by</span>
      <a href="https://coreui.io/vue">TD</a>
    </div>
  </CFooter>
</template>

<script>
export default {
  name: 'TheFooter'
}
</script>
