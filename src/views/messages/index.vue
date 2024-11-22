<script setup lang="ts">
import { ref } from 'vue';

// 定义文件内容的响应式引用，并指定其类型为string | null
const fileContent = ref<string | null>(null);

// 定义handleFileChange函数，该函数接收一个类型为Event的参数
function handleFileChange(event: Event): void {
  const fileInput = event.target as HTMLInputElement;

  // 检查files属性是否为null
  if (fileInput.files && fileInput.files.length > 0) {
    const file = fileInput.files[0];

    const reader = new FileReader();

    // eslint-disable-next-line func-names
    reader.onload = function (e: ProgressEvent<FileReader>) {
      fileContent.value = e.target?.result as string;
    };

    // eslint-disable-next-line func-names
    reader.onerror = function (e: ProgressEvent<FileReader>) {
      console.error('文件读取失败:', e);
    };

    reader.readAsText(file, 'UTF-8');
  } else {
    console.warn('没有选择文件或文件列表为空');
  }
}
</script>

<template>
  <div>messages</div>
  <div>
    <input type="file" @change="handleFileChange" />
    <div v-if="fileContent">
      <h3>文件内容：</h3>
      <pre>{{ fileContent }}</pre>
    </div>
  </div>
</template>

<style scoped></style>
