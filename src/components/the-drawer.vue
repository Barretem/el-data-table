<template>
  <el-drawer
    ref="dialog"
    class="the-dialog-container"
    :title="title"
    :visible.sync="visible"
    v-bind="dialogAttrs"
    @close="resetFields"
  >
    <!--https://github.com/FEMessage/el-form-renderer-->
    <el-form-renderer
      ref="form"
      class="form-renderer-container"
      :content="form"
      v-bind="formAttrs"
      :disabled="isView"
    >
      <!--@slot 额外的弹窗表单内容, 当form不满足需求时可以使用，参考：https://femessage.github.io/el-form-renderer/#/Demo?id=slot -->
      <slot :row="slotData" />
    </el-form-renderer>

    <div v-show="!isView" class="operating-button-container">
      <el-button :size="buttonSize" @click="visible = false">取 消</el-button>
      <el-button
        type="primary"
        :loading="confirmLoading"
        :size="buttonSize"
        @click="confirm"
        >确 定</el-button
      >
    </div>
  </el-drawer>
</template>

<script>
export const dialogModes = {
  new: 'new',
  edit: 'edit',
  view: 'view'
}

export default {
  props: {
    newTitle: {
      type: String,
      required: true
    },
    editTitle: {
      type: String,
      required: true
    },
    viewTitle: {
      type: String,
      required: true
    },
    form: {
      type: Array,
      required: true
    },
    formAttrs: {
      type: Object,
      required: true
    },
    dialogAttrs: {
      type: Object,
      required: true
    },
    buttonSize: String
  },
  data() {
    return {
      mode: dialogModes.new,
      visible: false,
      confirmLoading: false,
      slotData: null
    }
  },
  computed: {
    title() {
      switch (this.mode) {
        case dialogModes.edit:
          return this.editTitle
        case dialogModes.view:
          return this.viewTitle
        default:
          return this.newTitle
      }
    },
    isView() {
      return this.mode === dialogModes.view
    }
  },
  methods: {
    /**
     * 显示dialog
     * @public
     */
    show(mode, formValue) {
      this.mode = mode
      this.visible = true
      if (formValue) {
        // $nextTick 有时也拿不到 form ，这样是稳妥的做法
        this.$refs.dialog.$once('opened', () => {
          this.$refs.form.updateForm(formValue)
          this.slotData = formValue
        })
      }
    },
    async confirm() {
      const valid = await new Promise(this.$refs.form.validate)
      if (!valid) return

      const formValue = this.$refs.form.getFormValue()
      const isNew = this.mode === dialogModes.new
      this.confirmLoading = true
      const done = (close = true) => {
        this.confirmLoading = false
        if (close) this.visible = false
      }
      this.$emit('confirm', isNew, formValue, done)
    },
    resetFields() {
      this.$refs.form.resetFields()
      this.slotData = null
    }
  }
}
</script>

<style lang="less">
.the-dialog-container {
  .el-drawer__close-btn,
  .el-drawer__header > :first-child {
    outline: 0;
  }

  .form-renderer-container {
    padding: 0 20px;
  }

  .operating-button-container {
    position: absolute;
    height: 55px;
    left: 0;
    right: 0;
    bottom: 0;
    text-align: center;
    background: rgba(244, 246, 250, 1);
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
