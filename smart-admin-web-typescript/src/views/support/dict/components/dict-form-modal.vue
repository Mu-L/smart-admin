<!--
  * 字典表单 弹窗
  *
  * @Author:    1024创新实验室-主任：卓大
  * @Date:      2025-03-08 21:50:41
  * @Wechat:    zhuda1024
  * @Email:     lab1024@163.com
  * @Copyright  1024创新实验室 （ https://1024lab.net ），Since 2012
-->
<template>
  <a-modal :open="visible" :title="form.dictId ? '编辑字典' : '添加字典'" ok-text="确认" cancel-text="取消" @ok="onSubmit" @cancel="onClose">
    <br />
    <a-form ref="formRef" :model="form" :rules="rules" :label-col="{ span: 5 }" :wrapper-col="{ span: 19 }">
      <a-form-item label="字典编码" name="dictCode">
        <a-input v-model:value="form.dictCode" placeholder="请输入编码" />
      </a-form-item>
      <a-form-item label="字典名称" name="dictName">
        <a-input v-model:value="form.dictName" placeholder="请输入名称" />
      </a-form-item>

      <a-form-item label="备注" name="remark">
        <a-textarea v-model:value="form.remark" style="width: 100%; height: 100px; outline: none" />
      </a-form-item>
    </a-form>
  </a-modal>
</template>
<script setup lang="ts">
  import { ref, reactive } from 'vue';
  import { message } from 'ant-design-vue';
  import { SmartLoading } from '/@/components/framework/smart-loading';
  import { dictApi } from '/@/api/support/dict-api';
  import { smartSentry } from '/@/lib/smart-sentry';

  // emit
  const emit = defineEmits(['reloadList']);

  //  组件
  const formRef = ref();

  const formDefault = {
    dictId: undefined,
    dictCode: '',
    dictName: '',
    remark: '',
  };
  let form = reactive({ ...formDefault });
  const rules = {
    dictCode: [{ required: true, message: '请输入编码' }],
    dictName: [{ required: true, message: '请输入名称' }],
  };
  // 是否展示
  const visible = ref(false);

  function showModal(rowData) {
    Object.assign(form, formDefault);
    if (rowData) {
      Object.assign(form, rowData);
    }
    visible.value = true;
  }

  function onClose() {
    Object.assign(form, formDefault);
    visible.value = false;
  }

  function onSubmit() {
    formRef.value
      .validate()
      .then(async () => {
        SmartLoading.show();
        try {
          if (form.dictId) {
            await dictApi.updateDict(form);
          } else {
            await dictApi.addDict(form);
          }
          message.success(`${form.dictName ? '修改' : '添加'}成功`);
          emit('reloadList');
          onClose();
        } catch (error) {
          smartSentry.captureError(error);
        } finally {
          SmartLoading.hide();
        }
      })
      .catch((error) => {
        message.error('参数验证错误，请仔细填写表单数据!');
      });
  }

  // ----------------------- 以下是暴露的方法内容 ------------------------
  defineExpose({
    showModal,
  });
</script>
