
<template>

  <div class="chinese">
    <v-card dark tile height="10000px">
        <v-card-title>
          <h1 class="text-h4 blue--text">车辆路径规划数学模型</h1>
          <v-btn
            icon
            @click="model_show = ! model_show"
          >
            <v-icon>{{ model_show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
          </v-btn>
        </v-card-title>

        <v-expand-transition>
          <div v-show="model_show">
            <v-divider></v-divider>
            <p class="mt-5 mx-5 single-para">
              <span class="blue-bold">带时间窗</span>的车辆路径规划问题是本专栏所关心的问题。以下将对“时间窗”、路径规划所能决策的东西、路径规划的目的进行简介，具体介绍可参阅Bruce Golden等人的
              <a href="https://link.springer.com/book/10.1007/978-0-387-77778-8" class="neat">著作</a>。
              <ul>
                <li>“时间窗”指客户设定了一个服务的时间区间（如早上9点到10点这个区间），
                  在“<span class="blue-bold">硬时间窗</span>”的要求下，车辆必须要在该时间窗内服务客户;
                  在“<span class="blue-bold">软时间窗</span>”的要求下，如果车辆未在规定时间窗内服务客户，那么就要支付违约费用，本专栏只考虑“硬时间窗”。
                </li>
                <li>
                  路径规划能控制的东西，就是决策每辆车从车场出发后<span class="blue-bold">访问客户的顺序</span>，这个顺序需要保证客户<span class="blue-bold">在时间窗内被服务</span>，也要保证车辆服务客户所需要的<span class="blue-bold">容积不超出容量</span>。
                </li>
                <li>
                  路径规划的目的，就是通过控制访问客户的顺序，在满足一些约束的前提下，来<span class="blue-bold">最小化运输成本</span>（通常用运输距离来衡量）。
                </li>
              </ul>
              
            </p>
            <v-card-title>

              <h1 class="text-h5 blue--text">数学符号</h1>
              <v-btn
                icon
                @click="notation_show = ! notation_show"
              >
                <v-icon>{{ notation_show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
              </v-btn>
            </v-card-title>

            <v-expand-transition>
              <div v-show="notation_show">
                <v-divider></v-divider>   
                <p class="mt-5 mx-5 single-para">
                  定义清楚问题只需要盯着三方面的要素：<br>
                  <ul>
                    <li>客户的属性，包括客户需要的商品体积及其时间窗。</li>
                    <li>车辆的属性，包括车辆的最大容量、一天最多能运几次货。</li>
                    <li>运输路径的属性，包括从一个点到另一个点所需要的运输时长和运输成本。</li>
                  </ul>
                  本专栏从这三方面要素起点，定义的符号如下表所示。一开始先引入车场、客户所组成的点集、弧集，然后就对以上三方面要素进行展开。

                </p>
                <v-card-title class="caption pb-0 justify-center">
                  公式显示：由<a href="https://www.mathjax.org/" class="neat">Mathjax2.7.9</a>提供tex公式的渲染。
                </v-card-title>
                $$
                \begin{array}{|c l|}
                \hline
                \text{符号} & \text{含义}\\
                \hline
                \text{集合的定义}\\
                \hline
                V & \begin{array}{l}
                \text{车场、客户的点集，数学形式为} V=\{0^{+},0^{-},1,\dots,|V|\}，\\
                其中0^{+}表示起点车场，0^{-}为终点车场，其他数字表示客户\\
                \end{array}\\ 
                E & 车场、客户两两可以形成的弧集，(i,j)\in E意味着存在一条直接从点i指向点j的弧，i\in V, j\in V, i \neq j\}\\
                E^{+}(i) & 以点i为起点的弧的集合，数学形式为 E^{+}(i)=\{(i,j)| (i,j) \in E, j\in V\}, i\in V\\
                E^{-}(j) & 以点j为终点的弧的集合，数学形式为 E^{-}(j)=\{(i,j)| (i,j) \in E, i\in V\}, j\in V\\

                C & \text{客户的集合，数学形式为} C=\{1,\dots,|V|\}\\
                M & \text{运输车辆的集合，数学形式为}M=\{1,\dots,|M|\}\\
                N & \text{一辆车一天内运输次数的集合，数学形式为}N=\{0,1,\dots,|N|\}\\
                \hline
                \text{客户的属性}\\
                \hline
                q_{i} & \text{客户}i\text{需要的商品体积}，i\in C\\
                a_{i} & \text{客户}i\text{时间窗的最早时刻}，i\in C\\
                b_{i} & \text{客户}i\text{时间窗的最晚时刻}，i\in C\\

                \hline                
                \text{车辆的属性}\\
                \hline
                Q_{k} & \text{车辆}k\text{的最大容量，} k\in M\\
                r_{k} & \text{车辆}k\text{每次回车场后，离再次起点至少需要的时间}，k\in M\\
                
                \hline
                \text{运输路径的属性}\\
                \hline
                c_{ij} & \text{从点}i\text{到点}j\text{所需的运输成本（通常用运输距离代替)}，(i, j)\in E\\
                \tau_{ij} & \text{从点}i\text{到点}j\text{所需的运输时长，其中包含了在点}i\text{进行服务所需的时间}, (i, j)\in E\\
                \hline
                \text{决策变量}\\
                \hline
                x_{ij}^{kn} & 如果\text{车辆}k\text{在第}n\text{次运输中，从}i\text{点直接到}j\text{点},则变量为1,否则为0， (i, j)\in E, k\in M, n\in N\\
                t_{i}^{kn} & \text{车辆}k\text{在第}n\text{次运输中，到达}i\text{点的时刻}， i\in V, k\in K, n\in N\\
                \hline
                \end{array}
                $$
              <p class="mt-5 mx-5 single-para">
                关于集合和属性定义，大家可能有以下疑问，在此作统一回答：
                <ul>
                  <li>
                    <span>关于集合定义</span>
                    <ul>

                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">为什么车场要设为“起点”和”终点“呢？现实中不就一个车场既是起点又是终点吗？</span>
                          <span slot="body">答：现实中的确是一个车场既是起点又是终点，将这一个车场视作“起点”和“终点”两个车场是为了在构建时间窗约束时不会产生数学上的矛盾。
                      大家可以看下面的数学模型来理解这点，不然可能认为小Qiu在说什么谜语。</span>
                        </ExpandCard>
                        </li>
                      
                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">为什么用弧集来包含可以有的线路，直接在全部点之间两两相连不也能表示可有的线路吗？这样我就只需要定义点集就行了。</span>
                      <span slot="body">答：不是小Qiu故作玄虚 @_@，是现实中两个点之间不一定存在直达的线路，为了能等价描述这种点与点之间稀疏的连接关系，就需要额外定义个弧集。当然，如果你考虑的问题里，每个点
                      都有直达其他所有点的路径，你也可以将全部点两两连接，来表示可有的线路。</span>
                        </ExpandCard>
                      </li>                   
                      
                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">可以细说一下弧集\(E\)里，和起点车场和终点车场相关的东西吗？</span>
                          <span slot="body">答：对于起点车场，它除了能指向客户，还能无条件指向终点车场，但它<span class="blue-bold">不被任何客户、终点车场所指向</span>; 
                        终点车场截然相反，它除了能<span class="blue-bold">被</span>客户所指向，还能无条件<span class="blue-bold">被</span>起点车场所指向，但<span class="blue-bold">不能指向任何客户和起点车场</span>。这是“起点”和“终点”二字的应有之义，可以用下图来
                        辅助理解。
                          <VerticalCenterDiv width="1000px" height="282px" margin_left="-500px" overflow="hidden">
                            <v-card-title  class="justify-center caption pink"> 
                              图：简化的连接图 (绘图工具: <a href="http://dreampuf.github.io/GraphvizOnline" class="neat white--text"> Graphviz</a>)
                            </v-card-title>
                            <img src="../assets/vrp-edges.svg" alt="弧集简化示意图" width="1000px" height="230px">
                          </VerticalCenterDiv>
                          </span>
                        </ExpandCard>
                      </li>
                      
                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">车辆的一次运输是指服务一个客户，还是指从车场出发，服务完一些客户，又返回车场的一整个环节呢？</span>
                          <span slot="body">答：后者。</span>
                        </ExpandCard>
                        </li>
                      
                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">为什么需要车辆一天运输次数的集合？一辆车一天可能一次都不运，也可能一天运很多次，小Qiu你这样不是要求一辆车必须要运多少次了么？</span>
                          <span slot="body">答：
                            <p class="multi-para">一辆车的确可能一次都不运，也可能运很多次，为了能在数学上描述这种灵活性，我们设定一辆车一天必须运输的次数，且允许其在起点车场和终点车场间直接运输。这样不管车辆在车场外面运多少次，就都可以描述出来了。</p>
                            <p class="multi-para">比如：设定一辆车一天必须运输10次，如果它在车场外面运了7次，那么只需要让该车从起点车场直达终点车场3次即可，以此类推。敏锐的小伙伴会说：“那这样的话，一辆车必须要运的次数要往大的数值设定”，对于这种
                            小伙伴，我只能说英雄所见略同:D</p>
                          </span>
                        </ExpandCard>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <span>关于车辆属性</span>
                    <ul>
                      <li>
                          <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                            <span slot="title">为什么车辆每次回车场后，离再次出发还需要设一个时间间隔呢？</span>
                            <span slot="body">答：现实中，车场要再次出发需要备货、装载等操作，所需时间不可简单忽略。</span>
                          </ExpandCard>
                      </li>
                    </ul>
                  </li>

                  <li>
                    <span>关于运输路径的属性</span>
                    <ul>
                      <li>
                        <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">既然将一个车场同时视作“起点”车场和“终点”车场，那这两个车场之间的运输时长和运输成本怎么定义？</span>
                          <span slot="body">答：都是0,因为这两个车场实际上是具有相同空间位置的，所以在它们间运输起来不需要成本和时间。</span>
                        </ExpandCard>
                      </li>
                    </ul>
                  </li>
                </ul>
              
              </p>
              </div>

            </v-expand-transition>
            
               <v-card-title>

                  <h1 class="text-h5 blue--text">数学模型</h1>
                  <v-btn
                    icon
                    @click="math_show = ! math_show"
                  >
                    <v-icon>{{ math_show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
                  </v-btn>
                </v-card-title>

            <v-expand-transition>
              <div v-show="math_show">
                <v-divider></v-divider>   
                <p class="mt-5 mx-5 multi-para">
                  上面的数学符号只是把解决问题所必需的要素说了出来，即便从没接触过路径规划的人也完全可以做到这一点，但离充分描述车辆运输路径的形状还有一段距离。如果将“解决车辆路径规划”视作炒一盘鸡蛋，
                  那么定义符号只做到了准备好生鸡蛋、油盐、锅碗瓢盆，至于怎么炒还是一个问题，这就是我们接下来要做的事：<span class="blue-bold">规定车辆运输路径的样子</span>。
                </p>
                <p class="mt-5 mx-5 multi-para">
                  为此，我们需要问自己：抛开任何数学，自己能在纸上或脑海中画出一辆车可以有的运输路径么？如果连这点都做不到，那么使用数学、程序语言来描述车辆路径规划更是难上加难。所以小Qiu先挂上一幅玩具版的车辆路径图，
                  配合该图来说车辆路径要满足的最低要求。
                </p>

                <VerticalCenterDiv width="520px" height="320px" margin_left="-260px" overflow="scroll">
                    <v-card-title  class="justify-center caption pink"> 
                      图：简化的车辆路径 (图片来源: <a href="https://docs.abivin.com/docs/abivin-vroute-glossary" class="neat white--text"> abivin</a>)
                    </v-card-title>
                    <img src="../assets/vrp-intro.png" alt="简化的车辆路径" width="509px" height="500px">
                </VerticalCenterDiv>

                <p class="mt-5 mx-5 single-para">
                  大家对上图车辆路径的形状是否感到合理呢？如果是的话，我们就可从该图中总结路径满足的要求（可下拉呈现对应的数学约束）：
                  <ul>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">每个客户必须被服务一次，但不限定哪辆车来服务。</span>
                          <span slot="body">
                            $$
                              \sum_{k\in M, n\in N, j\in E^{-}(j)} x_{ij}^{kn}=1,  \quad \forall  j\in C\\
                            $$
                          </span>
                        </ExpandCard>
                      </li>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">一辆车的路径，必须是从起点车场出发，最后又必须回到终点车场。（现实中，起点和终点车场通常就是同一个车场）</span>
                          <span slot="body">
                            $$
                              \sum_{j\in E^{+}(0^{+})} x_{0^{+}j}^{kn}=\sum_{i\in E^{-}(0^{-})} x_{i0^{-}}^{kn}=1, \quad \forall k\in M, n\in N
                            $$
                          </span>
                        </ExpandCard>
                        </li>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">一辆车访问一个客户后，之后必须从该客户离开。</span>
                          <span slot="body">
                            $$
                            \sum_{i\in E^{-}(p)} x_{ip}^{kn}=\sum_{j\in E^{+}(p)} x_{pj}^{kn}, \quad \forall  p\in C, k\in M, n\in N
                            $$
                          </span>
                        </ExpandCard>
                        </li>
                  </ul>
                </p>

                <p class="mt-5 mx-5 single-para">
                  当然，还有一些对路径的要求不能在图上轻易表示出来，这需要我们额外的总结（可下拉呈现对应的数学约束）：
                  <ul>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">一辆车一次运输中，服务的客户所需要的商品体积不超过车本身的容积。</span>
                          <span slot="body">
                            $$
                              \sum_{(i,j)\in E} q_{j}x_{ij}^{kn} \leq Q_{k}, \quad \forall k\in K, n\in N
                            $$
                          </span>
                      </ExpandCard>
                    </li>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">一辆车开始服务一个客户的时刻，必须在时间窗内。</span>
                          <span slot="body">
                            $$
                            \begin{align*}
                            & a_{i} \leq t_{i}^{kn} \leq b_{i}, &\quad \forall i\in C, k\in K, n\in N\\
                            & t_{i}^{kn}+\tau_{ij}-M(1-x_{ij}^{kn}) \leq t_{j}^{kn}, &\quad \forall (i,j)\in E, k\in K, n\in N\\
                            & M 是一个很大的常数
                            \end{align*}
                            $$
                            <p class="single-para">
                              此处的第二行约束是用于描述时间的变化，如果一辆车从\(i点直达j点(也就是x_{ij}^{kn}=1)\)，那么该约束就变成下式，确实符合时刻\(t_{j}^{kn}\)和\(t_{i}^{kn}\)应有的关系。
                              $$
                                t_{i}^{kn}+\tau_{ij} \leq t_{j}^{kn}
                              $$
                            </p>
                            <p class="single-para">
                              如果一辆车并未从\(i点直达j点(也就是x_{ij}^{kn}=0)\)，那么该约束如下所示。由于\(M\)是一个很大的常数，那么该约束恒成立，从而不会约束时刻\(t_{j}^{kn}\)和\(t_{i}^{kn}\)之间的关系，这也符合应得的结果。
                              $$
                                t_{i}^{kn}+\tau_{ij}-M \leq t_{j}^{kn}
                              $$
                            </p>
                          </span>
                        </ExpandCard></li>
                    <li>
                      <ExpandCard :initial_show="false" title_size="body-1" text_color="blue--text">
                          <span slot="title">一辆车外出服务客户后，要再次出发必须有一段时间的休整。</span>
                          <span slot="body">
                            $$
                            t_{0^{-}}^{kn}+r_{k}(1-x_{0^{+}0^{-}}^{kn}) \leq t_{0^{+}}^{k(n+1)}, \quad \forall j\in C, k\in K, n\in N-\{|N|\}\\
                            $$
                          </span>
                        </ExpandCard></li>
                  </ul>
                </p>

                <p class="mt-5 mx-5 single-para">
                  我们直接将以上6点要求对应的数学约束拼在一起，就可以得到完整的数学模型，如下所示：
                </p>
                <v-card-title class="caption pb-0 justify-center">

                  公式显示：由<a href="https://www.mathjax.org/" class="neat">Mathjax2.7.9</a>提供tex公式的渲染。
                </v-card-title>


                <div style="position:relative;height:410px">
                  <div style="width:700px;position:absolute;left:50%;margin-left:-350px;">
                  $$
                    \begin{align}
                      & Min \sum_{k\in M, n\in N}\sum_{(i,j) \in E} c_{ij} x_{ij}^{kn}\\
                      & \sum_{k\in M, n\in N, j\in E^{-}(j)} x_{ij}^{kn}=1, & \quad \forall  j\in C\\
                      & \sum_{j\in E^{+}(0^{+})} x_{0^{+}j}^{kn}=\sum_{i\in E^{-}(0^{-})} x_{i0^{-}}^{kn}=1, &\quad \forall k\in M, n\in N\\
                      & \sum_{i\in E^{-}(p)} x_{ip}^{kn}=\sum_{j\in E^{+}(p)} x_{pj}^{kn}, &\quad \forall  p\in C, k\in M, n\in N\\
                      & \sum_{(i,j)\in E} q_{j}x_{ij}^{kn} \leq Q_{k}, &\quad \forall k\in K, n\in N\\
                      & a_{i} \leq t_{i}^{kn} \leq b_{i}, &\quad \forall i\in C, k\in K, n\in N\\
                      & t_{i}^{kn}+\tau_{ij}-M(1-x_{ij}^{kn}) \leq t_{j}^{kn}, &\quad \forall (i,j)\in E, k\in K, n\in N\\
                      & t_{0^{-}}^{kn}+r_{k}(1-x_{0^{+}0^{-}}^{kn}) \leq t_{0^{+}}^{k(n+1)}, &\quad \forall j\in C, k\in K, n\in N-\{|N|\}\\
                      & x_{ij}^{kn}\in \{0,1\}, t_{i}^{kn} \geq 0, &\quad \forall i,j\in V, k\in K, n\in N\\
                    \end{align}
                  $$
                  </div>
                </div>

                 <p style="text-indent:2em; line-height:2em" class="mt-5 mx-5">
                  看得到吗

                </p>
              </div>

             
            </v-expand-transition>
          </div>
        </v-expand-transition>
      </v-card>

  </div>
</template>

<script>
import ExpandCard from '../components/ExpandCard.vue'
import VerticalCenterDiv from '../components/VerticalCenterDiv.vue'
  export default {
    components:{
      ExpandCard,
      VerticalCenterDiv
    },

    data: () => ({
      model_show: true,
      notation_show: false,
      math_show:false,
    }),
    methods: {
    },
    created () {
    },
    mounted(){     
      if(!window.MathJax){
        let script = document.createElement('script');
        script.type = 'text/javascript';
        // script.src = 'https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.9/latest.js?config=TeX-MML-AM_CHTML';     
        script.src='http://localhost:8080/MathJax-2.7.9/MathJax.js?config=TeX-MML-AM_CHTML';
        script.async=true;
        script.id="MathJax-script"
        document.head.appendChild(script);     
      }
      let _this=this;
      setTimeout(()=>{
         window.MathJax.Hub.Typeset();
        setTimeout(()=>{
          _this.notation_show=!_this.notation_show;
          _this.math_show=!_this.math_show;
        },1000)
      } ,0);
    },
  }
</script>

<style>
span.blue-bold{
  color: #2196f3;
  font-weight:bold; 
}

p.single-para{
  text-indent:0em; 
  line-height:2em
}

p.multi-para{
  text-indent:2em; 
  line-height:2em
}

div.dark{
  background-color: #1e1e1e;
}

a.neat{
  text-decoration: none;
  border-bottom:1px solid
}

</style>
