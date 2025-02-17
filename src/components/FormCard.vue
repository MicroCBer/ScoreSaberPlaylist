<script setup>
import { reactive, defineEmits, defineProps } from 'vue'
import { SearchOutlined } from '@ant-design/icons-vue'
import dayjs from 'dayjs'
import locale from 'ant-design-vue/es/date-picker/locale/zh_CN'

defineProps({
  loading: Boolean
})

const form = reactive({
  fullComboType: 'All',
  rankedType: 'Ranked',
  date: {
    enable: false,
    value: []
  },
  acc: {
    enable: false,
    value: [0, 100]
  },
  rank: {
    enable: false,
    greater: false,
    value: [0, 300]
  },

  pp: {
    enable: false,
    value: [0, 600]
  },
  stars: {
    enable: false,
    value: [0, 14]
  },
  sort: {
    type: 'PP',
    order: 'DESC'
  }

})

const emits = defineEmits(['submit'])

function onSubmit () {
  emits('submit', form)
}

const accMarks = reactive({
  65: 'A-',
  80: 'S',
  90: 'SS'
})

const rankMarks = reactive({
  10: '10',
  50: '50',
  100: '100',
  200: '200',
  300: '300+'
})

const ppMarks = reactive({
  100: '100',
  200: '200',
  300: '300',
  400: '400',
  500: '500',
  600: '600+'
})

const headStyle = {
  'font-weight': 'bolder',
  'min-height': '30px'
}

function disabledDate (current) {
  return current && current > dayjs().endOf('day')
}

</script>

<template>
  <a-spin :spinning="loading" size="large">
    <a-card :bodyStyle="{ padding: '2px 24px' }">
      <a-card hoverable class="form-card-item">
        <a-space>
          <a-radio-group v-model:value="form.rankedType" button-style="solid" size="small">
            <a-radio-button value="All">全部</a-radio-button>
            <a-radio-button value="Ranked">排位曲</a-radio-button>
            <a-radio-button value="UnRanked">非排位曲</a-radio-button>
          </a-radio-group>

          <a-radio-group v-model:value="form.fullComboType" button-style="solid" size="small">
            <a-radio-button value="All">全部</a-radio-button>
            <a-radio-button value="FullCombo">已全连</a-radio-button>
            <a-radio-button value="UnFullCombo">未全连</a-radio-button>
          </a-radio-group>
        </a-space>
      </a-card>

      <a-card title="日期" hoverable :headStyle="headStyle" class="form-card-item">
        <template #extra>
          <a-switch v-model:checked="form.date.enable" />
        </template>
        <a-range-picker
          v-model:value="form.date.value"
          :disabled-date="disabledDate"
          :disabled="!form.date.enable"
          :locale="locale"
        />
      </a-card>

      <a-card title="ACC" hoverable :headStyle="headStyle" class="form-card-item">
        <template #extra>
          <a-switch v-model:checked="form.acc.enable" />
        </template>
        <a-slider
          v-model:value="form.acc.value"
          range
          :min="65"
          :disabled="!form.acc.enable"
          :tip-formatter="(value) => `${value}%`"
          :marks="accMarks"
        />
      </a-card>

      <a-card title="排名" hoverable :headStyle="headStyle" class="form-card-item">
        <template #extra>
          <a-switch v-model:checked="form.rank.enable" />
        </template>
        <a-slider
          v-model:value="form.rank.value"
          range
          :min="0"
          :max="300"
          :step="10"
          :disabled="!form.rank.enable"
          :tip-formatter="(value) => value === 300 ? '300+' : value"
          :marks="rankMarks"
        />
      </a-card>

      <div v-show="form.rankedType != 'UnRanked'">
        <a-card title="PP" hoverable :headStyle="headStyle" class="form-card-item">
          <template #extra>
            <a-switch v-model:checked="form.pp.enable" />
          </template>
          <a-slider
            v-model:value="form.pp.value"
            range
            :min="0"
            :max="600"
            :step="10"
            :disabled="!form.pp.enable"
            :tip-formatter="(value) => value === 600 ? '600+' : value"
            :marks="ppMarks"
          />
        </a-card>

        <a-card title="星级" hoverable :headStyle="headStyle" class="form-card-item">
          <template #extra>
            <a-switch v-model:checked="form.stars.enable" />
          </template>
          <a-slider
            v-model:value="form.stars.value"
            range
            :min="0"
            :max="14"
            :disabled="!form.stars.enable"
          />
        </a-card>
      </div>

      <a-button type="primary" shape="round" size="large" @click="onSubmit" :loading="loading">
        <template #icon>
          <search-outlined />
        </template>
        查询
      </a-button>
    </a-card>
  </a-spin>
</template>

<style scoped>
.form-card-item {
  margin: 12px 0;
}
</style>
