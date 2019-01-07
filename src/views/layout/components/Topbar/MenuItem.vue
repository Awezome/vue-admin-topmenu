<template >
  <el-submenu :index="resolvePath(item.path)" :show-timeout="20" :hide-timeout="20">
    <template slot="title"><svg-icon :icon-class="item.meta.icon"/> {{ item.meta.title }}</template>
    <template v-for="child in item.children" v-if="!child.hidden">

      <ItemLink :to="resolvePath(child.path)" :key="child.name">
        <el-menu-item :index="resolvePath(child.path)"> {{ child.meta.title }}</el-menu-item>
      </ItemLink>

    </template>
  </el-submenu>
</template>

<script>
import path from 'path'
import { isExternal } from '@/utils'
import ItemLink from '../Sidebar/Link'

export default {
  components: { ItemLink },
  props: {
    item: {
      type: Object,
      required: true
    },
    basePath: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      menus: null
    }
  },
  computed: {
  },
  created() {
  },
  methods: {
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
