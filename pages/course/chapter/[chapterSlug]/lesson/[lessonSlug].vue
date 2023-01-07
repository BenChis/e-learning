<template>
  <div>
    <p class="mt-0 uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0">{{ lesson.title }}</h2>
    <div class="flex space-x-4 mt-2 mb-8">
      <nuxt-link
          v-if="lesson.sourceUrl"
          class="font-normal text-md text-gray-500"
          :href="lesson.sourceUrl"
      >
        Download Source Code
      </nuxt-link>
      <nuxt-link
          v-if="lesson.downloadUrl"
          class="font-normal text-md text-gray-500"
          :href="lesson.downloadUrl"
      >
        Download Video
      </nuxt-link>
    </div>
    <VideoPlayer v-if="lesson.videoId"></VideoPlayer>
    <p>{{ lesson.text }}</p>
    <LessonsCompleteButton :model-value="isLessonComplete" @update:model-value="toggleComplete"/>
  </div>
</template>

<script setup>
const course = useCourse();
const route = useRoute();

const chapter = computed(() => {
  return course.chapters.find(chapter => chapter.slug === route.params.chapterSlug)
})

const lesson = computed(() => {
  return chapter.value.lesson.find(lesson => lesson.slug === route.params.lessonSlug)
})

const title = computed(() => {
  return chapter.value.lessons.find(lesson => lesson.slug === route.params.lessonSlug)
})

useHead({
  title: title
})

const progress = ref('progress', () => {
  return [];
});

const isLessonComplete = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }
  if (!progress.value[chapter.value.number - 1][
  lesson.value.number - 1
      ]) {
    return false;
  }
})



</script>