<script setup lang="ts">
import Prism from "prismjs"
import PrismBlock from "./helpers/prism.vue"
import { useNotionBlock, defineNotionProps } from "@/lib/blockable"
import { computed } from "vue"

const props = defineProps({ overrideLang: String, overrideLangClass: String, ...defineNotionProps })
//@ts-ignore
const { properties } = useNotionBlock(props)

const lang = computed(() => {
  return props.overrideLang || properties.value?.language?.[0]?.[0]?.toLowerCase()
})

const langClass = computed(() => {
  return props.overrideLangClass || `language-${lang.value}`
})

const supported = computed(() => {
  return lang.value ? Prism?.languages[lang.value] : false
})
</script>

<script lang="ts">
export default {
  name: "NotionCode",
}
</script>

<template>
  <div v-if="supported" :class="['notion-code']">
    <PrismBlock :language="lang">{{ properties?.title[0][0] }}</PrismBlock>
  </div>
  <div v-else :class="['notion-code']">
    <pre><code :class="langClass">{{ properties?.title[0][0] }}</code></pre>
  </div>
</template>
