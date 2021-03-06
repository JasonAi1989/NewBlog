title: Ruby elementary 1
toc: true
date: 2015-04-06 14:46:42
tags: [Ruby]
categories: Ruby 
description: The basic knowledge about the Ruby
feature: http://7xj4cp.com1.z0.glb.clouddn.com/ruby.png
---

## ruby install

使用Brightbox 维护的 Ruby PPA安装ruby到Ubuntu 上

原文url：http://chloerei.com/2014/07/13/the-best-way-to-install-the-latest-version-of-ruby-on-ubuntu/

		sudo apt-add-repository ppa:brightbox/ruby-ng
	
		sudo apt-get update

<!--more-->

		sudo apt-get install ruby2.1 ruby2.1-dev

		sudo gem install bundler

		ruby -v

注：如果第一句命令出现问题

    apt-add-repository: command not found
    
可以使用如下命令进行安装相关的依赖包

    sudo apt-get install python-software-properties
    sudo apt-get install software-properties-common 
    
## rails install

		sudo gem install rails -V
		
		rails -v

*这里可能会安装失败，如果是权限问题，那原因是gem需要sudo权限，于是sudo gem install rails -V试试；*

*如果不是权限问题导致的错误，建议再次安装，因为https://rubygems.org/ 这个站点有时可能会连接不上，导致下载文件出错。*	

*-V是为了能显示出详细的安装信息，不然会出现半天看不到反映，但其实是在静默安装的现象*

*由于使用PPA库安装的原因，以后在调用gem安装插件时需要使用sudo权限*

#### 安装失败

如果你无法安装成功，那可能是因为很多原因引起的，来试试下面的方法吧。

>1、权限问题，使用sudo权限，sudo gem install rails -V

>2、站点不稳定， https://rubygems.org/ 这个站点有时会不稳定，建议重试

>3、缺少库。如果是和zlib相关的错误，比如

	'zlib is missing; necessary for building libxml2'

	那可以安装包zlib1g和zlib1g-dev：
	
	sudo apt-get install zlib1g zlib1g-dev

>4、缺少库。如果是和nokogiri相关的错误，可以安装包：
	
	sudo apt-get install libxslt-dev libxml2-dev
	
	sudo gem install nokogiri -V 
	
	如果还是有问题，请尝试
	
	sudo gem install nokogiri -V -v '1.5.10'

>5、如果是缺少其他的库请自行百度谷歌，因为每台电脑的情况都不一样，我就不在这里罗列了
	
#### gem是个啥？

gem有多个意思，比如G.E.M是歌手邓紫棋的英文名字，她唱的歌倒是蛮好听的；

在英文中gem是宝石、珍宝、精华的意思；

而在Ruby的世界里，gem是一类基于Ruby开发工具包，而gem install这个命令呢就是安装这些工具包用的，比如rails，他就是基于
Ruby的工具，专门用来做网站的工具。

#### gem命令

    gem update  --system    //更新gem版本
    gem install [gem name]    //从远程gem仓库安装gem包
    gem install [gem name] -V    //从远程gem仓库安装gem包，并显示安装的详细信息
    gem install -l [gem name]    //从本机gem仓库安装gem包
    gem install [gem name] --version=[ver]    //从远程gem仓库安装指定版本的gem包
    gem update                //更新所有已经安装的gem包
    gem update [gem name]    //更新指定的gem包
    gem uninstall [gem name]    //删除指定gem包的所有已安装版本
    gem uninstall [gem name] --version=[ver]  //删除指定gem包的指定版本
    gem list   //显示本地已安装的gem包
    gem -v     //gem版本
    gem search [gem name] --both   //从本地和远程服务器上查找含有gem name字符串的包
    gem search [gem name] --remoter //只从远程服务器上查找含有gem name字符串的包
    gem cleanup //清除所有包旧版本，保留最新版本
    gem sources --remove https://rubygems.org/  //删除gem源
    gem sources -a http://ruby.taobao.org/  //添加gem源
    gem sources -l    //显示gem源列表

## create the first project -- 'blog'

		rails new blog
	
这个过程中可能会让你输入密码，那是在安装bundle时需要权限引起的，直接输入sudo时使用的密码即可。

#### 创建失败

创建项目时默认使用的是sqlite3数据库，因此如果你的电脑上没有安装这个软件就会发生创建失败。
	
	sudo apt-get install libsqlite3-dev
	
	sudo gem install sqlite3 -V
	
如果不想用sqlite3来做数据库，比如你想用mysql，可以用一下的操作

	rails new blog -d mysql

#### 测试项目

	cd blog
	
	rails server
	
	然后打开浏览器，输入  http://localhost:3000  如果出现 'welcome aboard' 页面，说明项目创建成功了
	
#### bundle是个啥？

同样，bundle也是有很多个意思，首先在英文中是收集、归拢的意思；其次他是Unix/Linux上的一种可执行文件格式；同时他也是用于Android的Activity之间传递数据的类；而在Ruby的世界中呢，Bundle则是用来管理项目中所有gem依赖的工具，该命令只能在一个含有Gemfile的目录下执行，所有Ruby项目的依赖包都在Gemfile中进行配置，此功能是在Rails3中被引入的。

#### 总结

>+ 使用apt-get安装ruby和ruby-dev（带有-dev的为驱动文件），与此同时，gem软件也会随之被安装。可见gem和ruby是紧耦合的关系。gem可执行程序一般放在/usr/bin/目录下。

>+ 使用gem去安装各种工具，如bundler、rails、nokogiri，这些工具都是使用ruby语言开发的，这些可执行程序一般放在/usr/local/bin/目录下。

>+ 使用rails去创建项目，在创建项目时会调用bundle程序，当然此调用可以被跳过，使用--skip-bundle即可跳过。

>+ 使用bundle去安装Gemfile文件中所包含的库文件，这些库文件都是项目中会依赖到的库文件，这些库文件会被安装到gem依赖库目录下，一般为/var/lib/gems/目录，这些库文件是供全局所有项目使用的。与此同时，bundle还修改了Gemfile.lock文件，将刚刚所安装的库文件的名字写入此文件，这是为了此项目能够找到所依赖的库而准备的。简言之，bundle做了两件事：1.安装全局库；2.将安装的库文件写到依赖文件Gemfile.lock中。

#### Tips
使用bundle install或者run bundle install时会联网下载gem依赖包，此过程受网速影响有时会很慢，如果你的电脑已经安装了这些依赖包（比如第一次创建一个项目时就将所有的依赖包装全了），当再次创建新项目时就理应没有必要再次下载这些依赖包，但是Gemfile.lock文件中还必须添加这些文件，一个好的办法就是自己在电脑上搭建一个gem服务器，其实很简单，操作如下：

运行

    gem server
    
会出现如下提示：

    Server started at http://0.0.0.0:8808
    Server started at http://[::]:8808

说明gem服务已经绑定到本机所有IP的8808端口上，接下来我们就可以通过访问对外IP（比如192.168.1.20）或者本地回环IP（127.0.0.1）的8808端口来访问gem服务器了。

将Gemfile文件的首行改为：

    source "http://192.168.1.20:8808/"
    
或者

    source "http://127.0.0.1:8808/"
    
然后运行

    bundle install
   
即可快速将这些依赖包的名字写到本地的Gemfile.lock文件中

## use controller

		rails generate controller welcome index
		
wlecome为一个controller，index为wlecome中的一个action，当在浏览器中敲入 http://0.0.0.0:3000/welcome/index 时便会跳转到此controller对应的view视图中，也就是一个页面。

如果显示的页面有问题，可以在Gemfile中添加两句

		gem 'therubyracer', platforms: :ruby
		gem 'execjs'
		
并把最上面的地址改成

		source 'https://ruby.taobao.org'

然后执行

		bundle install

安装所需要的软件包

#### Tips
    rails generate -h
    Usage: rails generate GENERATOR [args] [options]
    General options:
      -h, [--help]     # Print generator's options and usage
      -p, [--pretend]  # Run but do not make any changes
      -f, [--force]    # Overwrite files that already exist
      -s, [--skip]     # Skip files that already exist
      -q, [--quiet]    # Suppress status output
      
最后一个参数action可以是多个action，比如

    rails generate controller welcome index new create update
    
## resources 方法
使用resources 方法可以创建一个资源，资源是指一些列的对象，包括文章、人、动物。资源可以被创建、读取、更新和销毁，简称为CRUD。


在config/routes.rb文件中添加如下这行：
    
    resources :letters
    
*注：此处使用复数(s)和不适用复数所创建的资源，在路径上会有很大的差别，具体可以使用rake routes来进行查看，这里尽量使用复数*

当资源名为letter时：

    rake routes
          Prefix Verb   URI Pattern                Controller#Action
    letter_index GET    /letter(.:format)          letter#index
                 POST   /letter(.:format)          letter#create
      new_letter GET    /letter/new(.:format)      letter#new
     edit_letter GET    /letter/:id/edit(.:format) letter#edit
          letter GET    /letter/:id(.:format)      letter#show
                 PATCH  /letter/:id(.:format)      letter#update
                 PUT    /letter/:id(.:format)      letter#update
                 DELETE /letter/:id(.:format)      letter#destroy
***
当资源名为letters时：

    rake routes
        letters GET    /letters(.:format)          letters#index
                POST   /letters(.:format)          letters#create
     new_letter GET    /letters/new(.:format)      letters#new
    edit_letter GET    /letters/:id/edit(.:format) letters#edit
         letter GET    /letters/:id(.:format)      letters#show
                PATCH  /letters/:id(.:format)      letters#update
                PUT    /letters/:id(.:format)      letters#update
                DELETE /letters/:id(.:format)      letters#destroy
***
#### 创建对应的文件
快速创建文件的方法当然是使用rails generate命令，但如果我们直接这么做会发生什么呢，试一下：

    rails g controller letters index new create edit show update destroy
    
然后用git比较一下有哪些文件改变了

    :000000 100644 0000000... 24f83d1... A  app/assets/javascripts/letters.coffee
    :000000 100644 0000000... afad32d... A  app/assets/stylesheets/letters.css
    :000000 100644 0000000... c37c854... A  app/controllers/letters_controller.rb
    :000000 100644 0000000... cad8d62... A  app/helpers/letters_helper.rb
    :000000 100644 0000000... e47e0ab... A  app/views/letters/create.html.erb
    :000000 100644 0000000... a7882fd... A  app/views/letters/destroy.html.erb
    :000000 100644 0000000... b1e28b1... A  app/views/letters/edit.html.erb
    :000000 100644 0000000... ee73dff... A  app/views/letters/index.html.erb
    :000000 100644 0000000... c1ab9a4... A  app/views/letters/new.html.erb
    :000000 100644 0000000... 23ab1a4... A  app/views/letters/show.html.erb
    :000000 100644 0000000... 487c48a... A  app/views/letters/update.html.erb
    :100644 100644 c9b29fe... 6df43b7... M  config/routes.rb
    :000000 100644 0000000... 831ba05... A  test/controllers/letters_controller_test.rb
我们发现，除了生成我们想要的那一系列文件外，还对config/routes.rb文件进行了修改，然后我们继续看一下这个文件做了哪些改变：
     Rails.application.routes.draw do
 
    +  get 'letters/index'
    +
    +  get 'letters/new'
    +
    +  get 'letters/create'
    +
    +  get 'letters/edit'
    +
    +  get 'letters/show'
    +
    +  get 'letters/update'
    +
    +  get 'letters/destroy'
    +
       # The priority is based upon order of creation: first created -> highest    priority.
       # See how all your routes lay out with "rake routes".

添加的内容是新生成文件的路由，再来用rake routes来看一下路由状况发生了什么变化。

    rake routes
             Prefix Verb   URI Pattern                 Controller#Action
      letters_index GET    /letters/index(.:format)    letters#index
        letters_new GET    /letters/new(.:format)      letters#new
     letters_create GET    /letters/create(.:format)   letters#create
       letters_edit GET    /letters/edit(.:format)     letters#edit
       letters_show GET    /letters/show(.:format)     letters#show
     letters_update GET    /letters/update(.:format)   letters#update
    letters_destroy GET    /letters/destroy(.:format)  letters#destroy
            letters GET    /letters(.:format)          letters#index
                    POST   /letters(.:format)          letters#create
         new_letter GET    /letters/new(.:format)      letters#new
        edit_letter GET    /letters/:id/edit(.:format) letters#edit
             letter GET    /letters/:id(.:format)      letters#show
                    PATCH  /letters/:id(.:format)      letters#update
                    PUT    /letters/:id(.:format)      letters#update
                    DELETE /letters/:id(.:format)      letters#destroy
                    
我们会发现，相同的文件会对应两个不同的前缀，这样会不会在之后的开发中造成问题呢？现在我还不清楚，为了避免以后发生问题，我们将新多出来的路由地址给去掉。（在config/routes.rb中注释掉）

#### 创建表单
修改new的文件

    <%= form_for :letter, url: letters_path do |f| %>
      <p>
        <%= f.label :title %><br>
        <%= f.text_field :title %>
      </p>
     
      <p>
        <%= f.label :text %><br>
        <%= f.text_area :text %>
      </p>
     
      <p>
        <%= f.submit %>
      </p>
    <% end %>

letters_path代表路由表中letters前缀对应的文件地址，url: letters_path的意思是在表单提交后跳转到letters_path的post方法对应的页面上，即跳转到create页面。

#### 表单响应
提交表单后即跳转到create页面，在create页面添加如下内容，便可以显示提交的内容：

    def create
      render plain: params[:article].inspect
    end
    
显示的结果类似如下：

    {"title"=>"123", "text"=>"test"}
这只是将表单显示出来，但并没有太多实际的意义。

## 使用model
#### 创建模型
在 Rails 中，模型的名字使用单数，对应的数据表名使用复数。
    
    bin/rails generate model Letter title:string text:text

Letter为模型的名字；

title和text为自动添加到letters数据表中的属性；

string和text为这两个属性的类型，即字符串类型和文本类型

在这里，数据表和模型之间会自动建立映射，所以不用担心此问题。

#### 运行迁移
通过model命令生成的db/migrate/20150402055237_create_letters.rb文件，生成数据表，命令如下：

    rake db:migrate

我们发现会生成文件 db/schema.rb

#### 调用模型存储数据
修改create函数如下：

    def create
      @letter = Letter.new(params[:letter])
 
      @letter.save
      redirect_to @letter
    end

在 Rails 中，每个模型可以使用各自的属性初始化，自动映射到数据库字段上。create 动作中的第一行就是这个目的（params[:letter] 就是我们要获取的属性）。@article.save 的作用是把模型保存到数据库中。保存完后转向 show 动作。稍后再编写 show 动作。

但代码如上去写会有问题，大家可是用浏览器试试看。

把命令改成如下：

    def create
      @letter = Letter.new(letter_params)
     
      @letter.save
      redirect_to @letter
    end
     
    private
      def letter_params
        params.require(:letter).permit(:title, :text)
      end
      
私有函数letter_params是用来明确哪些参数可在控制器中使用。其中permit这个方法就是允许title和text属性在动作中使用。

这里使用的Letter对象就是我们生成的模型

现在再在new页面进行提交，就会将数据存储到数据库中了。

## 显示存储内容
#### 显示单封信
将信件存储到数据库后我们还需要将他再显示出来，通过show函数即可实现。先看一下show的路由信息。

    letter GET    /letters/:id(.:format)      letters#show
    
其中有个id，这个id指的就是信件在数据库中的编号，也就是信件的id。
修改show函数
    
    def show
      @letter = Letter.find(params[:id])
    end
    
shou的页面改为

    <p>
      <strong>Title:</strong>
      <%= @letter.title %>
    </p>
 
    <p>
      <strong>Text:</strong>
      <%= @letter.text %>
    </p>
    
Letter.find就是查找对应id的信件

#### Tips
这里你可能会遇到一个问题，打开某个信件时提示没有title方法，这可能是因为你添加私有函数letter_params导致的，private关键字会一直生效到下一个类似关键字（public, private, protected）或者类结束为止，而只有类中的public 方法才能在view中被调用，因此这个show方法被错误的变成private类型导致在view中无法被调用。

#### 列出所有信件
在index中列出所有文章，修改index函数

    def index
      @letters = Letter.all
    end

修改视图

    <h1>Listing letters</h1>
 
    <table>
      <tr>
        <th>Title</th>
        <th>Text</th>
      </tr>
 
      <% @letters.each do |letter| %>
        <tr>
          <td><%= letter.title %></td>
          <td><%= letter.text %></td>
        </tr>
      <% end %>
    </table>
    
## 跳来跳去
在各个页面之间添加跳转，包括控制器间跳转和控制器内跳转

#### 控制器间跳转
创建一个welcome控制器和action，编辑index的视图

    <%= link_to 'Letters Wall', controller: 'letters'%>
    
在控制器之间跳转时要加上controller， 之后的letters为控制器的名字而不是路由标识

*注：好像不加关键字controller而是直接用路由标识也可以，就像控制器内跳转一样。*

#### 控制器内跳转

    <%= link_to 'New Letter', new_letter_path%>
在控制器内跳转只需指定对应的路由标识即可

## 添加css、js和图片
#### 添加css文件
内部css就不说了，直接在view中的文件里添加即可。

外部css文件要放到app/assets/stylesheets/文件夹下。

#### 添加js脚本文件
内部js脚本就直接写在view中的文件里即可。

外部的js文件要放到app/assets/javascripts/文件夹下。好像不需要在view中文件里面添加这个脚本文件的路径就可访问的到。

#### 图片文件
图片文件要放到app/assets/images/文件夹下，在view中文件里面直接引用此图片文件的名字即可被访问到