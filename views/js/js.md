# Java Script 使用笔记 #

----------

1. 点击事件

	$("父元素").on("click","xx元素",function（）{})

2. 禁用input输入的方式

	readonly、disabled、autocomplete 

	readonly表示此域的值不可修改，仅可与 type="text" 配合使用，可复制，可选择,可以接收焦点，后台会接收到传值. 
	
	复制代码代码如下:
	
	input type="text" name="www.xxx" readonly="readonly"
	
	disabled表示禁用input元素，不可编辑，不可复制，不可选择，不能接收焦点,后台也不会接收到传值 
	
	复制代码代码如下:
	
	input type="text" name="www.xxx.com" disabled="disabled" 
	
	另外可以通过css屏蔽输入法：
	input style="ime-mode: disabled"
	最后介绍一个常用的标签，浏览器通常会记录input输入框的记录，所以你在输入的时候，经常会下拉很多内容，如下图： 
	如果你想去掉的话，最好加上autocomplete="off"，使用方法如下：autocomplete="off" 
	
	复制代码代码如下:
	
	input type="text" autocomplete="off" id="number" 

3. 多个同class节点如何实现同一click事件

	$(" xxx 父节点").on("click","子节点",fun(){})
	`永远不要用bootstrap提供的class样式去监听各种事件，不明白js的内部原理，用bs的class会多死好几倍的脑细胞`
	
4. 清除页面节点已存在的数据

	比如radio或checkbox的选中状态，removeAttr("checked")，用过有时会失效，prop("checked",true) => 添加选中，
	prop("checked",false) => 取消选中