[主页](https://github.com/greyli/helloflask)
/ [勘误](https://github.com/greyli/helloflask/blob/master/errata/errata.md)
/ FAQ
/ [示例程序](https://github.com/greyli/helloflask/blob/master/demos/)
/ [HelloFlask.com](http://helloflask.com)
/ [本书主页](http://helloflask.com/book)

# FAQ

如果你想提一个问题，请创建Issue。

### 为什么没有使用Flask-RESTful编写Web API？

在写作Web API这部分内容时，我考察了目前所有流行的API编写扩展，其中相对处于活跃开发状态的有[Flask-RESTful](https://github.com/flask-restful/flask-restful)、[Flask-apispec](https://github.com/jmcarp/flask-apispec)、[Flask-Classful](https://github.com/teracyhq/flask-classful)、[Flask-RestPlus](https://github.com/noirbizarre/flask-restplus)、[Flask-Restless](https://github.com/jfinkels/flask-restless)这几个。第10章的程序一开始是使用Flask-RESTful的，但后来经过再三考虑，改为单纯用Flask实现。

Flask-RESTful以及它的fork改良版Flask-RESTplus中的请求解析、响应格式化已经不推荐使用，而请求解析部分也将在未来移除，假如不使用这两部分功能，那么已经没有太大必要采用这个扩展，这是我没有使用它们的主要原因；而Flask内置的MethodView已经基本能够替代Flask-Classful，而且这个扩展看起来不太灵活；Flask-RESTless依赖于SQLAlchemy，不便于在作为一个作为示例用途的程序中使用；至于Flask-apispec，它结合了webargs和marshmallow，也提供了API文档生成功能，的确是非常好的选择，我很看好这个扩展，但是因为其刚刚出现不久，还不够完善，所以也没有选用。

### 为什么不叫REST API而使用Web API
有些读者可能不理解我为什么把常说的REST API/RESTful API说成Web API，我摘取书里的这部分内容供你参考一下：

> “仅仅通过HTTP协议返回JSON或XML数据的Web API并不能算是严格意义上的REST API。REST的提出者也在博文（http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven）中指出不是使用了HTTP的API都叫REST API。为了避免产生混乱，本章会尽量避免REST这个词。事实上，我们不必完全按照REST的架构要求来设计API。要尽量从API的自身特点和普适的规范来设计，而不是拘泥于REST一词。”

### 使用Anaconda时如何通过Pipenv创建虚拟环境

Anaconda我没用过，简单了解了一下，觉得很成熟和方便。不过，因为书中的示例程序的依赖都写在了Pipfile里，目前似乎只有Pipenv支持从Pipfile读写依赖信息（？）。Pipenv作为未来Python官方推荐的包依赖和环境管理工具，你也可以尝试一下。
这里出错通常是因为默认的Python解释器（即Anaconda内置的第三方Python发行版）和virtualenv不兼容。你可以使用下面的命令尝试一下：
```
pipenv --python /usr/local/bin/python3
```

### PyCharm无法配置运行程序

旧版本的PyCharm不支持通过模块来执行命令。新版本的PyCharm在图1-6的第4点位置有一个下拉选项，可以选择Module name，而不是Script。如果你使用的PyCharm没有这个下拉选项，可以通过为Python解释器添加-m选项可以起到类似的效果，即python -m flask run。

![旧版本PyCharm配置提示](https://camo.githubusercontent.com/a66b13d20c512156790cd3aa8cd29c4e8ac2740b/687474703a2f2f69322e6276696d672e636f6d2f3635393834312f346564633638333033306161393266372e706e67)

### 启动程序出现`TypeError`异常

Werkzeug当前版本（14.2）存在一个Bug，当在Windows系统下使用Python2开启调试模式时，重载器会因为环境变量FLASK_ENV的编码问题而出现TypeError异常。这个Bug已在master分支修复（话说定位这个Bug花了我很长时间），预计在纸书正式发售前会发布Werkzeug 0.15版本。

目前，临时的解决方案有修改Werkzeug源码、修改python-dotenv源码、从GitHub上的master分支更新Werkzeug等，但这些方法都太麻烦。我建议你临时不开启调试模式来避免这个异常出现，也就是在.flaskenv文件中将FLASK_ENV定义那一行注释掉（使用#号），比如：

```
# FLASK_ENV=development
```
等到Werkzeug 0.15发布后，我会在知乎专栏Hello, Flask!发一篇文章通知大家更新本地依赖，并给出具体的更新方式。