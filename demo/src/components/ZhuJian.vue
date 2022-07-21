<template>
<div>
    <BaseStep :currentStep="currentStep" :stepsLabel="stepsLabel" :stepsDesc="stepsDesc" @change="onChange">
    <div slot="1">一步骤显示内容</div>
    <div slot="2">二步骤显示内容</div>
    <div slot="3">三步骤显示内容</div>
    <div slot="4">四步骤显示内容</div>
    
    </BaseStep>
    <!--接口信息①    -->
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
