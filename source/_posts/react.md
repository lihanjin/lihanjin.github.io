---
title: React知识点小结
date: 2016-04-23 22:24:08
tags: React
categories: React
---
# 1.react 和jq的区别？

reactjs 是个mvc的组件化框架,jquery 只是个函数库。

# 2.为什么要用react 写项目？

	单页面应用 运行速度快



# 3.React 语法基于jsx   、  javascript xml      （nodejs里面的模板ejs）


jsx怎么理解？ javascript xml 混合写




  ReactDOM.render(
        `<div>
            <h2>你和React</h2>
            <ul>
                <li>列表1</li>
                <li>列表1</li>   {/**/}
                <li>列表1</li>
            </ul>           
        </div>`
        ,
        document.getElementById('box')

    )




# 4. 模板里面渲染数据   {}



# 5.定义数据

es5 

	getInitialState:function(){  
		return{  

		    msg:'这是InitialState里面的数据',
		    name:'张三',
		    list:''
		}

    }


    用这里的数据： this.state.数据名称


es6 


      constructor(props){
  
        super(props);     

        this.state={
            msg:'这是Life的msg',
            name:'张三',
            list:''
        }    
    }     


    用这里的数据：this.state.数据名称


# 6.定义模板


	    render(){

                return(
                    <div>

                        <h2>这是一个内容组件-{this.state.msg}--{this.state.name}</h2>
                    </div>
                )
            }

# 6.定义组件
	

	es5 
		var Header=React.createClass({

			getInitialState:function(){  
		        return{  

		            msg:'这是InitialState里面的数据',
		            name:'张三',
		            list:''
		        }

		    },

		    render(){

		    	return (
		    		<div></div>
		    	)
		    }
		})

	
es6 

	

	 class Header extends React.Component{

			constructor(props){

					super(props);

					//数据

					this.state={

						msg:'这是一个头部组件'
					}

					this.run=this.run.bind(this);

			}
			run(){
				
			}
			render(){

				return (

					<div></div>
				)
			}


	 }

	 暴露:

	 export default Header;



	 其他地方引入：

	 import Header from './Header.js';  //引入 js



# 7.模板里面执行方式

	onClick    注意大小写  注意不要写引号  不要写()

	onClick={this.run}

	onChange  

	
	传值：

	onClick={this.run.bind(this,'传递的数据')}




# 8.获取数据 以及更改数据

	获取
	this.state.数据

	更改

	this.setState({

		msg:'更改后的数据'
	})


# 9.input 框  onChange事件会传递一个事件对象


	<input onChange={this.方法}	 />

	 console.log(e);

	方法(){

		e.target.value

	}
	

	 

# 10.ref  获取dom节点

	给元素加ref属性   比如： <div ref='home'></div>	 

	this.refs.home  获取dom节点



# 11.	给模板里面的元素加属性

	注意：

		className 设置class

		htmlFor   设置for  

		style="对象"

		<div>
		   <label htmlFor="user">用户名： </label>
		   <input type="text" id="user" placeholder="用户名" />
		</div>

	其他的以前怎么写还是怎么写  id


# 12.父子组件传值

	1.父组件给子组件传值，子组件获取父组件的数据和方法

		1.父组件  调用子组件的时候传值	
			<Header msg='这是一个数据' h={this} />

		2.子组件获取父亲组件的数据

			this.props.msg

	 2.子组件给父组件传值 ，父组件获取子组件的数据和方法

	 	1.父组件调用子组件的时候定义ref
	 		<Header ref='header' />

	 	2.this.refs.header拿到了子组件的对象。可以获取子组件的数据和方法


# 13.父子给组件传值的时候 propTypes  defaultProps

	子组件里面通过 propTypes  可以验证父组件传递数据的合法性


	defaultProps 父组件不给子组件传值的时候的默认值



	Header.propTypes = {
	  text: PropTypes.string   /*指定 text是字符串类型*/
	};

	//不传值的时候  给props一个默认值
	Header.defaultProps = {
	  username: '张三'
	};


# 14.生命周期函数

	生命周期函数：

       组件加载之前  加载完成    更新数据   销毁组件触发的一系列的方法就是生命周期函数


	componentWillMount   


	componentDidMount  


	https://facebook.github.io/react/docs/react-component.html





# 15.axios请求数据

	https://github.com/mzabriskie/axios


	1.安装
	2.引入   
	3.使用

	//1.cnpm install axios  --save

	//2.import axios from 'axios'; 

	//3.看官方文档使用



# 16.创建单文件组件

	https://facebook.github.io/react/docs/installation.html



	1.安装脚手架 （ 项目生成工具）    只需要安装一次


	npm install -g create-react-app    /  cnpm install -g create-react-app
	 

	2.创建项目  （可以创建）

	create-react-app  项目名称

	cd 项目名称

	npm start 运行项目


	例如:

	create-react-app reactdemo01

	cd reactdemo01  

	npm start 执行项目


	3.运行  打开项目

	npm start


	4.正式打包 

	npm run build






# 17.路由配置  坑 （老版本）



	1.安装react-router 

		注意要指定版本安装

		npm info react-router  查看路由的版本



		cnpm install react-router@2.8.1    老版本的稳定版本



	2.引入import { Router, Route, Link } from 'react-router'    

	(index.js  App.js)


	3.定义组件  首页 Home组件  新闻 News组件



	4.配置路由 （ index.js）



		/  加载APP组件


		/home   加载Home组件

		/news  加载News组件

		ReactDOM.render((
		  <Router>
		    <Route path="/" component={App}>
		      <Route path="home" component={Home} />
		      <Route path="news" component={Inbox}>     
		      </Route>
		       <Route path="newscontent/:aid" component={NewsContent}>     
		      </Route>
		    </Route>
		  </Router>
		), document.body)




	5.配置 加载的组件 的显示的地方   （App.js）


	  {this.props.children}


	6.页面跳转 （组件的跳转）   (任何的组件里面都可以写)

		 <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/inbox">Inbox</Link></li>
        </ul>


     7.Link动态传值


	<li><Link to={'/newscontent/'+aid}>新闻详情</Link></li>


     	<Link to={{pathname:'newscontent/'+this.state.aid,query:{name:'zhangsan'}}}>跳转新闻详情</Link>

     8.获取动态传值

     	 {this.props.params.aid}


 # 18.模板上面解析html


	https://facebook.github.io/react/docs/dom-elements.html


	function createMarkup() {
	  return {__html: '<h2>这是一个h2</h2>'};
	}


	render(){
		return (
		
			<div>
				<div dangerouslySetInnerHTML={this.createMarkup()} />;
			</div>
		)
	
	}

# 19.父子路由、路由的嵌套
嵌套路由

1.父路由下面配置子路由
            <Route path="news" component={News}>      
                    
                    <Route path="add" component={NewsAdd} />
                     <Route path="list" component={NewsList} /> 
            </Route>    
2.父组件里面的模板上面加上

     {this.props.children}  来显示子路由

*/



import Home from './components/Home.js';
    import Welcome from './components/Welcome.js';

import News from './components/News.js';
    import NewsList from './components/NewsList.js';
    import NewsAdd from './components/NewsAdd.js';




ReactDOM.render(
    
    <Router history={browserHistory}>
        <Route path="/" component={App}>
            <IndexRedirect to="/home/welcome" />

            <Route path="home" component={Home}>
                  <IndexRoute component={Welcome}/>
                  <Route path="welcome" component={Welcome} />
                 
            </Route>

            <Route path="news" component={News}>      
                    
                    <Route path="add" component={NewsAdd} />
                     <Route path="list" component={NewsList} /> 
            </Route>    
                     
           
        </Route>
    </Router>, 



点击加class	


	<ul>
                <li><Link to="/home" activeClassName="active" onlyActiveOnIndex={true} >首页</Link></li>
                <li><Link to="/news" activeClassName="active" >新闻</Link></li>
        </ul>  


	onlyActiveOnIndex={true} 如果加上这个数据 路由和设置的路由完全相等的情况选中




# 20.js跳转路由。
	

	browserHistory.push('home');


# 21.swiper的使用

	1.cnpm install swiper --save


		
	2、import Swiper from 'swiper';


	3.引入swiper.css


	4.自己改变元素的高度


	5.componentDidMount实例化Swiper   

	    componentDidMount(){
		//DOM操作
		var mySwiper = new Swiper('.swiper-container', {
		    autoplay: 5000,//可选选项，自动滑动
		})
	    }

# 22.ant desigin  react的UI框架


	官网https://ant.design/index-cn


	1.安装

			cnpm install antd  --save


	2.引入

			import { Button } from 'antd';			

	3.引入css 		

			1.新建一个自己的css

			2.在自己的css中引入 antd的css

			@import '~antd/dist/antd.css';

	4.看文档使用			


	

	报错怎么办？


	提取里面的antd.css ,自己引入



# 23.导航选中。


	<ul>
                <li><Link to="/home" activeClassName="active" onlyActiveOnIndex={true} >首页</Link></li>
                <li><Link to="/news" activeClassName="active" >新闻</Link></li>
        </ul>  


	onlyActiveOnIndex={true} 如果加上这个数据 路由和设置的路由完全相等的情况选中


	
	