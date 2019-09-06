<template>
  <div class="item">
    <div class="player">
      <video-player class="vjs-custom-skin" :options="playerOptions">
      </video-player>
    </div>
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
                playerOptions: {
                    height: '1000',
                    sources: [{
                        type: 'rtmp/flv',
                        src: 'rtmp://47.96.237.94:1935/live/mylive'
                    }],
                    techOrder: ['flash'],
                    muted: false, // 默认静音
                    preload: 'auto', // 视频预加载
                    autoplay: true,
                    controls: true,
                    notSupportedMessage: '此视频暂无法播放，请稍后再试',
                    poster: "https://surmon-china.github.io/vue-quill-editor/static/images/surmon-9.jpg"
                }

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
            }
        }
    }


</script>
<style lang="scss">


</style>
