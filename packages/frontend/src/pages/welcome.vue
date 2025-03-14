<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
  <div v-if="instance">
    <!-- MkTinkerbell 컴포넌트에 색상과 스타일 바인딩 -->
    <MkTinkerbell :colour="'#6CA69C'" :style="{ zIndex: '9999' }" />
    
    <!-- 인스턴스 설정 여부에 따른 컴포넌트 렌더링 -->
    <XSetup v-if="instance.requireSetup" />
    <XEntrance v-else />
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';
import * as Misskey from 'misskey-js';
import XSetup from './welcome.setup.vue';
import XEntrance from './welcome.entrance.a.vue';
import { instanceName } from '@@/js/config.js';
import { definePageMetadata } from '@/scripts/page-metadata.js';
import { fetchInstance } from '@/instance.js';

const instance = ref<Misskey.entities.MetaDetailed | null>(null);

fetchInstance(true).then((res) => {
	instance.value = res;
});

const headerActions = computed(() => []);

const headerTabs = computed(() => []);

definePageMetadata(() => ({
	title: instanceName,
	icon: null,
}));
</script>
