<script setup lang="ts">
import { refDebounced } from '@vueuse/core'

const route = useRoute()
const router = useRouter()

const open = ref(false)

// 搜索输入（防抖）
const searchInput = ref('')
const debouncedSearch = refDebounced(searchInput, 300)

// 监听 URL 变化同步到输入框
watch(() => route.query.search, (value) => {
  searchInput.value = (value as string) || ''
}, { immediate: true })

// 监听防抖后的搜索词更新 URL
watch(debouncedSearch, (value) => {
  if (value) {
    router.push({
      query: { ...route.query, search: value },
    })
  }
  else {
    // 清空搜索时移除 search 参数
    const { search, ...restQuery } = route.query
    router.push({ query: restQuery })
  }
})

function clearSearch() {
  searchInput.value = ''
}
</script>

<template>
  <Popover v-model:open="open">
    <PopoverTrigger as-child>
      <TooltipIconButton
        :label="$t('header.search')"
        icon="i-lucide-search"
        variant="ghost"
        size="icon"
      />
    </PopoverTrigger>
    <PopoverContent class="op-80 w-96" align="start">
      <div class="space-y-2">
        <!-- 搜索框 -->
        <div class="flex gap-2 items-center">
          <Input
            v-model="searchInput"
            :placeholder="$t('search.placeholder')"
            class="flex-1"
          />
          <Button
            v-if="searchInput"
            variant="ghost"
            size="icon"
            @click="clearSearch"
          >
            <div class="i-lucide-x" />
          </Button>
        </div>

        <!-- 搜索提示 -->
        <p class="text-xs text-muted-foreground">
          {{ $t('search.hint') }}
        </p>
      </div>
    </PopoverContent>
  </Popover>
</template>
