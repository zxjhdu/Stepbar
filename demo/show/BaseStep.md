一。组件使用
<template>
<div>
    <BaseStep :currentStep="currentStep" :stepsLabel="stepsLabel" :stepsDesc="stepsDesc" @change="onChange">
    <div slot="1">一步骤显示内容</div>
    <div slot="2">二步骤显示内容</div>
    <div slot="3">三步骤显示内容</div>
    <div slot="4">四步骤显示内容 </div>
    </BaseStep>
    <!--接口信息    -->
    </div>
</template>

 <script>
import BaseStep from '@/components/BaseStep'
export default {
components: {
    BaseStep
},
data () {
    return {
      stepsLabel: ['Step 1', 'Step 2', 'Step 3', 'Step 4'],
      stepsDesc: ['description 1', 'description 2', 'description 3', 'description 4'],
      currentStep:1
    }
},
methods: {
    onChange (index) { // 父组件获取切换后的选中步骤
      console.log('parentIndex:', index);
      this.currentStep=index
    }
}
}
 </script>




二.组件内容：
<template>
  <div>
  <div class="m-steps-area"><!--syep组件设置居中-->
    <div class="m-steps"><!--弹性盒子 布局-->
      <div
        :class="['m-steps-item',
          { 'finished': current > n,
            'process': current === n && n !== totalSteps,
            'last-process': current === totalSteps && n === totalSteps,
            'middle-wait': current < n && n !== totalSteps,
            'last-wait': current < n && n === totalSteps,
          }
        ]"
        v-for="n in totalSteps"
        :key="n"
        @click="onChange(n)">
        <div class="m-steps-icon">  <!--原型图标的父元素     m-steps-icon基础样式，可以和上面父元素不同阶段的class配合生成不同的样式（也包括等待  惊醒中  完成）-->
          <span class="u-icon" v-if="current<=n">{{ n }}</span>
          <!--原型图标的元素，根据当前进行的步骤分为两种样式，一种为还未到该步骤（显示该步骤数），另一种为超过或者正在进行 （显示√）-->    
          <span class="u-icon" v-else>✓</span>
        </div>
        <div class="m-steps-content"> <!--步骤的名称  以及步骤的描述信息-->
          <div class="u-steps-title">{{ stepsLabel[n-1] || 'S ' + n }}</div>   <!--步骤的名称，即步骤数组中的第n-1个元素  --> 
          <div class="u-steps-description">{{ stepsDesc[n-1] || 'Desc ' + n }}</div><!--步骤的描述信息描述数组中的第n-1个元素-->
        </div>
      </div>
    </div>
  </div>
    <div class="slot" v-for="k in totalSteps" :key="k" v-show="k==current"> 
      <slot :name="k" >  显示内容{{k}}</slot>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    stepsLabel: { // 步骤title数组
      type: Array,
      default: () => {//给了默认值和传递的类型
        return []
      }
    },
    stepsDesc: { // 步骤description数组
      type: Array,
      default: () => {
        return []
      }
    },
    currentStep: { // 当前选中的步骤
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      // 若当前选中步骤超过总步骤数，则默认选择步骤1
      current: this.currentStep > this.totalSteps ? 1 : this.currentStep,
      totalSteps:this.stepsLabel.length//通过传递进来的步骤数组来计算总的步骤数，方便循环生成标签和判断最后的步骤条
    }
  },
  methods: {
    onChange (index) { // 点击切换选择步骤
      console.log('index:', index)
      if (this.current !== index) {
        this.current = index
        this.$emit('change', index)
      }
    }
  }
}
</script>
<style lang="less" scoped>
.slot {  //1插槽的样式，
  width: 1100px;
  margin:0 auto; 
  }
.m-steps-area {//2所有步骤块基本的样式，规定了步骤块的大小等等
  width: 1100px;
  margin: 0px auto;//所有居中区域
  .m-steps {   //这部分也是步骤块的基本样式，开启了弹性盒子布局
    padding: 30px 0;
    display: flex;
    .m-steps-item {//3每个步骤条的内部图标和文字的基本样式，文字行距、文字大小等等
      display: inline-block;//
      flex: 1; // 弹性盒模型对象的子元素都有相同的长度，且忽略它们内部的内容   flex 属性用于设置或检索弹性盒模型对象的子元素如何分配空间。flex 属性是 flex-grow、flex-shrink 和 flex-basis 属性的简写属性。
      overflow: hidden;//裁掉超出的线
      font-size: 16px;//文字大小
      line-height: 32px;//行距
      .m-steps-icon {//4每个步骤条分为图标和标题部分，这个属于图标的基本样式
        display: inline-block;
        margin-right: 8px;
        width: 32px;
        height: 32px;
        border-radius: 50%;//把border变成圆形
        text-align: center;
      }
      .m-steps-content {//每个步骤条的标题的基本样式
        display: inline-block;
        vertical-align: top;//顶部对其
        padding-right: 16px;
        .u-steps-title {
          position: relative;
          display: inline-block;
          padding-right: 16px;
        }
        .u-steps-description {//每个步骤条的描述基本样式，位于标题的下面
          font-size: 14px;
          max-width: 140px;
        }
      }
    }
    .finished {           //组件内部图标和文字的完成样式（图标蓝色   文字黑色）
      margin-right: 16px;
      cursor: pointer;
      &:hover {
        .m-steps-content {    //在完成状态下鼠标移入时候步骤标题和注释的样式会改变颜色
          .u-steps-title {
            color: #1890ff;    
          }
          .u-steps-description {
            color: #1890ff;
          }
        }
      }
      .m-steps-icon {     //完成状态的图标样式
        background: #fff;
        border: 1px solid rgba(0,0,0,.25);
        border-color: #1890ff;
        .u-icon {
          color: #1890ff;//√
        }
      }
      .m-steps-content {    //完成状态下步骤标题和注释的样式
        color: rgba(0,0,0,.65);
        .u-steps-title {
          color: rgba(0,0,0,.65);
          &:after {        //加一个伪元素     通过:before和:after两个伪元素，在文字两边画线，设置height：1px;再通过控制伪元素的width属性来控制线的长短。
            background: #1890ff;//线的颜色
            position: absolute;
            top: 16px;
            left: 100%;//定位到步骤条title的最右边,并超出父元素被最外一层元素hidden
            display: block;
            width: 9999px;//超出外标签长度，但会被hidden裁剪
            height: 1px;//并把内容变成一条线，因为高度为1  然后调整背景颜色便可以设置线的颜色
            content: "";
          }
        }
        .u-steps-description {
          color: rgba(0,0,0,.45);
        }
      }
    }
    .process {
      margin-right: 16px;
      .m-steps-icon {
        background: #1890ff;
        border: 1px solid rgba(0,0,0,.25);
        border-color: #1890ff;
        .u-icon {
          color: #fff;
        }
      }
      .m-steps-content {
        color: rgba(0,0,0,.65);
        .u-steps-title {
          font-weight: 600;
          color: rgba(0,0,0,.85);
          &:after {    //！！画后面的线，在last process 和finish中没有
            background: #e8e8e8;
            position: absolute;
            top: 16px;
            left: 100%;
            display: block;
            width: 9999px;
            height: 1px;
            content: "";
          }
        }
        .u-steps-description {
          color: rgba(0,0,0,.65);
        }
      }
    }
    .last-process {
      margin-right: 0;//因为是最后一个了
      .m-steps-icon {
        background: #1890ff;
        border: 1px solid rgba(0,0,0,.25);
        border-color: #1890ff;
        .u-icon {
          color: #fff;
        }
      }
      .m-steps-content {
        color: rgba(0,0,0,.65);
        .u-steps-title {
          font-weight: 600;
          color: rgba(0,0,0,.85);
        }
        .u-steps-description {
          color: rgba(0,0,0,.65);
        }
      }
    }
    .middle-wait {
      margin-right: 16px;
      cursor: pointer;
      &:hover {
        .m-steps-icon {
          border: 1px solid #1890ff;
          .u-icon {
            color: #1890ff;
          }
        }
        .m-steps-content {
          .u-steps-title {
            color: #1890ff;
          }
          .u-steps-description {
            color: #1890ff;
          }
        }
      }
      .m-steps-icon {
        background: #fff;
        border: 1px solid rgba(0,0,0,.25);
        .u-icon {
          color: rgba(0,0,0,.25);
        }
      }
      .m-steps-content {
        color: rgba(0,0,0,.65);
        .u-steps-title {
          color: rgba(0,0,0,.45);
          &:after {
            background: #e8e8e8;
            position: absolute;
            top: 16px;
            left: 100%;
            display: block;
            width: 9999px;
            height: 1px;
            content: "";
          }
        }
        .u-steps-description {
          color: rgba(0,0,0,.45);
        }
      }
    }
    .last-wait {
      margin-right: 0;
      cursor: pointer;
      &:hover {
        .m-steps-icon {
          border: 1px solid #1890ff;
          .u-icon {
            color: #1890ff;
          }
        }
        .m-steps-content {
          .u-steps-title {
            color: #1890ff;
          }
          .u-steps-description {
            color: #1890ff;
          }
        }
      }
      .m-steps-icon {
        background: #fff;
        border: 1px solid rgba(0,0,0,.25);
        .u-icon {
          color: rgba(0,0,0,.25);
        }
      }
      .m-steps-content {
        color: rgba(0,0,0,.65);
        .u-steps-title {
          color: rgba(0,0,0,.45);
        }
        .u-steps-description {
          color: rgba(0,0,0,.45);
        }
      }
    }
  }
}
</style>