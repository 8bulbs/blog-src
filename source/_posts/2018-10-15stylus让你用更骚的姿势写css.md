---
title: stylus让你用更骚的姿势写css
date: 2018-10-15 12:15:17
tags: 开发效率
categories: 开发效率
---

# 认识stylus这货
[张鑫旭老师的stylus文档](https://www.zhangxinxu.com/jq/stylus/)

# 我这里有一波祖传的mixin, 可以让你写更精简可复用的css代码, 这位少侠请了解一下
<!-- more -->
```css

tac()
  text-align center

norpt()
  background-repeat no-repeat

// 文本处理
txt($width, $height, $size)
  width $width
  height $height
  line-height $height
  font-size $size
  tac()
  
// 字体大小
fs($size)
  font-size $size

// 文本加粗
bold()
  font-weight bold

// 行内块
inline()
  display inline-block

// 圆
circle($size)
  width $size
  height $size
  border-radius 100%

// 小圆点
dot($size, $color)
  display inline-block
  width $size
  height $size
  border-radius 100%
  background-color $color

// 模态对话框
modal()
  z-index 1000
  position fixed
  top 0
  right 0
  bottom 0
  left 0
  background-color rgba(0, 0, 0, .6)

// 占满宽度
full-width($height)
  width 375px
  height $height

// 头像
avatar($size)
  norpt()
  width $size
  height $size
  background-size $size $size
  border-radius 100%

// 描线
stroke-line($width, $y)
  width $width
  border 0.5px solid #8A91A4
  transform translateY($y)

// 左浮动
fl($margin-left)
  float left
  margin-left $margin-left

// 文本行
text-line($height, $font-size)
  inline()
  height $height
  line-height $height
  font-size $font-size

// 背景图
bg($width, $height, $img)
  norpt()
  background-size $width $height
  background-image url('../../assets/images/' + $img + '@2x.png')
  @media (-webkit-min-device-pixel-ratio 3), (min-device-pixel-ratio 3)
    background-image url('../../assets/images/' + $img + '@3x.png')

// 带背景图
with-bg($width, $height, $img)
  norpt()
  width $width
  height $height
  background-size $width $height
  background-image url('../../assets/images/' + $img + '@2x.png')
  @media (-webkit-min-device-pixel-ratio 3), (min-device-pixel-ratio 3)
    background-image url('../../assets/images/' + $img + '@3x.png')

// 有背景图
has-bg($width, $height)
  norpt()
  width $width
  height $height
  background-size $width $height

// 组件背景图
bg-img($img)
  background-image url('../../assets/images/' + $img + '@2x.png')
  @media (-webkit-min-device-pixel-ratio 3), (min-device-pixel-ratio 3)
    background-image url('../../assets/images/' + $img + '@3x.png')

// app.vue背景图
app-bg-img($img)
  background-image url('./assets/images/' + $img + '@2x.png')
  @media (-webkit-min-device-pixel-ratio 3), (min-device-pixel-ratio 3)
    background-image url('./assets/images/' + $img + '@3x.png')

// 全屏带背景图
with-bg-fs($img)
  width 375px
  height 667px
  background-image url($img)
  background-size 375px 667px

// 绝对定位, left, top
ab-lt($left, $top)
  position absolute
  top $top
  left $left

// 绝对定位, right, top
ab-rt($right, $top)
  position absolute
  top $top
  right $right

// 绝对定位, left, bottom
ab-lb($left, $bottom)
  position absolute
  left $left
  bottom $bottom

// 绝对定位, right, bottom
ab-rb($right, $bottom)
  position absolute
  right $right
  bottom $bottom

ab-r($right)
  position absolute
  right $right
  top 50%
  transform translateY(-50%)

// 绝对定位, 上下居中, left
ab-l($left)
  position absolute
  left $left
  top 50%
  transform translateY(-50%)

// 绝对定位, 左右居中, top
ab-center-t($top)
  position absolute
  left 50%
  top $top
  transform translateX(-50%)

// 绝对定位, 上下左右都居中
ab-center()
  position absolute
  top 50%
  left 50%
  transform translate(-50%, -50%)

// 不换行
no-wrap()
  text-overflow ellipsis
  overflow hidden
  white-space nowrap

// 扩展点击区域
extend-click()
  &:before
    content ''
    position absolute
    top -10px
    left -10px
    right -10px
    bottom -10px

// 灰阶处理
gray($scale)
  filter grayscale($scale)

// 1px边框
border-1px($color)
  position: relative
  &:after
    display: block
    position: absolute
    left: 0
    bottom: 0
    width: 100%
    border-top: 1px solid $color
    content: ' '
    @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5)
      -webkit-transform: scaleY(0.7)
      transform: scaleY(0.7)
    @media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2)
      -webkit-transform: scaleY(0.5)
      transform: scaleY(0.5)
    @media (-webkit-min-device-pixel-ratio: 3),(min-device-pixel-ratio: 3)
      -webkit-transform: scaleY(0.33)
      transform: scaleY(0.33)

// 占满全屏
full-screen()
  width 100vw
  height 100vh
  position relative

```
# 用法
```javascript
<template>
  <div class="sub-path-card-root">
    <div 
      class="pic" 
      :style="{ backgroundImage: `url(${ imgUrl })` }"
    >
    </div>
    <div class="txt">{{ name }}</div>
    <jsl-stars class="stars" :score=score />
  </div>
</template>


<script type="text/ecmascript-6">
import JslStars from 'base-components/stars/stars'

export default {
  props: ['img', 'score', 'name', 'imgUrl'],
  data () {
    return {
      data: ''
    }
  },
  components: {
    JslStars
  }
}
</script>


<style scoped lang="stylus" rel="stylesheet/stylus">
@import "~styles/mixin"

.sub-path-card-root
  width 101px
  margin 0 auto
  .pic
    margin 0 auto
    has-bg(101px, 101px)
  .bg1
    bg-img('bg1')
  .bg2
    bg-img('bg2')
  .bg3
    bg-img('bg3')
  .txt
    txt(43px, 22px, 16px)
    margin 7px auto
    color #fff
  .stars
    margin 9px auto 20px
</style>
```