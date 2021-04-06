<template lang='pug'>
.p-2.m-2.flex.flex-col.border-2.rounded-3xl.border-gray-400.w-200.shadow-lg.bg-green-200.cursor-pointer(class='hover:bg-purple-700' @click='ggl(i.Title)')
  span.text-red-400.font-bold.text-3xl.flex-1.text-center {{i.Title}}
  img.rounded-xl(:src='i.Poster')
  .flex.justify-between
    span.text-blue-400 {{i.Type}}
    span {{i.Year}}
</template>

<script>
export default {
  props:['i'],
  methods:{
    ggl(i){
      window.location.href = `https://www.google.com/search?q=${i}`
    }
  }
}
</script>

<style>
.w-200{
  width: 19%;
}
</style>