# 采坑记录
这里记录着我第一次开发uni-app的整个过程的采坑记录
pages.json：配置页面路由、导航条、选项卡等页面类信息，详见。
manifest.json：配置应用名称、appid、logo、版本等打包信息，详见。
App.vue：应用配置，用来配置App全局样式以及监听应用的生命周期。
main.js：Vue初始化入口文件
static目录：存放应用引用静态资源（如图片、视频等）的地方，注意：静态资源只能存放于此
pages目录：业务页面文件存放目录
components目录：组件文件存放目录
## 小程序采坑篇
1. 引用第三方字体
```
	//引入阿里矢量图标库为例，选中阿里云图标后不是下载，而是创建项目，然后利用项目生成下面的cdn（uniapp对于本地引入文件编译出错）
	@font-face {
	  font-family: 'iconfont';  /* project id 1099141 */
	  src: url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.eot');
	  src: url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.eot?#iefix') format('embedded-opentype'),
	  url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.woff2') format('woff2'),
	  url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.woff') format('woff'),
	  url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.ttf') format('truetype'),
	  url('https://at.alicdn.com/t/font_1099141_1p8edrri1z4.svg#iconfont') format('svg');
	}
	
	//除了上面的引用方法，我们还可以将字体包 转换成[base64编码字体](https://transfonter.org)
```

2. 对于小程序自定义导航栏
```
	//小程序app.json中的tabBar设置
	"tabBar": {
		"custom": true,//这个值设置未true
		"color": "#7a7e83",
		"selectedColor": "#0faeff",
		"backgroundColor": "#ffffff",
		"list": [……]
	  }
	//如果custom设置未true（默认未false），系统就会自动引入根目录下的custom-tab-bar文件，里面文件命名都是index.xxx
	//对于这个文件，怎么去配置编译我还没搞清楚，我只能手动复制进去到编译前和编译后的代码里面
	//如果没复制进去，编译后会自动清楚不相关文件
```

3. 1.7.1测试版bug(听说其他版本也会)
```
	:style=""必选用双引号包起来，如果其他字符用单引号（反过来用报错）
```


4. base64图片在传输中会加入回车，赋值前从计算属性重新替换下
```
	computed:{
		captchaSrc(){
			return this.captcha.replace(/[\r\n]/g, "");
		}
	},
```
5. 如果删除或者修改文件  最好重新运行，不然可能会导致文件不存在错误

6. input封装时，外层用v-model绑定后，如果要改变子组件input的值，需要用$nextTick
```
	this.$nextTick(function(){
		this.inputVal = 'xxxxx';
	})
```
