<template>
  <div class="topbar">
    <div class="left-logo" >Logo Here</div>
    <div class="middle-menu">
      <el-menu
        :default-active="$route.path"
        background-color="#304156"
        text-color="#eee"
        active-text-color="#ffd04b"
        mode="horizontal">
        <template v-for="route in routes" v-if="!route.hidden">
          <MenuItem v-if="!route.realPath" :key="route.path" :item="route" :base-path="route.path" >
          <el-menu-item v-if="route.realPath" :key="route.path" :index="route.path">
            <ItemLink :to="route.realPath" :key="route.name">
              <svg-icon :icon-class="route.meta.icon"/> {{ route.meta.title }}
            </ItemLink>
          </el-menu-item>
        </template>
      </el-menu>
    </div>
    <div class="right-logout">
      <div class="logout-left">
        <img src="@/assets/rabbit.gif" class="user-avatar" @click="logout">
      </div>
      <div class="logout-right">
        <el-button type="info" plain round size="mini" @click="logout" >Logout</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import MenuItem from './MenuItem'
import ItemLink from '../Sidebar/Link'
import path from 'path'
import { isExternal } from '@/utils'

export default {
  components: { MenuItem, ItemLink },
  data() {
    return {
      activeIndex: null
    }
  },
  computed: {
    routes() {
      return this.$router.options.routes
    }
  },
  methods: {
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        location.reload() // 为了重新实例化vue-router对象 避免bug
      })
    },
    resolvePath(routePath) {
      if (this.isExternalLink(routePath)) {
        return routePath
      }
      return path.resolve(this.basePath, routePath)
    },
    isExternalLink(routePath) {
      return isExternal(routePath)
    }
  }
}
</script>
