<template>
  <div class="home">
    <h1>Welcome to the animal world</h1>
    <el-carousel :interval="4000" type="card" height="500px">
      <el-carousel-item v-for="item in urls" :key="item">
        <img :src="item" alt="">
      </el-carousel-item>
    </el-carousel>
    <div class="floating-ball" @click="goToShift">
      go
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  components: {
  },
  data () {
    return {
      urls: [
        'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg',
        'https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg',
        'https://fuss10.elemecdn.com/0/6f/e35ff375812e6b0020b6b4e8f9583jpeg.jpeg',
        'https://fuss10.elemecdn.com/9/bb/e27858e973f5d7d3904835f46abbdjpeg.jpeg',
        'https://fuss10.elemecdn.com/d/e6/c4d93a3805b3ce3f323f7974e6f78jpeg.jpeg',
        'https://fuss10.elemecdn.com/3/28/bbf893f792f03a54408b3b7a7ebf0jpeg.jpeg',
        'https://fuss10.elemecdn.com/2/11/6535bcfb26e4c79b48ddde44f4b6fjpeg.jpeg'
      ]
    }
  },
  methods: {
    goToShift() {
      this.$router.push({
        path: '/about'
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.floating-ball {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  position: fixed;
  right: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  border: 1px solid #666666;
  cursor: pointer;
}
.floating-ball:hover{
  background-color: lightblue;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
</style>
