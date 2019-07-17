NGUI和UGUI区别
	1.Label: NGUI的Label支持静态图文混排和文字URL超链接，UGUI不支持。
	2.Anchor: UGUI  RectTransform是对NGUI的Transform+Widget+Anchor的封装。NGUI的Anchor可以相对特定的GO，UGUI的Anchor只相对父节点。
	3.图集：NGUI是必须先打出图集然后才能开始做界面。在制作的时候需要将图片打入图集后才能进行制作；UGUI：自带的图集打包模式，sprite packer。
	4.渲染顺序：NGUI可以看到源码，现在UGUI也开源了，分析可得NGUI会先根据Panel的depth进行排序，然后再按WidgetDepth排序。UGUI：采用层级和Z轴坐标来决定渲染顺序，越下面渲染在顶层。（具体的渲染顺序参考本人下篇博文，UGUI渲染顺序）。
	5.uGUI的Image可以使用material
	6.UGUI通过Mask来裁剪，而NGUI通过Panel的Clip
	
