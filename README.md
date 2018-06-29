关于vue
1、安装node.js
Ps:vue项目通过webpack工具来构建，而webpack的命令执行依赖于node.js的环境，所以要先安装node.js,判断node版本的命令：node –v
安装cnpm淘宝镜像，依赖包在国内，提高下载速度，安装cnpm的命令
npm install -g cnpm --registry=https://registry.npm.taobao.org
2、安装vue-cli 脚手架工具，用来快速搭建大型单页应用
cnpm install -g vue-cli
检查是否安装成功的命令：直接输入vue即可
3、开始搭建名为vue-demo的项目
在创建项目的文件夹下打开命令行窗口，利用脚手架快速搭建项目
搭建项目的命令：vue init webpack vue-demo（模板使用的webpack，项目名是vue-demo）

4、创建完项目时候，使用以下命令行进行启动：
先进入项目：cd  vue-demo
再运行项目：cnpm run dev
5、手动下载自己需要的依赖包
cnpm install element-ui --save
ps:在项目文件中打开package.json中的"dependencies"，确认是否下载element-ui的依赖包

6、下载的依赖包要在项目中的srcmain.js中引入
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
7、vue项目开始编译：
App.vue里面是单页面公共部分
Components里面是对应的子页面
Index.js里面是配置路由
Main.js里面是引入依赖包
8、<router-view/>将其放在app.vue页面中，想要渲染子页面的位置上，用来渲染子页面的内容
9、<router-link/>将其放在app.vue页面中，用来跳转到即将渲染的子页面，to跟上子页面（注：直接写子页面的名称，不带.vue的后缀）
10、配置路由：在index.js中配置路由
1）先引入子页面：import first from ‘@/components/first’（此为页面路径）
2)再配置路径：{path:’/first’,（与route-link to的层级保持一致）
                name:’first’,
                component:first}
注：默认的子页面，需要重新配置一个路由，路径为‘/’即可

11、v-model和v-bind和v-on的联系以及区别？
v-bind用来绑定元素的属性，比如class、src、href等属性，用来在实例中动态的改变元素的属性
例：<span class=”className”></span>此时class只是一个固定的class名称
<span v-bind:class=”className”></span>此时的class却是可以通过实例中的data中的className来动态的变化new Vue({el:”#app”,data:{className:”sss”}})
v-model主要用于表单的双向数据绑定，它实现的是当data.message发生变化的时候，视图中的显示也会发生变化，同样当视图发生变化的时候，data中也会发生变化，即在有地方发生修改的元素才会使用v-model，比如input/textarea/select
<label><input v-model=”message”/>{{message}}</label>
v-bind是来响应更新html的特性
v-on是来监听DOM的事件
12、{{插值表达式}}和v-bind的区别？
{{}}会将数据解析为纯文本的形式，而不能使用于Html形式中,而v-bind则可以将数据解析为html形式，即将数据处理为属性来使用，如下：
<span v-bind=”message”>{{message}}</span>
13、v-if指令和v-else的搭配使用
v-if用来判断DOM元素是否要渲染，比如在用户登陆的时候，如果用户未登陆，就提示请用户先登陆，则不必渲染用户的信息，而用户登陆成功再渲染用户的信息，此时搭配v-else使用。
<div id="app">
        <span v-if="isLogin">请先登录，再查看信息</span>
        <span v-else>{{message}}已登录，请注意查收信息</span>
    </div>
var app=new Vue({
    el:"#app",
    data:{
        message:"石闪闪",
        isLogin:false
    }
})
打印出来的结果为：石闪闪已登录，请注意查收信息
14、v-if和v-show的联系和区别？
v-if是判断是否加载DOM元素，这个是根本就没有渲染DOM元素。
v-show是DOM元素已经加载过了，但是css的display属性没有显示，导致这个元素没有在页面上显示。
15、v-for解决模板循环问题
循环渲染数据，你需要循环哪个标签，就将v-for写到哪个上面即可。
16、vue项目中文件夹下的index.js文件，设置autoOPenBrowser属性为true,即可自动打开浏览器。
17、在element组件中，对表格的操作,比如设置表头文字居中的样式：
header-cell-style  表头单元格的 style 的回调方法，也可以使用一个固定的 Object 为所有表头单元格设置一样的 Style，即在当前标签里面以对象的形式去设置该属性
:header-cell-style="{'text-align':'center'}"
18、由子页面跳转到上一级页面，点击按钮触发事件，事件里面配置路由：
backPrev:function(){
      this.$router.push({path:'/articleContent'})
    }
19、命令行运行 cnpm run build，即可将项目打包成功，将生成的dest文件夹发给其他人，即可在其他电脑上浏览项目页面，但是页面一旦发生修改，就要重新打包，并重新发送给别人，其中可能遇到的问题：dest文件里面的index页面打不开，解决方法，在config下面的index.js找到build里面assetsPublicPath改成’./’




20、关于element-ui动态渲染表格的表头及数据
<el-table :data="data_list">
<el-table-column  :label="date" v-for="(date, key) in header">
        <template scope="scope">
            {{data_list[scope.$index][key]}}
          </template>
    </el-table-column>

 </el-table>
//返回数据格式：
 data() {
            return {
                header:["A","B", "C"],
                data_list:[
                    [1, 2, 3],
                    [4,5,6]
                ]
            }
        }
21、关于element-ui渲染表格数据
问题：从后台返回的数据中的某一项，不需要在当前表格渲染出来，但是还需要此属性作为参数传给下个接口
解决办法：
1）该属性为简单属性，比如id,可以将其绑定到某列的label属性上，在调取下个接口时，通过label去获取该属性。
2）
3）该属性为复杂属性，比如文章详情，因为文字较多，则不适合绑定在label属性，此时可以仍为此属性开辟一列，让其渲染在表格中，但是设置该列属性v-if=”false”,让其在前端页面的表格中不显示，但是通过scope.row获取表格的某行数据，是可以获取包括该属性的一整行的数据。

22、关于vue反向代理的使用：（即配置代理，替换接口域名）
在config中的index.js中，设置proxyTable属性，设置完需要重启服务器

23、关于vue中设置请求头，设置向后台传递参数的格式：
Vue中默认的请求头的参数格式是：application/json,将其改为application/x-www-form-urlencoded
Var param=new URLSearchParams();
Param.append(‘key1’,value1);
Param.append(‘key2’,value2);
Axios({
    Method:’post’,
    url:’...’,
    headers:{‘Content-Type’:’application/x-www-form-urlencoded’},
    data:param
}).then(res=>{
    Console.log(res)
})

24、关于vue设置css样式的问题，如果设置某一样式不起作用，并且该样式不是公共样式，只是针对某一组件里面的元素，可以尝试再搞一个<style></style>，标签里面不加scoped
25、关于vue+element过滤更改表格数据：
问题：表格某项后台返回的数据是0,1，而在前端对应显示的为上架和下架，并且操作中可以修改上下架的状态，此时可以使用过滤器（实现效果图如下）

解决：1）状态项，将数字转换为文字显示，使用三元表达式（或者过滤器）
注：根据后台返回数据中获取status属性，如果对应属性值为0就显示上架，否则下架
<el-table-column prop="status" label="状态">
          <template slot-scope="scope">
            <span>{{tableData[scope.$index]['status']==0?'上架':'下架'}}</span>
          </template>
        </el-table-column>
2）在操作项设置过滤器，通过状态属性值来定义操作项的显示值，如果接收的数据为0，就显示’下架’,1就显示为’上架’
<el-button @click="handleClick(scope.$index,scope.row)" type="text" size="small">{{scope.row.status | formatStatus}}</el-button>
过滤器的设置：
Import Vue from ‘vue’(注意引入)
filters: {
      formatStatus: function(val) {
        return val == 0 ? '下架' : val == 1 ? '上架':val == '上架' ?'下架':val == '下架' ?'上架':未知
      },
    },
4）在点击操作的时候，动态的修改状态值以及操作项
5）handleClick(index,row) {
6）    var status=row.status;
7）    if(status==0){
8）    row.status=1;//*修改状态值，上面的代码就会动态的修改状态显示和操作显示
9）          var params = new URLSearchParams();
10）      params.append('contentId', row.contentId)
11）      params.append('status', row.status)
12）      axios({
13）        method: 'post',
14）        url: '/Ajax/admin/bops/content/status',
15）        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
16）         data: params
17）           })
18）       .then(res => {
19）          console.log('成功')
20）         });
21）}else if(status==1||'下架'){
22）row.status=0;//*修改状态值，上面的代码就会动态的修改状态显示和操作显示
23）          var params = new URLSearchParams();
24）          params.append('contentId', row.contentId)
25）          params.append('status', row.status)
26）          axios({
27）            method: 'post',
28）            url: '/Ajax/admin/bops/content/status',
29）            headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
30）            data: params
31）        })
32）      .then(res=>{
33）        console.log('成功')
34）      });
35）       }
26、关于vue+element分页功能的实现
<el-pagination background layout="prev, pager, next" :page-size="pageSize" :current-page="currentPage" :page-count="pageCount" @current-change="handlePage" >
</el-pagination>


先初始化pageSize  currentPage  pageCount
current-change是当页码发生变化时触发的事件，该事件里直接传递有currentPage(当前页)，在事件里接收该参数，并将其作为参数传递给后台，然后直接渲染数据即可。
handlePage:function(currentPage){
        this.currentPage=currentPage;
        //console.log(this.currentPage)
        var params = new URLSearchParams();
        params.append('pageNo', this.currentPage)
        params.append('pageSize', 10)
        axios({
          method:'post',
          url:'/Ajax/admin/bops/data/behavior/',
          headers:{'Content-Type': 'application/x-www-form-urlencoded'},
          data:params
        })
      .then(res=>{
        //console.log(res)
        this.tableData1=res.data.result;
      });
27、使用axios调取接口的实现：
1)先在项目文件下打开命令行窗口，下载axios依赖包
Cnpm install axios –save
2)在调取接口的页面的script中引入axios
Import axios from ‘axios’
3)直接使用axios调取接口
axios.post('/Ajax/admin/bops/data/summary',{})
      .then(res=>{
        this.totalCount=res.data.result;
      });
28、vue项目中报有scope的警告问题，解决办法：
将<template scope="scope">//此处需要修改为slot-scope=”scope”
        {{tableData2[scope.$index][0]}}
  </template>
修改之后还报错就重新下载一个依赖
29、vue+element动态渲染数据的几种场景：
1）渲染表格数据
<el-table :data="tableData"  border style="width: 100% ;text-align:center" :header-cell-style="{'text-align':'center'}">
       <el-table-column prop="downloadNumber" label="下载数">
       </el-table-column>
       <el-table-column prop="registerNumber" label="注册">
       </el-table-column>
</el-table>
a.表格总数据设置:data=” tableData”,在下面进行初始化，它的数据格式要与后台返回的数据格式保持统一（因后台返回的表格总数据是放在一个数组里面，数组里是一个个的对象，因此要将tableData定义为数组
b.表格每列的属性设置prop，要将其属性值和后台返回数据中的字节段保持一致（即后台返回的数组中对象对应的字节段）
c.在ajax请求的时候，将后台总数据赋给tableData即可，即（this. tableData=res.data.result）
2）渲染表单数据
 <div class="channel">
        <el-button type="text" disabled>频道</el-button>
        <el-radio-group v-model="radio1">
          <el-radio-button v-bind:label="channel.channelId" v-for= "(channel,index) in totalChannel" :key = 'index'>{{channel.channelName}}</el-radio-button>
        </el-radio-group>
      </div>
3）渲染二维数组的数据
<el-table-column  v-bind:label="data" v-for="(data,index) in header" :key='index' >
      <template slot-scope="scope">
         {tableData2[scope.$index][index]}}
      </template>
</el-table-column>



4）渲染对象数组
数据格式是数组，里面是一个个对象，若想遍历对象里面的每个属性已经属性值，需要先初始化一个对象，然后在数组里面循环对象，再调用里面的属性值。（下面的obj即为初始化空对象）



30、关于vue中发送ajax请求，报open 的错误：
调取接口的时候，接口链接要带上http://即可
31、关于element中修改默认样式：
Element-ui组件中的样式要修改，1）如果style中带有scoped然而样式不起作用，就另再写一个style来设置样式; 2)对于element中封装的ui标签在css中要以类名的形式去使用,比如折叠面板（.el-collapse-item）; 3)要设置element-ui里面封装的div样式，就要打开控制台找到对应的类名去设置，比如折叠面板的头部样式
（. el-collapse-item  .el-collapse_header）
(下面为设置提示消息的显示位置)
https://blog.csdn.net/qq_32340877/article/details/79135686


32、关于vue项目打包生成的dist文件，需要启动nginx或者xampp服务器，这样才能获取到后台接口的数据。
Xampp:需要将dist文件放到htdocs文件夹里面，然后运行localhost
Nginx需要配置conf中的test.conf，
server {
    listen       8080;
    server_name  localhost;
    #charset koi8-r;
    #access_log  logs/host.access.log  main;
    location / {
        root   E:\dist;
        index  index.html index.htm;
    }
}
这个文件如果使用记事本打开进行编辑会报错，这是因为记事本编辑过的文本会自动加入一个带有BOM头 的文件，解决方法，需要使用其他的编译器打开编辑，并且将utf-8 with BOM 改为utf-8的编码形式保存即可，然后运行localhost

33、关于element+vue在main.js中引入index.css，
vue1.0版本路径是import ‘element-ui/lib/theme-default/index.css’
vue2.0版本路径是import ‘element-ui/lib/theme-chalk/index.css’
34、关于公共头部组件化：
1）创建headTop.vue的页面，将公共的头部提取在本页面
2）将此组件引入在其他页面中
    作为标签放在页面的头部位置<head-top></head-top>
    引入该页面所在的路径 import headTop from ‘../components/headTop’
    在页面的组件中声明 components:{headTop}
注：最好在<view-router>的位置引入，只引入一次，在每个页面都可以使用,如果在每个页面分别引用的话，子页面中会找不到该组件

35、ele+vue设置日期时间选择器，并设置当前时间之前不可选择：
参考文章的链接：https://www.cnblogs.com/xjcjcsy/p/7977966.html
<div class="block">
    <span class="demonstration">默认</span>
    <el-date-picker
      v-model="value1"
      type="datetime"
      placeholder="选择日期时间" :picker-options="pickerOptions1">
    </el-date-picker>
  </div>
export default {
    data() {
      return {
value1: '',
         pickerOptions1: {
         disabledDate(time) {
            return time.getTime() < Date.now() - 8.64e7;
          }
          }
      };
    }
  };
36、ele+vue图片上传的问题：
文章参考：https://zhuanlan.zhihu.com/p/25714998
https://segmentfault.com/a/1190000014137083

关于vue-router路由的配置：
一级路由的配置：
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20  import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import Hello from '@/components/Hello'  //引入根目录下的Hello.vue组件
import Hi from '@/components/Hi'
 
Vue.use(Router)  //Vue全局使用Router
 
export default new Router({
  routes: [              //配置路由，这里是个数组
    {                    //每一个链接都是一个对象
      path: '/',         //链接路径
      name: 'Hello',     //路由名称，
      component: Hello   //对应的组件模板
    },{
      path:'/hi',
      name:'Hi',
      component:Hi
    }
  ]
})
子路由的配置：
import Vue from 'vue'  
import Router from 'vue-router'  
import Hello from '@/components/Hello'  
import Hi from '@/components/Hi'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'
 
Vue.use(Router)
 
export default new Router({
  routes: [            
    {                    
      path: '/',        
      name: 'Hello',    
      component: Hello  
    },{
      path:'/hi',
      component:Hi,
      children:[
        {path:'/',component:Hi},
        {path:'hi1',component:Hi1},
        {path:'hi2',component:Hi2},
      ]
    }
  ]
})
Vue学习的博客：
Vue使用element-ui动态渲染表头及数据：
http://blog.csdn.net/Ricky110/article/details/78924688
关于vue的学习详细的讲解：***
http://blog.csdn.net/fungleo/article/details/77584701
关于vue的优秀案例：***
https://github.com/bailicangdu/vue2-elm
https://github.com/lin-xin/vue-manage-system
关于vue的另版本讲解：
http://www.jqhtml.com/9032.html
关于vue的表格分页功能：
http://www.cnblogs.com/luozhihao/p/5516065.html
关于vue封装的组件，比如富文本编译器：
https://segmentfault.com/a/1190000009275424
技术胖的博客：
http://jspang.com/2017/02/23/vue2_01/
vue学习第三季：
http://jspang.com/2017/03/26/vue3/3/
vue学习第四季：
http://jspang.com/2017/04/09/vue2_4/
关于v-bind/v-model/v-on:
http://www.tangshuang.net/3507.html
关于路由的详细介绍：
https://router.vuejs.org/zh-cn/
vue的官方网址：
http://vuejs.org/








Excel文章的导出：

outputFile() {//导出
        let arr = [];
        for (let i in this.selectionObj) {
          for (let j = 0; j < this.selectionObj[i].length; j++) {
            arr.push(this.selectionObj[i][j].id);
          }
        }
        let url = 'admin/goods/exportGoods?token=' + localStorage.getItem('token');
//        let url = 'admin/goods/exportGoods?token=' + localStorage.getItem('token');
        let str = '';
        if (arr.length === 0) {
          if (this.searchType === 1) {
            str = '&catId=' + JSON.stringify(this.easyForm.catId) + '&type=' + this.easyForm.type;
          } else {
            str = '&keyword=' + this.form.keyword +
              '&catId=' + JSON.stringify(this.form.catId) +
              '&brandId=' + this.form.brandId +
              '&upLimit=' + this.form.upLimit +
              '&downLimit=' + this.form.downLimit +
              '&zero=' + this.form.zero +
              '&type' + this.form.type +
              '&source' + this.form.source;
          }
          this.$confirm('此操作将导出全部数据, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
          }).then(() => {
            location.href = url + str;
            this.$message({
              type: 'success',
              message: '导出成功!'
            });
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消导出'
            });
          });

        } else {
          this.$confirm('此操作将导出已选数据, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning',
          }).then(() => {
            location.href = url + '&skuList=' + JSON.stringify(arr);
            this.$message({
              type: 'success',
              message: '导出成功!'
            });
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消导出'
            });
          });

        }
      },