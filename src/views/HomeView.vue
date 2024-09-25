<script>
import content from '../components/content/index.vue';
import navbar from '../components/navbar/index.vue';
import foot from '../components/footer/index.vue';

export default {
  components:{
    content,
    navbar,
    foot,
  }
}
</script>

<template>
  <div style="width: 100vw;">
    <navbar />
    <content />
    <foot />
  </div>
</template>
