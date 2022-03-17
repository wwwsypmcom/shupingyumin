<template>
  <div class="nopage">
    <h2 class="totle">{{info}}</h2>
  </div>
</template>
<script>
export default {
  data(){
    return{
      info:'漱玉平民简介'
    }
  },
  created(){
    this.info = this.$route.query.name;
  },
  methods:{

  }
}
</script>
<style scoped>
  .totle{
    padding: 20px;
    font-size: 20px;
    color: #666;
    font-weight: bold;
  }
</style>
