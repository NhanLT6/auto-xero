﻿<script setup lang="ts">
import { ref } from 'vue';

import type { LocaleItem } from '@/interfaces/LocaleItem';

import { useLocale } from 'vuetify';

import { useStorage } from '@vueuse/core';

const allowedLocales = ref<LocaleItem[]>([
  { country: 'English', localeCode: 'en' },
  { country: 'Vietnamese', localeCode: 'vi' },
]);

const { current } = useLocale();
console.log('current', current.value);

const userLocale = useStorage('userLocale', allowedLocales.value[0]);

const changeLocale = (locale: any) => {
  console.log('changeLocale', locale);
  current.value = locale;

  const newLocal = allowedLocales.value.find((l) => l.localeCode === locale);
  if (newLocal === undefined) return;

  userLocale.value = newLocal;

  console.log('currentLocale', current.value);
};
</script>

<template>
  <form class="d-flex flex-column ga-2 v-col-4">
    <VSelect
      :items="allowedLocales"
      label="Locale"
      :model-value="userLocale"
      item-title="country"
      item-value="localeCode"
      persistent-hint
      hint="This option change how Date input component display selected date. Required because Vuetify doesn't expose
          config this for now"
      @update:model-value="changeLocale"
    >
    </VSelect>

    <!-- First day of week -->
    <VSelect
      :items="['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']"
      label="First day of week"
      persistent-hint
      hint="This option change which day is first day of week in Calendar component"
    >
    </VSelect>

    <VBtn class="flex-fill" variant="tonal" color="green-darken-3" prepend-icon="mdi-content-save-outline"> Save </VBtn>
  </form>
</template>

<style scoped></style>
