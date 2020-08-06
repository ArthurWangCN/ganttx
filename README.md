# gantt

#### 介绍
a dhtmx-gantt component based on vue

#### 软件架构
DHTMLX-GANTT + Vue.js


#### 安装教程&使用说明

1.  假设你已经有了一个vue项目
2.  安装依赖dhtmx-gantt：

       `yarn add dhtmlx-gantt --save (for yarn)`  或
       
       `npm install dhtmlx-gantt --save (for npm)`
3.  拷贝 Ganttx.vue
4.  在父组件中引入 Ganttx.vue，并且注册 gantt 组件
    ```html
    <div class="wrapper">
        <div class="container">
            <ganttx class="left-container" :tasks="tasks"></ganttx>
        </div>
    </div>
    ```
5. 注意给.wrapper一个高度(或者使用`gantt.config.autosize = true`来自适应)：
    ```css
    .wrapper{
        height: 500px;
    }
    .container {
      height: 100%;
      width: 100%;
    }
    .left-container {
      overflow: hidden;
      position: relative;
      height: 100%;
    }
    ```
6. 父组件中定义需要传入Ganttx.vue的数据：
    ```js
    data() {
        return {
          tasks: {
            data: [
              {id: "5557a99a-a34e-471b-8a42-98bc47db0712", text: 'Task #', start_date: '2020-01-17', duration: 23, progress: 0.6},
              {id: 2, text: 'Task #2', start_date: '2020-01-20', duration: 30, progress: 0.4, },
              // {id: 2, text: 'Task #2', start_date: '2020-01-20', duration: 30, progress: 0.4, 'color': '#fcca02' },
              {id: 3, text: 'Task #2', start_date: '2020-01-20', duration: 3, progress: 0.4, parent: "5557a99a-a34e-471b-8a42-98bc47db0712", type: 2},
            ],
          },
        }
      },
    ```
7. 具体配置见 `Ganttx.vue`

