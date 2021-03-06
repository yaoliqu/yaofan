<template>
  <view class="almost-lottery" :style="{ width: canvasWidth + 44 + 'px', height: canvasHeight + 44 + 'px'}">
    <view class="almost-lottery-wrap" :style="{width: canvasWidth + 'px', height: canvasHeight + 'px'}">
      <!-- #ifdef MP-ALIPAY -->
      <canvas :class="className" :id="canvasId" :width="canvasWidth" :height="canvasHeight" :style="{
         width: canvasWidth + 'px',
         height: canvasHeight + 'px'
       }" />
      <!-- #endif -->
      <!-- #ifndef MP-ALIPAY -->
      <canvas :class="className" :canvas-id="canvasId" :width="canvasWidth" :height="canvasHeight" :style="{
         width: canvasWidth + 'px',
         height: canvasHeight + 'px'
       }" />
      <!-- #endif -->
      <image class="canvas-img" :src="canvasImg" :style="{
         width: canvasWidth + 'px',
         height: canvasHeight + 'px',
         transform: `rotate(${canvasAngle + targetAngle}deg)`,
         transitionDuration: `${transitionDuration}s`
       }"
        v-if="canvasImg"></image>
      <view class="almost-lottery__action" @click="handleActionStart"></view>
      <!-- 为了兼容 app 端 ctx.measureText 所需的标签 -->
      <text class="almost-lottery__measureText">{{ measureText }}</text>
    </view>
  </view>
</template>

<script>
  export default {
    name: 'RaffleWheel',
    props: {
      // canvas 宽度
      canvasWidth: {
        type: Number,
        default: 240
      },
      // canvas 高度
      canvasHeight: {
        type: Number,
        default: 240
      },
      // 奖品列表
      prizeList: {
        type: Array,
        // 必须是偶数
        validator: function(value) {
          return value.length % 2 === 0
        },
        required: true
      },
      // 奖品区块对应背景颜色
      colors: {
        type: Array,
        default: () => [
          '#FFFFFF',
          '#FFE9AA'
        ],
        // 必须是偶数且仅为 2 个颜色相互交替
        validator: function(value) {
          return value.length === 2
        }
      },
      // 旋转动画时间 单位s
      duration: {
        type: Number,
        default: 8
      },
      // 旋转的圈数
      ringCount: {
        type: Number,
        default: 8
      },
      // 字体颜色
      fontColor: {
        type: String,
        default: '#C30B29'
      },
      // 文字的大小
      fontSize: {
        type: Number,
        default: 12
      },
      // 奖品文字多行情况下的行高
      lineHeight: {
        type: Number,
        default: 20
      },
      // 奖品名称所对应的 key 值
      strKey: {
        type: String,
        default: 'name'
      },
      // 奖品文字总长度限制
      strMaxLength: {
        type: Number,
        default: 12
      },
      // 奖品文字多行情况下第一行文字长度
      strLineLength: {
        type: Number,
        default: 6
      }
    },
    data() {
      return {
        // 画板className
        className: 'almost-lottery__canvas',
        // 画板标识
        canvasId: 'almostLotteryCanvas',
        // 画板导出的图片
        canvasImg: '',
        // 旋转到奖品目标需要的角度
        targetAngle: 0,
        // 旋转动画时间 单位 s
        transitionDuration: 0,
        // 是否正在旋转
        isRotate: false,
        // 当前停留在那个奖品的序号
        stayIndex: 0,
        // 解决 app 不支持 measureText 的问题
        measureText: ''
      }
    },
    computed: {
      // 设备像素密度
      pixelRatio () {
        return uni.getSystemInfoSync().pixelRatio
      },
      // 根据奖品列表计算 canvas 旋转角度
      // 让 启动按钮指针 在奖品分区中间 position = 45
      // 让 启动按钮指针 在奖品分区边界 position = 90
      canvasAngle () {
        let prizeCount = this.prizeList.length
        let position = 90
        if (prizeCount % 4 !== 0) {
          return 0
        } else {
          let num = prizeCount / 4
          return num % 2 === 0 ? position / num : position
        }
      },
      // 根据画板的宽度计算奖品文字与中心点的距离
      textRadius () {
        return Math.round(this.canvasWidth / 2.4)
      }
    },
    methods: {
      // 开始旋转
      onRotateStart (targetIndex) {
        // 奖品总数
        let prizeCount = this.prizeList.length
        let baseAngle = 360 / prizeCount
        let angles = 0
        if (this.targetAngle === 0) {
          console.log('第一次旋转')
          // 因为第一个奖品是从0°开始的，即水平向右方向
          // 第一次旋转角度 = 270度 - (停留的序号-目标序号) * 每个奖品区间角度 - 每个奖品区间角度的一半 - canvas自身旋转的度数
          angles = (270 - (targetIndex - this.stayIndex) * baseAngle - baseAngle / 2) - this.canvasAngle
        } else {
          console.log('后续旋转')
          // 后续继续旋转 就只需要计算停留的位置与目标位置的角度
          angles = -(targetIndex - this.stayIndex) * baseAngle
        }
        // 更新目前序号
        this.stayIndex = targetIndex
        // 转 8 圈，圈数越多，转的越快
        this.targetAngle += angles + 360 * this.ringCount

        // 计算转盘结束的时间，预加一些延迟确保转盘停止后触发结束事件
        let endTime = this.transitionDuration * 1000 + 100
        setTimeout(() => {
          this.isRotate = false
          this.$emit('draw-end')
        }, endTime)
      },
      // 点击 开始抽奖 按钮
      handleActionStart () {
        if (this.isRotate) return
        this.isRotate = true
        this.$emit('draw-start')
      },
      // 渲染转盘
      async onCreateCanvas () {
        // 获取 canvas 画布
        const canvasId = this.canvasId
        const ctx = uni.createCanvasContext(canvasId, this)

        // canvas 的宽高
        let canvasW = this.canvasWidth
        let canvasH = this.canvasHeight
        
        // 根据奖品个数计算 角度
        let prizeCount = this.prizeList.length
        let baseAngle = Math.PI * 2 / prizeCount

        // 设置描边颜色
        ctx.setStrokeStyle('#FFBE04')
        
        // 设置字体和字号
        // #ifndef MP
        let fontFamily = '-apple-system, BlinkMacSystemFont, \'PingFang SC\', \'Helvetica Neue\', STHeiti, \'Microsoft Yahei\', Tahoma, Simsun, sans-serif'
        ctx.font = `${this.fontSize}px ${fontFamily}`
        // #endif
        // #ifdef MP
        ctx.setFontSize(this.fontSize)
        // #endif

        // 注意，开始画的位置是从0°角的位置开始画的。也就是水平向右的方向。
        // 画具体内容
        for (let i = 0; i < prizeCount; i++) {
          // 当前角度
          let angle = i * baseAngle

          // 保存当前画布的状态
          ctx.save()

          // 开始画内容
          ctx.beginPath()

          // 开始画圆弧
          // x => 圆弧对应的圆心横坐标 x
          // y => 圆弧对应的圆心横坐标 y
          // radius => 圆弧的半径大小
          // startAngle => 圆弧开始的角度，单位是弧度
          // endAngle => 圆弧结束的角度，单位是弧度
          // anticlockwise(可选) => 绘制方向，true 为逆时针，false 为顺时针
          let outsideRadius = canvasW / 2
          let insideRadius = 20
          ctx.arc(canvasW * 0.5, canvasH * 0.5, outsideRadius, angle, angle + baseAngle, false)
          ctx.arc(canvasW * 0.5, canvasH * 0.5, insideRadius, angle + baseAngle, angle, true)

          // 开始链接线条
          ctx.stroke()
          // 每个奖品区块背景填充颜色
          ctx.setFillStyle(this.colors[i % 2])
          // 填充颜色
          ctx.fill()

          // 开始绘制奖品内容
          ctx.setFillStyle(this.fontColor)
          let rewardName = this.strLimit(this.prizeList[i][this.strKey])

          // translate方法重新映射画布上的 (0,0) 位置
          let translateX = canvasW * 0.5 + Math.cos(angle + baseAngle / 2) * this.textRadius
          let translateY = canvasH * 0.5 + Math.sin(angle + baseAngle / 2) * this.textRadius
          ctx.translate(translateX, translateY)

          // rotate方法旋转当前的绘图，因为文字是和当前扇形中心线垂直的
          ctx.rotate(angle + (baseAngle / 2) + (Math.PI / 2))

          // 设置文本位置并处理换行

          // 是否需要换行
          let isLineBreak = rewardName.length > this.strLineLength
          if (isLineBreak) {
            // 获得多行文本数组
            rewardName = rewardName.substring(0, this.strLineLength) + ',' + rewardName.substring(this.strLineLength)
            let rewardNames = rewardName.split(',')

            // 循环文本数组，计算每一行的文本宽度
            for (let j = 0; j < rewardNames.length; j++) {
              if (ctx.measureText && ctx.measureText(rewardNames[j]).width) {
                // 文本的宽度信息
                let tempStrSize = ctx.measureText(rewardNames[j])
                ctx.fillText(rewardNames[j], -tempStrSize.width / 2, j * this.lineHeight)
              } else {
                this.measureText = rewardNames[j]

                // 等待页面重新渲染
                await this.$nextTick()

                let textWidth = await this.getTextWidth()

                ctx.fillText(rewardNames[j], -textWidth / 2, j * this.lineHeight)
                // console.log(rewardNames[j], textWidth, i)
              }
            }
          } else {
            if (ctx.measureText && ctx.measureText(rewardName).width) {
              // 文本的宽度信息
              let tempStrSize = ctx.measureText(rewardName)
              ctx.fillText(rewardName, -tempStrSize.width / 2, 0)
            } else {
              this.measureText = rewardName

              // 等待页面重新渲染
              await this.$nextTick()

              let textWidth = await this.getTextWidth()

              ctx.fillText(rewardName, -textWidth / 2, 0)
            }
          }

          ctx.restore()
        }

        // 保存绘图并导出图片
        ctx.draw(true, () => {
          let drawTimer = setTimeout(() => {
            clearTimeout(drawTimer)
            drawTimer = null
            
            // #ifdef MP-ALIPAY
            ctx.toTempFilePath({
              destWidth: this.canvasWidth * this.pixelRatio,
              destHeight: this.canvasHeight * this.pixelRatio,
              success: (res) => {
                // console.log(res.filePath)
                this.canvasImg = res.filePath
                // 通知父级组件，抽奖转品生成图片完成
                this.$emit('finish')
              }
            })
            // #endif
            // #ifndef MP-ALIPAY
            uni.canvasToTempFilePath({
              destWidth: this.canvasWidth * this.pixelRatio,
              destHeight: this.canvasHeight * this.pixelRatio,
              canvasId: this.canvasId,
              success: (res) => {
                // 在 H5 平台下，tempFilePath 为 base64
                // console.log(res.tempFilePath)
                this.canvasImg = res.tempFilePath
                // 通知父级组件，抽奖转品生成图片完成
                this.$emit('finish')
              }
            }, this)
            // #endif
          }, 20)
        })
      },
      // 兼容 app 端不支持 ctx.measureText
      // 已知问题：初始绘制时，低端安卓机 平均耗时 2s
      getTextWidth () {
        return new Promise((resolve, reject) => {
          uni.createSelectorQuery().in(this).select('.almost-lottery__measureText').fields({
            size: true,
          }, (res) => {
            resolve(res.width)
          }).exec()
        })
      },
      // 处理文字溢出
      strLimit (value) {
        let maxLength = this.strMaxLength
        if (!value || !maxLength) return value
        return value.length > maxLength ? value.slice(0, maxLength - 1) + '...' : value
      }
    },
    mounted () {
      this.$nextTick(() => {
        let stoTimer = setTimeout(() => {
          clearTimeout(stoTimer)
          stoTimer = null
          
          this.onCreateCanvas()
          this.transitionDuration = this.duration
        }, 50)
      })
    }
  }
</script>

<style lang="scss" scoped>
  $actionBgUrl: '~static/almost-lottery/almost-lottery__action';
  $raffleBgUrl: '~static/almost-lottery/almost-lottery__bg';

  .almost-lottery {
    position: relative;
    left: 0;
    top: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .almost-lottery {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: contain;
    background-image: url($raffleBgUrl + ".png");

    @media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
      background-image: url($raffleBgUrl + "@2x.png");
    }

    @media (-webkit-min-device-pixel-ratio: 3),
    (min-device-pixel-ratio: 3) {
      background-image: url($raffleBgUrl + "@3x.png");
    }
  }

  .almost-lottery__canvas {
    position: absolute;
    left: -9999px;
    opacity: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .almost-lottery__action {
    position: absolute;
    top: calc(50% - 58px);
    left: calc(50% - 58px);
    width: 114px;
    height: 114px;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: contain;
    background-image: url($actionBgUrl + ".png");

    @media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
      background-image: url($actionBgUrl + "@2x.png");
    }

    @media (-webkit-min-device-pixel-ratio: 3),
    (min-device-pixel-ratio: 3) {
      background-image: url($actionBgUrl + "@3x.png");
    }
  }

  .almost-lottery__measureText {
    position: absolute;
    left: 0;
    top: 0;
    white-space: nowrap;
    font-size: 12px;
    opacity: 0;
  }

  .canvas-img {
    transition: transform cubic-bezier(.34, .12, .05, .95);
  }
</style>
