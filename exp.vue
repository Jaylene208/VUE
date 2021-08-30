<style lang="less">

</style>
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<template>
    <div>
        <!-- v-bind 声明渲染 -->
        <div>
            {{test5}}
        </div>
        <div id="test2">
            <span v-bind:title="test">鼠标悬停</span>
        </div>
        
        <!-- v-if -->
        <div id="test3">
            <p id="see" v-if="seen" >can see</p>
            <button @click="seenFunction">点击不可见</button>
        </div>

        <!-- v-for 条件渲染 -->
        <!-- of替代in作为分隔符 -->
        <div id="test4">
            <ol>
                <li v-bind:key="item.text" v-for="item in list">
                    {{item.text}}
                </li>
            </ol>
        </div>

        <!-- 遍历对象 -->
        <div>
            <ul>
                <li v-for="value in styleObj" v-bind:key="value"> {{value}}</li>
            </ul>
        </div>
        <br>
        <!-- 提供第二个的参数为 property 名称 -->
        <div v-for="(value,name) in styleObj" v-bind:key="value">
            {{name}}:{{value}}
        </div>

        <!-- v-on(简写@) 监听事件 处理用户输入 -->
        <div>
            <p>{{test5}}</p>
            <p>reversed message: "{{ reversedMessage }}"</p>
            <button @click="reFunction">反转消息</button>
            <!-- 或者：<p>reverse message:"{{reFunction()}}"</p> -->
        </div>

        <!-- v-model 双向绑定 -->
        <div>
            <p>{{test5}}</p>
            <input v-model="test5">
        </div>

        <!-- 数据方法 -->
        <div @click="watchFunction">
            {{a}}
        </div>

        <!-- 生命周期 -->
        <div>
            {{a}}
        </div>

        <!-- v-once 只渲染元素和组件一次 -->
        <div>
            <span v-once>这里不会改变: {{ test5 }}</span>
        </div>

        <!-- 显示 innerHTML -->
        <div>
            <p>显示innerHtml：{{inner}}</p>
            <span v-html="inner"></span>
        </div>

        <!-- v-bind 动态绑定id -->
        <div v-bind:id="dynamicID" @click="idFunction">
            test
        </div>

        <!-- JavaScript 表达式 -->
        <div>
            {{a+1}}
            {{1==3 ? 'yes' : 'no'}}
        </div>

        <!-- v-bind绑定 href -->
        <div>
            <a v-bind:href="url">...</a>
        </div>

        <!-- 动态参数 -->
        <div>
            <a v-on:[doSome]="doSome">111</a>
        </div>

        <!-- 修饰符 -->
        <!-- 执行完2就停止 -->
        <div>
            <button @click="click1">
                <div @click.stop="click2">click</div>
            </button>
        </div>

        <!-- 缩写 -->
        <!-- v-bind : -->
        <!-- v-on @ -->
        <div>
            <a :href="url">url</a> <br>
            <a @click="doSome">click</a>
        </div>

        <!-- 计算属性 -->
        <div id="example">
            {{ test5.split('').reverse().join('') }}
        </div>
        <br>
        <!-- 计算属性 -->
        <div id="demo">
            {{ fullName }}
        </div>

        <!-- 侦听器 -->
        <div>
            <p>ask a question</p>
            <input v-model="question">
            <p>{{answer}}</p>
        </div>
        
        <!-- class绑定 -->
        <!-- class的存在取决于isActive的真值 -->
        <div v-bind:class="{active:isActive}">1</div>
        <!-- 当 isActive 或者 hasError 变化时，class 列表将相应地更新 -->
        <div class="static" v-bind:class="{ active:isActive,'text-danger':hasError }">2</div>
        <div v-bind:class="[activeClass,errorClass]">[1,2]</div>
        <div v-bind:class="[{active:isActive},errorClass]"></div>

        <!-- 绑定内联样式 -->
        <div v-bind:style="{ color:activeColor,fontSize:fontSize+'px' }"></div>

        <!-- 直接绑定到一个样式对象 -->
        <div v-bind:style="styleObj"></div>

        <!-- v-if:条件性地渲染一块内容，只在指令的表达式返回truthy值的时候被渲染 -->
        <h1 v-if="awesome">Vue is awesome</h1>

        <!-- v-else-if -->
        <div v-if="type === 'A'"> A </div>
        <div v-else-if="type === 'B'"> B </div>
        <div v-else-if="type === 'C'"> C </div>
        <div v-else> Not A/B/C </div>

        <!-- key管理可复用的元素 -->
        <div v-if="loginType">
            <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
        </div>

        <div v-else>
            <label>Email</label>
            <input placeholder="Enter your email"  key="email-input">
        </div>
        <button @click="toggleFunction">toggle login type</button>

        <!-- v-show:简单地切换元素的 CSS property display -->
        <h1 v-show="seen">Hello!</h1>

        <!-- 创建计算属性，返回数组过滤 -->
        <div>
            <li v-for="n in evenNumbers" v-bind:key="n"></li>
        </div>

        <!-- 计算属性不适用 -->
        <ul v-for="num in sets" v-bind:key="num" >
            <li v-for="n in even(num)" v-bind:key="n">{{ n }}</li>
        </ul>

        <div>
            <span v-for="n in 10" v-bind:key="n">{{n}}</span>
        </div>

        <!-- 事件处理：监听事件 -->
        <div>
            <button v-on:click="counter += 1">Add 1</button>
            <p>clicked {{counter}} times. </p>
        </div>
    </div>
</template>
<script>


import { color, number } from 'echarts';
    export default {  
        data () {
            return {
                test:'time:'+new Date().toLocaleString(),
                seen:true,
                sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]],
                numbers: [ 1, 2, 3, 4, 5 ],
                list:[
                    {text:'文本1'},
                    {text:'文本2'}
                ],
                counter:0,
                test5: 'hello',
                a:1,
                inner:'<span style="color:red">this is red</span>',
                dynamicID:'id1',
                url:"https://www.baidu.com/",
                firstName: 'Foo',
                lastName: 'Bar',
                question:'',
                answer:'please ask a question!',
                isActive:true,
                hasError:false,
                activeClass:'active',
                errorClass:'text-danger',
                activeColor:'red',
                fontSize:30,
                styleObj:{
                    color:'yellow',
                    fontSize:'18px'
                },
                awesome:true,
                type:'A',
                loginType:true,
            }

        },

        /**
         * 生命周期钩子
         * created：实例被创建之后执行代码
         */
        created:function(){
            this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
            console.log('a is: ' + this.a)
        },

        /**
         * 计算属性
         */
        computed: {
            reversedMessage:function(){         //反转信息
                return this.test5.split('').reverse().join('')
            },
            fullName:{
                //getter
                get: function(){
                    return this.firstName + ' ' + this.lastName
                },
                //setter
                set:function(newValue){
                    var names = newValue.split('')
                    this.firstName = names[0]
                    this.lastName = names[names.length - 1]
                }
            },
            evenNumbers:function(){
                return this.numbers.filter(function(number){
                    return number % 2 == 0
                })
            },
            
        },
        
        /**
         * 监听事件
         */
        watch:{
            a:function(newVal,oldVal){
                console.log(newVal,oldVal);
            },
            question:function(newQues,oldQues){
                this.answer = 'Waiting...'
                this.debouncedGetAnswer()
            }
        },

        methods: {
            seenFunction:function(){
                document.getElementById('see').style.display = 'none';
            },
            reFunction:function(){
                this.test5 = this.test5.split('').reverse().join('');
            },
            watchFunction:function(){
                this.a = 3;
            },
            idFunction:function(){
                console.log(this.dynamicID);
            },
            doSome:function(){
                console.log(0);
            },
            click1:function(){
                console.log(1);
            },
            click2:function(){
                console.log(2);
            },
            // 侦听器 ？
            getAnswer: function () {
            if (this.question.indexOf('?') === -1) {
                this.answer = 'Questions usually contain a question mark. ;-)'
                return
            }
            this.answer = 'Thinking...'
            var vm = this
            axios.get('https://yesno.wtf/api')
                .then(function (response) {
                vm.answer = _.capitalize(response.data.answer)
                })
                .catch(function (error) {
                vm.answer = 'Error! Could not reach the API. ' + error
                })
            },
            /**
             * 切换输入类型
             */
            toggleFunction:function(){
                this.loginType = !this.loginType;
            },
            /***
             * 方法过滤数组
             */
            even: function (numbers) {
                return numbers.filter(function (number) {
                return number % 2 === 0
                })
            }

        }, 

    }   
    
</script>