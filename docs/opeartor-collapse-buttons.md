设置搜索头部内容inline，并行排列

```vue
<template>
  <el-data-table v-bind="$data" />
</template>
<script>
export default {
  data() {
    return {
      hasSearchButton: false,
      headerInline: true,
      buttonContainerWidth: 100,
      url: 'https://mockapi.eolinker.com/IeZWjzy87c204a1f7030b2a17b00f3776ce0a07a5030a1b/el-data-table?q=search-immediately',
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
      hasView: true,
      extraButtons: [
        {
          type: 'success',
          disabled: row => row.date === '2016-05-04',
          text: row => row.status === 'normal' ? '禁用' : '启用',
          collapse: true,
          atClick(row) {
            alert(row.address)
            return new Promise((resolve, reject) => {
              setTimeout(() => {
                resolve(true)
              }, 1000);
            })
          }
        },
      ]
    }
  }
}
</script>
```
