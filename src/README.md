``` vue
<template>
  <div>
    <wh-side-menu :menu-data="menuData" :is-root="true"/>
  </div>
</template>

<script>
import WhSideMenu from '../src/WhSideMenu.vue'

export default {
  name: 'App',
  components: { WhSideMenu },
  data() {
    return {
      menuData: [
        {
          name: 'test',
          children: [
            {
              name: 'child'
            }
          ]
        }
      ]
    }
  }
}
</script>

<style scoped>

</style>
```
