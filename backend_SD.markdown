###技术选型
   - **Flask：**
   
    Flask是一个使用 Python 编写的轻量级 Web 应用框架，其 WSGI 工具箱采用 Werkzeug ，模板引擎则使用 Jinja2。Flask的核心十分简单，具备很强的拓展能力，没有默认使用的数据库。
    
   - **redis：**

    Redis是一个高性能的key-value数据库，有着复杂的数据结构并且提供对它们的原子性操作，支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
    
###模块划分

由于使用的是Flask开发框架，我们可以直接将所有业务逻辑操作封装在同一个app.py文件中，并根据API设计文档进行结构划分


    app.py
    ├─ Restaurant
        ├─/restaurant/recommendation
        ├─/restaurant/session
        ├─/restaurant/category
        ├─/restaurant/category/
        ├─/restaurant/category/<int:categoryId>
        ├─/restaurant/dish/<int:dishId>
        ├─/restaurant/getdish/<int:dish_id>
        ├─/restaurant/order
    ├─ Customer
        ├─/restaurant/customer/record
        ├─/restaurant/customer/edit
        ├─/restaurant/customer/read
        ├─/restaurant/table/read
        ├─/restaurant/table/payment
        ├─/restaurant/customer/history
        

###软件设计技术

####flask的使用
使用flask可以快速处理各种request，并根据API设计返回相应的response，举例如下：

    @app.route('/restaurant/customer/read')
    def customerRead():
        # deal request from frontend
        # use database    
        return response

flask提供route装饰器，当发送对应的route请求时，会调用相应的函数，生成API设计所需要的response进行返回

####redis+session
        
redis可以用于存放临时数据，利用flask提供的session机制来生成redis所需要的key，然后存放前端发送的临时数据，根据需要来写入后台数据库或进行删除，这样可以减轻数据库的读写压力，同时提高后台的传递速度

####CORS
开发RESTful后端时，前端请求会遇到跨域的问题，处理方式是引用flask_cors，在app初始化时进行设置，同时编写一个包装函数，为每一个response进行headers包装

    res.headers['Access-Control-Allow-Origin'] = '*'
    res.headers['Access-Control-Allow-Methods'] = 'POST,GET,PUT,DELETE,OPTIONS'
    res.headers['Access-Control-Allow-Headers'] = 'x-requested-with,content-type'