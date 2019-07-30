1. 写 React / Vue 项目时为什么要在列表组件中写key, 其作用是什么？
    基于没有 key 的情况下 diff 速度会更快
        没有绑定 key 的情况下,并且在遍历模版简单的情况下,会导致虚拟旧节点对比速度更快,节点也会发生复用。而这种复用是就地复用,一种鸭子辩型的复用。
        ```
        <div id="app">
            <div v-for="i in dataList">{{ i }}</div>
        </div>

        var vm = new Vue({
            el: '#app',
            data: {
                dataList: [1, 2, 3]
            }
        })
        ```
        
