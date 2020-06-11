自定义搜索函数

```vue
<template>
  <el-data-table v-bind="$data" />
</template>
<script>
export default {
  data() {
    const url = 'https://mockapi.eolinker.com/IeZWjzy87c204a1f7030b2a17b00f3776ce0a07a5030a1b/el-data-table?q=search-immediately';
    return {
      hasSearchButton: false,
      headerInline: true,
      buttonContainerWidth: 100,
      customQueryDataFn: (query) => {
        // 返回的数据格式为axios的请求格式
        return this.$axios.get(url, query)
      },
      columns: [
        {prop: 'date', label: '日期'},
        {prop: 'q', label: '姓名'},
        {prop: 'address', label: '地址'},
      ],
      searchForm: [
        {
          el: {placeholder: '请选择'},
          label: '姓名',
          id: 'q',
          type: 'select',
          searchImmediately: true,
          options: [
            {label: '王小虎', value: 'tiger'},
            {label: '李小猫', value: 'cat'},
          ]
        },
      ],
      hasView: true
    }
  }
}
</script>
```
