<template>
  <div class="page-header-index-wide">
    <a-card :bordered="false" :bodyStyle="{ padding: '16px 0', height: '100%' }" :style="{ height: '100%' }">
      <div class="account-settings-info-main" :class="{ 'mobile': isMobile }">
        <div class="account-settings-info-left">
          <a-menu
            :mode="isMobile ? 'horizontal' : 'inline'"
            :style="{ border: '0', width: isMobile ? '560px' : 'auto'}"
            :selectedKeys="selectedKeys"
            type="inner"
            @openChange="onOpenChange"
          >
            <a-menu-item key="/account/settings/base">
              <router-link :to="{ name: 'settings.base' }">
                个人信息
              </router-link>
            </a-menu-item>
            <a-menu-item key="/account/settings/change_password">
              <router-link :to="{ name: 'settings.change_password' }">
                修改密码
              </router-link>
            </a-menu-item>
            <a-menu-item key="/account/settings/webauthn" v-if="webauthnAvailable">
              <router-link :to="{ name: 'settings.webauthn' }">
                使用安全秘钥登录
              </router-link>
            </a-menu-item>
            <a-menu-item key="/account/settings/system">
              <router-link :to="{ name: 'settings.system' }">
                系统设置
              </router-link>
            </a-menu-item>
          </a-menu>
        </div>
        <div class="account-settings-info-right">
          <div class="account-settings-info-title">
            <span>{{ $route.meta.title }}</span>
          </div>
          <route-view></route-view>
        </div>
      </div>
    </a-card>
  </div>
</template>

<script>
import { RouteView } from '@/layouts'
import { baseMixin } from '@/store/app-mixin'

export default {
  components: {
    RouteView
  },
  mixins: [baseMixin],
  data () {
    const webauthnAvailable = document.location.protocol === 'https:' || document.location.host === 'localhost'
    return {
      // horizontal  inline
      webauthnAvailable,
      mode: 'inline',
      openKeys: [],
      selectedKeys: []
    }
  },
  mounted () {
    this.updateMenu()
  },
  methods: {
    onOpenChange (openKeys) {
      this.openKeys = openKeys
    },
    updateMenu () {
      const routes = this.$route.matched.concat()
      this.selectedKeys = [ routes.pop().path ]
    }
  },
  watch: {
    '$route' (val) {
      this.updateMenu()
    }
  }
}
</script>

<style lang="less" scoped>
  .account-settings-info-main {
    width: 100%;
    display: flex;
    height: 100%;
    overflow: auto;

    &.mobile {
      display: block;

      .account-settings-info-left {
        border-right: unset;
        border-bottom: 1px solid #e8e8e8;
        width: 100%;
        height: 50px;
        overflow-x: auto;
        overflow-y: scroll;
      }
      .account-settings-info-right {
        padding: 20px 40px;
      }
    }

    .account-settings-info-left {
      border-right: 1px solid #e8e8e8;
      width: 224px;
    }

    .account-settings-info-right {
      flex: 1 1;
      padding: 8px 40px;

      .account-settings-info-title {
        color: rgba(0,0,0,.85);
        font-size: 20px;
        font-weight: 500;
        line-height: 28px;
        margin-bottom: 12px;
      }
      .account-settings-info-view {
        padding-top: 12px;
      }
    }
  }

</style>
