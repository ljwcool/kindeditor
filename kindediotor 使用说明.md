## kindediotor 使用说明

#### 不管是vuecli2及以上，使用的是npm安装依赖

参考链接
1.[kindeditor官方文档](http://kindeditor.net/doc.php)

[学习大佬的源码地址](https://github.com/l-x-f/vue-use-kindeditor)    

[我的源码地址](https://github.com/ljwcool/kindeditor.git)    

```
https://github.com/ljwcool/kindeditor.git 
```

https://blog.csdn.net/qq_39953537/article/details/100043354?depth_1-  vue后台管理系统富文本组件（二）kindeditor



https://blog.csdn.net/qq_39953537/article/details/100516072?depth_1-    vue后台管理系统富文本组件（三）qull  需要时间看

https://blog.csdn.net/qq_39953537/article/details/100041453   vue后台管理系统富文本组件（一）tinymce 需要时间看

*  依赖说明:axios网络请求，使用饿了么ui ,vue 和kindeditor 其它样式依赖就不用说了 安装kindeditor使用-D安装，开发和运行都要

  ```
  {
      "axios": "^0.18.0",
      "element-ui": "2.11.1",  
      "vue": "^2.6.10",
      "kindeditor": "^4.1.10",
   }
  ```

  

* 新建项目：vuecli2 =>vue init webpack  项目名 

  ​					vuecli3以上=》vue created  项目名    自己选择要装的扩展

*  水平有限+项目急没有认真去看配置文件，引用大佬写好的组件复制

 组件封装结构：可放到 components里共用 

里面的配置文件夹config:里面认真看，在里面改东西

![image-20201104101933554](.\kindediotor 使用说明\image-20201104101933554.png)

* 在需要引用的地方

  ```js
  import KindEditor from "@/components/Kindeditor";
  <Kind-editor
    ref="kindeditor"
    :html="html" //初始数据
    @input="getContent" //获取改变的值
    @count="conuts" //为了计数文字用，自己查看API，在组件里写方法
     v-model="ruleForm.content"
     ></Kind-editor>
  ```

  

* 效果

![image-20201104102342792](.\kindediotor 使用说明\image-20201104102342792.png)

* 记主要出现的问题

  * 1、上面工具类的选项配置：在config/items.js里面配置-》配置自己去找文档看

  * 2、在组件的index.vue里面引用==>如果出现报错

    <font color='red'>Refused to apply style from 'http://localhost:8001/themes/default/default.css' because its MIME type ('text/html') is not a supported stylesheet MIME type, and strict MIME checking is enabled.</font>

    ![image-20201104102800698](.\kindediotor 使用说明\image-20201104102800698.png)

    出现问题原因：路径问题（新建新的项目不报错，引用到旧项目就报错，能力有限没时间找，以下介绍目前粗暴的解决方法）

    看下面的引用代码=>明显是在安装依赖node_modules里，那么路径出错那就去改路径（因为不熟找的过程一言难尽：使用<font color='red'>default.cs</font>s关键字搜索看它引用的地方，然后再<font color='red'>kindeditor-all-min.js</font>里找到一个关键字<font color='red'>themesPath</font>，然后找到配置文件<font color='red'>otherConfig.js</font>有这个，所以在那边改路径，2个方案，<font color='red'>一种用依赖</font>    <font color='red'> 另一种</font>把依赖里的重新复制一份到static里面，<font color='red'>推荐第二种</font>，后面自己想怎么玩就怎么玩），

  ```js
  //引用文件的
  
  import "kindeditor/themes/default/default.css"; //看解决方案可不写
   import "kindeditor/kindeditor-all-min.js"; //得写 可使用独立出来的文件，路径要改
   import "kindeditor/lang/zh-CN.js"; //得写 可使用独立出来的文件，路径要改
  ```

  ```	js
  kindeditor组件里引用的
   themesPath: {
      type: String,
      default: '/node_modules/kindeditor/themes/'   //使用依赖的方案
      // default: '/static/kin/themes/' //自己把配置文件独立出来 把依赖里的文件复制一份放到static里
    },
  
  ```

  ![image-20201104103659788](.\kindediotor 使用说明\image-20201104103659788.png)

  * 3、表情包加载失败，这个就是编辑器的属性：pluginsPath（不要问我为什么在知道，我是看着报错去解决的）:

    在otherConfig.js改路径，并且把表情包拷贝到static文件夹下![image-20201104104255065](.\kindediotor 使用说明\image-20201104104255065.png)

    ```
    pluginsPath: {
        type: String,
        default: '/static/'
      },
    ```

    

  * 4、里面的显示字数等等，自己去看api有很多，可以用监听的父子传参也可以调用子组件方法，看你自己，大佬已经做好很多方法，可以根据自己去改造

*  如果是旧项目推荐旧项目，新的项目看新的，这边都是vue-cli2创建的项目,, 3以上的区别就是public和static的区别，反正不需要你改webpack配置，