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
          name: '用户信息',
          routeTo: { path: '/' },
          children: [
            {
              name: '用户信息11',
              routeTo: { path: '/' }
            }
          ]
        },
        {
          name: '身份认证',
          routeTo: { path: '/' }
        },
        {
          name: '绑定新手机',
          routeTo: { path: '/' }
        },
        {
          name: '修改密码',
          routeTo: { path: '/' }
        },
        {
          name: '发票中心',
          routeTo: { path: '/' }
        },
        {
          name: '地址管理',
          routeTo: { path: '/' }
        }
      ]
    }
  }
}
</script>

<style scoped>

</style>
