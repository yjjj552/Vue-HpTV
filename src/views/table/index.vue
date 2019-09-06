<template>
  <div class="tabulation">

  <el-row :gutter="20"  id="el" >
  <el-col :span="6" >
    <div class="grid-content bg-purple" >

      <div class="demo-image">
        <div class="block" >
          <el-image
            style="width: 100px; height: 100px"
            :src="url"
            ></el-image>
        </div>
      </div>

   </div>
  </el-col>
  </el-row>
  </div>




</template>

<script>
import { getList } from '@/api/table'

export default {

  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      // playerOptions: {
      //   height: '1000',
      //   sources: [{
      //     type: 'rtmp/flv',
      //     src: 'rtmp://47.96.237.94:1935/live/mylive'
      //   }],
      //   techOrder: ['flash'],
      //   muted: false, // 默认静音
      //   preload: 'auto', // 视频预加载
      //   autoplay: true,
      //   controls: true,
      //   notSupportedMessage: '此视频暂无法播放，请稍后再试',
      //     poster: "https://surmon-china.github.io/vue-quill-editor/static/images/surmon-9.jpg"
      // }
        url: 'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg',
        fits: ['fill', 'contain', 'cover', 'none', 'scale-down'],
        url: 'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'

    }
  },
  created() {
    // this.fetchData()
  },


  methods: {
    fetchData() {
      this.listLoading = true
      getList().then(response => {
        this.list = response.data.items
        this.listLoading = false
      })
    },
      mouseOver: function(){
          this.active = 'background-color: #cccccc';
      },
      mouseLeave: function () {
          this.active = '';
      },

  }
}


</script>
<style lang="scss">
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #e5e9f2;
  }
  .bg-purple {
    background: #cdcdcd;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    /*min-height: 36px;*/
    cursor: pointer;/*鼠标变成手指样式*/
    transition: all 0.2s;/*所有属性变化在0.6秒内执行动画*/
    height:200px;
    border-radius: 6px;
    text-align: center;
    margin-top: 20px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }

  .grid-content:hover{
    transform: scale(1.2);/*鼠标放上之后元素变成1.4倍大小*/
  }
  .tabulation{
    margin: 0 20px 20px 20px;
  }



</style>
