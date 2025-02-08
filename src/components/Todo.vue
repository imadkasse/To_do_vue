<template>
  <div class="flex items-center bg-gray-100 p-3 rounded my-2">
    <!-- ربط خاصية checked بالقيمة من props -->
    <input
      type="checkbox"
      :checked="completed"
      @click="handleCompleted"
      class="mr-2"
    />
    <span :class="{ 'line-through': completed }" class="flex-grow">{{
      taskName
    }}</span>
    <button
      @click="handleDelete"
      class="font-semibold text-red-400 hover:text-red-500 transition duration-300"
    >
      حذف
    </button>
  </div>
</template>

<script setup>
// تعريف الخصائص (props) المطلوبة من المكون الأب
const props = defineProps({
  id: Number,
  taskName: String,
  completed: Boolean,
  deleteTask: Function,
  completedMark: Function,
});

// دالة الحذف
const handleDelete = () => {
  props.deleteTask(props.id); // تمرير اسم المهمة عند الحذف
};
const handleCompleted = () => {
  props.completedMark(props.id);
};
</script>

<style scoped></style>
