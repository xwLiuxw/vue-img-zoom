<template>
    <div
        class="box"
        ref="parentBox"
        @mousemove="mousemove($event)"
        @mouseleave="mouseleave"
    >
        <div class="content">
            <img :src="url" alt="" ref="originalImg" />
        </div>
        <div class="cover" ref="coverBox" v-show="zooming"></div>
        <div class="zoom-box" v-show="zooming">
            <div class="zoom" ref="zoom">
                <div class="zoom-pic" ref="zoomPic">
                    <img :src="url" alt="" ref="zoomImg" />
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data () {
        return {
            zooming: false,
            leftRange: [],
            TopRange: []
        }
    },
    props: {
        url: String,
        scale: Number,
        width: Number,
        height: Number
    },
    computed: {
        coverBox () {
            return this.$refs['coverBox']
        },
        parentBox () {
            return this.$refs['parentBox']
        },
        zoom () {
            return this.$refs['zoom']
        },
        zoomPic () {
            return this.$refs['zoomPic']
        },
        originalImg () {
            return this.$refs['originalImg']
        },
        zoomImg () {
            return this.$refs['zoomImg']
        }
    },
    watch: {
        'url': function () { // 获取图片比例
            let img = new Image()
            img.src = this.url
            img.onload = () => {
                if (img.width / img.height > 1) {
                    this.originalImg.style.width = '100%'
                    this.originalImg.style.height = 'auto'
                    this.zoomImg.style.width = '100%'
                    this.zoomImg.style.height = 'auto%'
                } else {
                    this.originalImg.style.height = '100%'
                    this.originalImg.style.width = 'auto'
                    this.zoomImg.style.height = '100%'
                    this.zoomImg.style.width = 'auto'
                }
            }
        }
    },
    mounted () {
        this.zoom.style.width = `${100 * this.scale}%`
        this.zoom.style.height = `${100 * this.scale}%`
        this.coverBox.style.width = `${100 / this.scale}%`
        this.coverBox.style.height = `${100 / this.scale}%`
        this.leftRange = [this.width / this.scale / 2, this.width - this.width / this.scale / 2]
        this.TopRange = [this.height / this.scale / 2, this.height - this.height / this.scale / 2]
    },
    methods: {
        mousemove (event) {
            if (!this.zooming) {
                this.zooming = true
            }
            let parentX = this.parentBox.getBoundingClientRect().left // 父层 位置 防止页面滚动，所以每一次都重新获取
            let parentY = this.parentBox.getBoundingClientRect().top // 父层 位置
            let { clientX, clientY } = event // 记录鼠标的坐标
            let coverX = this.getRangeValue(this.leftRange, clientX - parentX)// 遮罩层移动的X轴距离
            let coverY = this.getRangeValue(this.TopRange, clientY - parentY)// 遮罩层移动的X轴距离
            this.coverBox.style.left = coverX + 'px'
            this.coverBox.style.top = coverY + 'px'
            this.zoomPic.style.left = `-${(coverX) * this.scale - this.width / 2}px`// 放大层移动的距离
            this.zoomPic.style.top = `-${coverY * this.scale - this.height / 2}px`// 放大层移动的距离
        },
        mouseleave () {
            this.zooming = false
        },
        /**
         * 获取范围内的值
         *  arr 范围数组
         * value 当前值
        */
        getRangeValue (arr, value) {
            if (value < arr[0]) {
                return arr[0]
            } else if (value > arr[1]) {
                return arr[1]
            } else {
                return value
            }
        }
    }
}
</script>
<style lang="scss" scoped>
.box {
    width: 100%;
    height: 100%;
    position: relative;
    .content {
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        img {
            display: block;
            max-width: 100%;
            max-height: 100%;
        }
    }
    .cover {
        background-color: rgba(0, 0, 0, 0.5);
        cursor: move;
        position: absolute;
        top: 0;
        left: 0;
        z-index: 99;
        transform: translate(-50%, -50%);
    }
    .zoom-box {
        position: absolute;
        z-index: 99;
        left: calc(100% + 10px);
        top: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
        box-shadow: 0 0 10px #dfdfdf;
        .zoom {
            position: relative;
            .zoom-pic {
                width: 100%;
                height: 100%;
                position: absolute;
                top: 0;
                left: 0;
                background: #fff;
                display: flex;
                align-items: center;
                justify-content: center;
                img {
                    max-width: 100%;
                    max-height: 100%;
                }
            }
        }
    }
}
</style>
