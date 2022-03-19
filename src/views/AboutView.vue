<template>
  <div class="chinese">
      <v-card dark tile style="height: 2000px">
        <v-card-title>
          <h1 class="text-h4 blue--text">本站的定位</h1>
        </v-card-title>
        <v-card-text>
          这是个人的博客网站，用于记录自己在运筹优化领域里学习和工作的所学、所思，并提供该领域的api接口来让大家调用。
          已开放、或者准备开放的api接口如下表所示，大家也可以通过搜索栏来查询所关心的api。
        </v-card-text>
        <SearchTable :headers="headers" :assets="assets"/>
      </v-card>
  </div>
</template>

<script>
import SearchTable from '../components/SearchTable.vue'
export default {
  components:{
    SearchTable
  },

  data(){
    return {
      headers:[
        {
          text:'api名称',
          value:'api',
        },
        {
          text:'描述',
          value:'description',
          filterable: false,
        },
        {
          text:'上线状态',
          value:'status',
          filterable: false
        },
        {
          text:'受欢迎度',
          value:'stars',
          filterable: false
        },
        {
          text:'跳转链接',
          value:'link',
          filterable: false
        }
      ],
      assets:[
        {
          api:'车辆路径规划',
          description: {
            title:'车辆路径规划',
            setting:['车辆路径规划(Vehicle Routing Problem, VRP)通过决策车队的路径，在满足业务约束的条件下服务客户群，以最小化配送成本。','这是一个NP-hard问题，也即客户规模增大，会让求解难度非线性地增长。可用混合整数线性规划来刻画其数学模型，其NP-hard特性也可从整数规划的角度看出来。'],
            category:['根据业务约束的不同，可以将该问题分为不同的类，这通常都是围绕满足需求侧（如客户要求的时间窗）与供给侧（增多车场数量）而进行的。','需求侧：客户可以要求车辆为其配送的时刻在一个固定的区间内，也可以允许车辆迟到（但要车辆附加一个违约成本），相关拓展可参见文末的链接去挖掘。','供给侧：物流提供商可以提供多个车场，以经济地覆盖服务区域，也可以提供多个配送日期供客户挑选（比如京东物流提供配送时段选择），在满足客户时效的同时便于自己制定调度计划，其他拓展参见文末的链接。'],
            solutions:['几乎所有车辆路径规划及其变体，都可用混合整数线性规划对其建模。如果不追求极致的求解速度，那么直接调用商业求解器或开源求解器就是一种直截了当的求解方式。但根据具体的业务约束有意设计大规模分解算法（如Dantzig-Wolf分解与Benders分解），亦或是添加validation cut，都可以让问题的求解时间得到显著提升，这需要分析员的主观能动性。','对于本站，将聚焦于带时间窗的车辆路径规划问题，并提供两类求解方式的接口，第一类是使用开源SCIP求解器，第二类则是使用启发式算法。'],
            references:{'车辆路径规划(VRP)-wikipedia':'https://en.wikipedia.org/wiki/Vehicle_routing_problem',
              '车辆路径规划(VRP)-springer-book':'https://link.springer.com/book/10.1007/978-0-387-77778-8'
            },
            keys:['setting','category','solutions'],
            translate:{
              'setting':'背景知识',
              'category':'分类',
              'solutions':'求解方式',
            }
          },
          status:'1',
          stars: '0',
          link: '/api/vrp'
        },
        {
          api:'列生成框架',
          description: {

          },
          status:'2',
          stars: '0',
          link: '/api/cg/advice'
        },
        {
          api:'行生成框架',
          description: {

          },
          status:'2',
          stars: '0',
          link: '/api/rg/advice'
        }
      ]
    }
  },
  methods:{
    sortBy(prop){
      this.projects.sort((a,b)=> a[prop]<b[prop]? -1:1);
    }
  }
}
</script>

<style>



</style>
