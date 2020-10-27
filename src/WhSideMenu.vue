<script>
import Vue from 'vue'
import { Menu, MenuItem, Submenu } from 'element-ui'

Vue.use(Menu)
Vue.use(MenuItem)
Vue.use(Submenu)

const opends = []
const data = Vue.observable({
  menuRoot: '',
  defaultActive: ''
})

export default {
  name: 'WhSideMenu',
  functional: true,
  props: {
    /**
     * 路由对象
     **/
    router: {
      type: [Object],
      default() {
        return null
      }
    },
    /**
     * 菜单数据
     */
    menuData: {
      type: [Object, Array],
      required: true
    },
    /**
     * 是否为菜单根部，调用时必须设置为true
     */
    isRoot: {
      type: Boolean,
      default() {
        return false
      }
    },
    svgComponent: {
      type: [String, Object],
      default() {
        return 'svg-icon'
      }
    }
  },
  render(createElement, ctx) {
    const { props } = ctx
    const isMenuItemGroup = props.menuData && props.menuData.children !== undefined
    const isMenuItem = props.menuData && props.menuData.children === undefined
    if (props.isRoot) {
      const menuRoot = createElement('el-menu', {
        class: 'side-menu-root',
        props: {
          // 'default-openeds': opends,
          'default-active': data.defaultActive
        }
      },
      props.menuData.map((m) => createElement('wh-side-menu', {
        props: {
          key: m.name,
          menuData: m
        }
      })))
      return menuRoot
    } if (isMenuItemGroup) {
      if (!opends.includes(props.menuData.name)) {
        opends.push(props.menuData.name)
      }

      let itemTitle = createElement('span', props.menuData.name)
      if (props.menuData.badge) {
        itemTitle = createElement('el-badge', {
          ...props.menuData.badge
        }, props.menuData.name)
      }

      const submenuItemContent = []
      if (props.menuData.menuItemRender) {
        submenuItemContent.push(props.menuData.menuItemRender(createElement, props.menuData))
      } else {
        let icon = null
        if (props.menuData.iconRender) {
          icon = props.menuData.iconRender(createElement, props.menuData)
        }
        submenuItemContent.push(icon, itemTitle)
      }

      return createElement(
        'el-submenu',
        {
          props: {
            index: props.menuData.name
          },
          nativeOn: {
            click(...args) {
              if (props.menuData.action !== undefined &&
                  props.menuData.action.constructor === Function) {
                props.menuData.action(...args)
              }
            }
          }
        },
        [
          createElement('span', {
            slot: 'title'
          }, submenuItemContent),
          props.menuData.children.map((item) => createElement('wh-side-menu', {
            props: {
              key: item.name,
              menuData: item
            }
          }))
        ]
      )
    } if (isMenuItem) {
      const r = props.router
      if (r && r.currentRoute.path === r.resolve(props.menuData.routeTo).route.path) {
        data.defaultActive = props.menuData.name
      }

      let itemTitle = createElement('span', {
        slot: 'title'
      }, props.menuData.name)

      if (props.menuData.badge) {
        itemTitle = createElement('el-badge', {
          slot: 'title',
          ...props.menuData.badge
        }, props.menuData.name)
      }

      const itemContent = []
      if (props.menuData.menuItemRender) {
        itemContent.push(props.menuData.menuItemRender(createElement, props.menuData))
      } else {
        let icon = null
        if (props.menuData.iconRender) {
          icon = props.menuData.iconRender(createElement, props.menuData)
        }

        itemContent.push(icon, itemTitle)
      }
      const innerItem = createElement('el-menu-item', {
        props: {
          index: props.menuData.name
        },
        nativeOn: {
          click(...args) {
            if (props.menuData.action !== undefined &&
                props.menuData.action.constructor === Function) {
              props.menuData.action(...args)
            } else {
              ctx.router.push(props.menuData.routeTo)
            }
          }
        }
      }, itemContent)
      return innerItem
    }
    return null
  }
}
</script>

<style scoped lang="less">
.side-menu-root {
  color: #383d4a;
  overflow-x: hidden;
  background-color: #383d4a;
  ::v-deep {
    ul.el-menu.el-menu--inline {
      background-color: #2b2e3a;
    }
    .el-submenu.is-opened .el-submenu__title {
      background-color: #2b2e3a;
    }
    .side-menu-root{
      background-color: #383d4a;
    }
    .el-submenu.is-opened > ul{
      background-color: #2b2e3a;
    }
    .el-menu-item, .el-submenu__title {
      color: #fff;
    }
    .el-menu-item.is-active {
      background-color: #0084ff!important;
    }
    .el-menu-item{
      &:hover, &:focus{
        background-color: #0084ff;
      }
    }
    .el-submenu__title{
      &:hover, &:focus{
        background-color: transparent;
      }
    }
    .el-submenu .el-menu-item{
      padding-left: 60px!important;
    }
  }
}
</style>

