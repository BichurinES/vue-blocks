<template>
  <div class="vue-block" :class="{selected: selected}" :style="style">
    <header :style="headerStyle">
      {{ stage || title }}
      <a class="delete" @click="deleteBlock">x</a>
    </header>
    <div class="inputs">
      <div class="input" v-for="(slot, index) in inputs">
        <div class="circle inputSlot" :class="{active: slot.active}"
             @mouseup="slotMouseUp($event, index)"
             @mousedown="slotBreak($event, index)"></div>
      </div>
    </div>
    <div class="outputs">
      <div class="output" :class="{'output_no-answer': !slot.label}" v-for="(slot, index) in outputs">
        <div
            class="circle"
            :class="{
              active: slot.active,
              circle_checkbox: type === 'checkbox'
            }"
            @mousedown="slotMouseDown($event, index)">
          <IconCheck class="circle_checkbox-check" v-if="type === 'checkbox'" />
        </div>
        {{slot.label}}
        <img v-if="type === 'range'" class="output__img" src="https://lh3.googleusercontent.com/kT0mNYkbw5j6ZtQx2bgwQipVHheZf7K7ut6lGXRugITIdOawOZhzN2N-ku99zBu6pMEFENskgJQD_ITupeNxWJRhQzXQ3Xe9W-uE3Ovl-Hbybi0OPg=w1064-v0">
        <img v-if="type === 'textarea'" class="output__img" src="https://icon-library.com/images/textbox-icon/textbox-icon-12.jpg">
        <img v-if="type === 'file'" class="output__img" src="https://cdn.picpng.com/icon/upload-files-icon-66764.png">
        <img v-if="type === 'image'" class="output__img" src="data:image/png;base64,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">
      </div>
    </div>
  </div>
</template>

<script>
import IconCheck from './IconCheck'

  export default {
    name: 'VueBlock',
    components: {
      IconCheck
    },
    props: {
      x: {
        type: Number,
        default: 0,
        validator: function (val) {
          return typeof val === 'number'
        }
      },
      y: {
        type: Number,
        default: 0,
        validator: function (val) {
          return typeof val === 'number'
        }
      },
      selected: Boolean,
      title: {
        type: String,
        default: 'Title'
      },
      stage: {
        type: String,
        default: ''
      },
      type: {
        type: String,
        default: ''
      },
      inputs: Array,
      outputs: Array,

      options: {
        type: Object
      }
    },
    created () {
      this.mouseX = 0
      this.mouseY = 0

      this.lastMouseX = 0
      this.lastMouseY = 0

      this.linking = false
      this.dragging = false
    },
    mounted () {
      document.documentElement.addEventListener('mousemove', this.handleMove, true)
      document.documentElement.addEventListener('mousedown', this.handleDown, true)
      document.documentElement.addEventListener('mouseup', this.handleUp, true)
    },
    beforeDestroy () {
      document.documentElement.removeEventListener('mousemove', this.handleMove, true)
      document.documentElement.removeEventListener('mousedown', this.handleDown, true)
      document.documentElement.removeEventListener('mouseup', this.handleUp, true)
    },
    data () {
      return {
        width: this.options.width,
        hasDragged: false
      }
    },
    methods: {
      handleMove (e) {
        this.mouseX = e.pageX || e.clientX + document.documentElement.scrollLeft
        this.mouseY = e.pageY || e.clientY + document.documentElement.scrollTop

        if (this.dragging && !this.linking) {
          let diffX = this.mouseX - this.lastMouseX
          let diffY = this.mouseY - this.lastMouseY

          this.lastMouseX = this.mouseX
          this.lastMouseY = this.mouseY

          this.moveWithDiff(diffX, diffY)

          this.hasDragged = true
        }
      },
      handleDown (e) {
        this.mouseX = e.pageX || e.clientX + document.documentElement.scrollLeft
        this.mouseY = e.pageY || e.clientY + document.documentElement.scrollTop

        this.lastMouseX = this.mouseX
        this.lastMouseY = this.mouseY

        const target = e.target || e.srcElement
        if (this.$el.contains(target) && e.which === 1) {
          this.dragging = true

          this.$emit('select')

          if (e.preventDefault) e.preventDefault()
        }
      },
      handleUp () {
        if (this.dragging) {
          this.dragging = false

          if (this.hasDragged) {
            this.save()
            this.hasDragged = false
          }
        }

        if (this.linking) {
          this.linking = false
        }
      },
      // Slots
      slotMouseDown (e, index) {
        this.linking = true

        this.$emit('linkingStart', index)
        if (e.preventDefault) e.preventDefault()
      },
      slotMouseUp (e, index) {
        this.$emit('linkingStop', index)
        if (e.preventDefault) e.preventDefault()
      },
      slotBreak (e, index) {
        this.linking = true

        this.$emit('linkingBreak', index)
        if (e.preventDefault) e.preventDefault()
      },
      save () {
        this.$emit('update')
      },
      deleteBlock () {
        this.$emit('delete')
      },
      moveWithDiff (diffX, diffY) {
        let left = this.x + diffX / this.options.scale
        let top = this.y + diffY / this.options.scale

        this.$emit('update:x', left)
        this.$emit('update:y', top)
      }
    },
    computed: {
      style () {
        return {
          top: this.options.center.y + this.y * this.options.scale + 'px',
          left: this.options.center.x + this.x * this.options.scale + 'px',
          width: this.width + 'px',
          transform: 'scale(' + (this.options.scale + '') + ')',
          transformOrigin: 'top left'
        }
      },
      headerStyle () {
        return {
          height: this.options.titleHeight + 'px'
        }
      }
    }
  }
</script>

<style lang="less" scoped>
  @blockBorder: 1px;

  @ioPaddingInner: 2px 0;
  @ioHeight: 16px;
  @ioFontSize: 14px;

  @circleBorder: 1px;
  @circleSize: 12px;
  @circleMargin: 2px; // left/right

  @circleNewColor: #1e255b;
  @circleRemoveColor: #FF0000;
  @circleConnectedColor: #4C5CE3;

  .vue-block {
    position: absolute;
    box-sizing: border-box;
    background: white;
    z-index: 1;
    opacity: 0.9;
    cursor: move;
    border-radius: 5px;
    -webkit-box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16), 0px 3px 6px rgba(0, 0, 0, 0.23);
    box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16), 0px 3px 6px rgba(0, 0, 0, 0.23);
    padding-bottom: 4px;

    &.selected {
      border: @blockBorder solid red;
      z-index: 2;
    }

    > header {
      background: #171c44;
      text-align: center;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      padding: 4px 10px;
      border-radius: 5px 5px 0 0;
      margin-bottom: 4px;
      color: #fff;
      > .delete {
        color: red;
        cursor: pointer;
        float: right;
        position: absolute;
        right: 5px;
      }
    }

    .inputs, .outputs {
      padding: @ioPaddingInner;
      display: block;
      > * {
        width: 100%;
      }
    }

    .inputs {
      width: 7%;
    }

    .outputs {
      width: 90%;
    }

    .circle {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      box-sizing: border-box;
      margin-top: @ioHeight / 2 - @circleSize / 2;

      width: @circleSize;
      height: @circleSize;

      border: @circleBorder solid #bfbfbf;
      border-radius: 100%;

      cursor: crosshair;
      &::after {
        content: '';
        position: absolute;
        display: block;
        top: 50%;
        left: 50%;
        transform: translateX(-50%) translateY(-50%);
        width: @circleSize * .6;
        height: @circleSize * .6;
        border: @circleSize * .1 solid white;
        border-radius: 50%;
        background: #fff;
      }
      &.active::after {
        background: @circleConnectedColor;
      }
      &.active {
        background: @circleConnectedColor;
      }
      &_checkbox {
        border-radius: 20%;
        &-check {
          fill: #fff;
          width: @circleSize * .6;
          height: @circleSize * .6;
        }
      }
      &_checkbox::after {
        display: none;
      }
      &_checkbox.active {
        background-color: @circleConnectedColor;
      }

    }

    .inputs {
      float: left;
      text-align: left;

      margin-left: -(@circleSize/2 + @blockBorder);
    }

    .input, .output {
      height: @ioHeight;
      overflow: hidden;
      font-size: @ioFontSize;

      &:last-child {
      }
    }

    .input {
      float: left;

      .circle {
        float: left;
        margin-right: @circleMargin;

        &:hover {
          background: @circleNewColor;
          &::after {
            background: @circleNewColor;
          }
          &.active {
            background: @circleRemoveColor;
            &::after {
              background: @circleRemoveColor;
            }
          }
        }
      }
    }

    .outputs {
      float: right;
      text-align: right;

      margin-right: -(@circleSize/2 + @blockBorder);
    }

    .output {
      float: right;
      &_no-answer {
        height: 70px;
      }
      .circle {
        float: right;
        margin-left: @circleMargin;

        &:hover {
          background: @circleNewColor;
          &::after {
            background: @circleNewColor;
          }
        }
      }
      &__img {
        width: 90%;
        height: 100%;
        object-fit: contain;
      }
    }
  }
</style>
