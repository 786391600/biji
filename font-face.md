#font-face的使用

`@font-face {`
 ` font-family: abc;  /* project id："182472" */`
 ` src: url('css/iconfont.eot');`
 ` src: url('css/iconfont.eot?#iefix') format('embedded-opentype'),`
  `url('css/iconfont.woff') format('woff'),`
 ` url('css/iconfont.ttf') format('truetype'),`
  `url('css/iconfont.svg#iconfont') format('svg');`
`}`
.fontface{
	font-family: abc;
	font-size: 300px;
	color:#ccc;
}
.fontface:before{
	content: "\e606 \e6db ";
}