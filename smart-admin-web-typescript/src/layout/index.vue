<!--
  *  layout 多种模式
  *
  * @Author:    1024创新实验室-主任：卓大
  * @Date:      2022-09-06 20:40:16
  * @Wechat:    zhuda1024
  * @Email:     lab1024@163.com
  * @Copyright  1024创新实验室 （ https://1024lab.net ），Since 2012
-->
<template>
  <!--左侧菜单 模式-->
  <SideLayout v-if="layout === LAYOUT_ENUM.SIDE.value" />
  <!--左侧展开菜单 模式-->
  <SideExpandLayout v-if="layout === LAYOUT_ENUM.SIDE_EXPAND.value" />
  <!--顶部菜单 模式-->
  <TopLayout v-if="layout === LAYOUT_ENUM.TOP.value" />
  <!--定期修改密码-->
  <RegularChangePasswordModal />
  <!--顶部展开 模式-->
  <TopExpandLayout v-if="layout === LAYOUT_ENUM.TOP_EXPAND.value" />
</template>
<script setup lang="ts">
  import { computed } from 'vue';
  import { LAYOUT_ENUM } from '/@/constants/layout-const';
  import SideExpandLayout from './side-expand-layout.vue';
  import TopExpandLayout from './top-expand-layout.vue';
  import SideLayout from './side-layout.vue';
  import TopLayout from './top-layout.vue';
  import { useAppConfigStore } from '/@/store/modules/system/app-config';
  import RegularChangePasswordModal from './components/change-password/regular-change-password-modal.vue';

  const layout = computed(() => useAppConfigStore().$state.layout);
</script>
