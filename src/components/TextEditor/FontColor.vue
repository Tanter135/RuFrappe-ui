<template>
  <Popover transition="default">
    <template #target="{ togglePopover, isOpen }">
      <slot
        v-bind="{ onClick: () => togglePopover(), isActive: isOpen }"
      ></slot>
    </template>
    <template #body-main>
      <div class="p-2">
        <div class="text-sm text-ink-gray-7">Цвет текста</div>
        <div class="mt-1 grid grid-cols-8 gap-1">
          <Tooltip
            class="flex"
            v-for="color in foregroundColors"
            :key="color.name"
            :text="color.label"
          >
            <button
              :aria-label="color.label"
              class="flex h-5 w-5 items-center justify-center rounded border text-base"
              :style="{
                color: color.hex,
              }"
              @click="setForegroundColor(color)"
            >
              A
            </button>
          </Tooltip>
        </div>
        <div class="mt-2 text-sm text-ink-gray-7">Цвет фона</div>
        <div class="mt-1 grid grid-cols-8 gap-1">
          <Tooltip
            class="flex"
            v-for="color in backgroundColors"
            :key="color.name"
            :text="color.label"
          >
            <button
              :aria-label="color.label"
              class="flex h-5 w-5 items-center justify-center rounded border text-base text-ink-gray-9"
              :class="
                !color.hex ? 'border-outline-gray-modals' : 'border-transparent'
              "
              :style="{
                backgroundColor: color.hex,
              }"
              @click="setBackgroundColor(color)"
            >
              A
            </button>
          </Tooltip>
        </div>
      </div>
    </template>
  </Popover>
</template>

<script>
import Popover from '../Popover.vue'
import { Tooltip } from '../../index'

export default {
  name: 'FontColor',
  props: ['editor'],
  components: { Popover, Tooltip },
  methods: {
    setBackgroundColor(color) {
      if (color.name !== 'Default') {
        this.editor.chain().focus().toggleHighlight({ color: color.hex }).run()
      } else {
        this.editor.chain().focus().unsetHighlight().run()
      }
    },
    setForegroundColor(color) {
      if (color.name !== 'Default') {
        this.editor.chain().focus().setColor(color.hex).run()
      } else {
        this.editor.chain().focus().unsetColor().run()
      }
    },
  },
  computed: {
    foregroundColors() {
      return [
        { name: 'Default', label: 'По умолчанию', hex: '#1F272E' },
        { name: 'Yellow', label: 'Жёлтый', hex: '#ca8a04' },
        { name: 'Orange', label: 'Оранжевый', hex: '#ea580c' },
        { name: 'Red', label: 'Красный', hex: '#dc2626' },
        { name: 'Green', label: 'Зелёный', hex: '#16a34a' },
        { name: 'Blue', label: 'Синий', hex: '#1579D0' },
        { name: 'Purple', label: 'Фиолетовый', hex: '#9333ea' },
        { name: 'Pink', label: 'Розовый', hex: '#db2777' },
      ]
    },
    backgroundColors() {
      return [
        { name: 'Default', label: 'По умолчанию', hex: null },
        { name: 'Yellow', label: 'Жёлтый', hex: '#fef9c3' },
        { name: 'Orange', label: 'Оранжевый', hex: '#ffedd5' },
        { name: 'Red', label: 'Красный', hex: '#fee2e2' },
        { name: 'Green', label: 'Зелёный', hex: '#dcfce7' },
        { name: 'Blue', label: 'Синий', hex: '#D3E9FC' },
        { name: 'Purple', label: 'Фиолетовый', hex: '#f3e8ff' },
        { name: 'Pink', label: 'Розовый', hex: '#fce7f3' },
      ]
    },
  },
}
</script>
