# blog-manage
这是一个基于vue2.x+iview ui+express（node）的后台管理系统

### 技术要求：
 - 1.vue2.x+iview ui框架+node.js+mongodb
### 实现的效果
 - 1.用户模块：实现了用户的添加、修改、删除、分页
 - 2.日志模块：实现了日志的添加、修改、删除、分页、模糊查询
 - 3.分类模块：运用回调函数实现了无限分类、添加节点、修改节点、删除节点等操作
 - 4.评论模块：实现了评论的添加、修改、删除、分页、模糊查询
 - 5.上传模块：实现了各种格式图片的上传、展示、大图显示、删除
 - 6.图标模块：引入了echarts
 
### 遇到的问题
 - 1.图片上传时，获取不到图片的路径（解决：执行upload后将req打印出来，找到了路径）
 - 2.路径找到后，返回什么样的数据成为了一个问题（解决：最后决定以json的格式将图片的地址和id一块儿返回）
 - 3.有了路径，要通过本地服务器获取到图片，并且进行展示
 
### 网页效果：
![请输入图片描述][2]

  [2]: http://www.maodan.online/blog-image/11.bmp
![请输入图片描述][3]

  [3]: http://www.maodan.online/blog-image/8.bmp
  
![请输入图片描述][4]

  [4]: http://www.maodan.online/blog-image/9.bmp
![请输入图片描述][5]

  [5]: http://www.maodan.online/blog-image/6.bmp
![请输入图片描述][6]

[6]: http://www.maodan.online/blog-image/6.bmp



