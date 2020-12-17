<template>
  <div class="home">
    
    <AutocompleteField2 
      @choice="handleChoice"
      :searchInput="search"
    />
  </div>
</template>

<script>
// import AutocompleteField from '@/components/AutocompleteField'
import AutocompleteField2 from '@/components/AutocompleteField2'
export default {
  name: 'Home',
  components: { AutocompleteField2 },
  data: () => ({
    search: '123'
  }),
  mounted() {
    setTimeout(() => {
      this.search = 'Эту строку передал родительский компонент'
    }, 500)
  },
  methods: {
    handleChoice(el) {
      this.search = el
    }
  }
}
</script>

<style>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

@font-face {
  font-family: 'Material Icons';
  font-style: normal;
  font-weight: 400;
  src: url(https://example.com/MaterialIcons-Regular.eot); /* For IE6-8 */
  src: local('Material Icons'),
    local('MaterialIcons-Regular'),
    url(https://example.com/MaterialIcons-Regular.woff2) format('woff2'),
    url(https://example.com/MaterialIcons-Regular.woff) format('woff'),
    url(https://example.com/MaterialIcons-Regular.ttf) format('truetype');
}
</style>