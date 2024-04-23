css学习笔记：
1.因为块级元素有换行的特性，所以可以用来清除浮动。clear
2.外在盒子和内在盒子(容器盒子) ，外在盒子决定元素是一行显示还是换行显示。内在盒子决定元素的宽高和内容
3.元素width属性的默认值是auto, width:auto的常见的4种情况：
a.充分利用可用空间 
b.收缩与包裹。典型代表是浮动、绝对定位、inline-block元素或者table元素。
c.收缩到最小，table-layout: auto, 当每一列空间不够的时候，文字能断就断，但中文是随便断的，英文单词不能断。
d.超出容器限制：内容很长的连续英文和数字，或者设置了white-space: nowarp
4.块级元素一旦设置width，流的特性就丢失了。
5.格式化宽度: 格式化宽度仅出现在position属性值为absolute和fixed的元素中，在默认情况下，绝对定位元素的宽度表现是包裹性，宽度由内部尺寸决定，
      有一种情况例外，对于非替换元素，当left/right或者top/bottom对立方位的属性值同时存在的时候。元素的宽度是相对与最近的具有定位特性（position的值不是static）
      的祖先元素计算的，充分利用可用空间。
6.如何判断元素是否使用的是内部尺寸，当元素没有内容时宽度是0则使用的是内部尺寸，反之则是外部尺寸。内部尺寸有以下3种表现形式：
a.包裹性
b.首选最小宽度。当父容器宽度为0时，子元素的宽度设置为auto时，此时表现出来的就是首选最小宽度，因为css图片和文字的权重要远大于布局，不会让子元素的宽度等于0。东亚文字（中文）的首选最小宽度时每个文字的宽度。西方文字最小宽度由特定的连续的英文字符单元决定。（如果想让英文字符和中文一样，每一个字符都用最小宽度单元，可以试试使用css中的word-break: break-all)  。类似图片这样的替换元素的最小宽度就是该元素内容本身的宽度。
c.最大宽度。最大宽度就是元素可以有的最大宽度。最大宽度实际等同于"包裹性元素"设置white-space ：nowarp声明后的宽度。
7.CSS 宽度分离原则，为了保持元素的流体灵活性。如果给元素设置width设置一个值，同时也设置了padding或者border，元素会变得像一个砖头一样死板。
     所以width一般不与border或padding有时也包括margin组合。width独立占用一层标签，而padding、border、margin利用流动性在内部自适应呈现。
8.box-sizing属性改变的是元素width作用的盒子，content-box, border-box,padding-box。
9.box-sizing运用例子，使textarea、input等替换元素100%自适应父容器。替换元素的尺寸由内部元素决定，即使是设置display ：block也不具有流体的特性。所以要
     设置替换元素100%自适应父容器就必须要通过width:100%来确定。有一些替换元素是自带有border、或者padding设置的，此时要设置100%自适应父容器就必须设置box-  sizing : border-box
10.height和width的区别，对与百分比的支持。对于width属性就算父元素的width为auto, 子元素设置百分比也是支持的。在普通文档流中，高度百分比想要生效，父级必须有
       一个可以生效的高度值。
11.如果包含块的高度没有显示指定，且该元素不是绝对定位，则计算值为auto , 子元素的设置百分比不能参与计算。宽度如果没有指定就按照包含块真实的计算值作为
      百分比的计算基数。
12.如何让元素支持height:100%
a.设置显示高度值。
b.使用绝对定位。绝对定位的宽高百分比是相对于padding-box计算的，非绝对定位的百分比是相对与content-box计算的
13.min-width和max-width出现的场景一定是自适应布局或者流体布局。width和height的默认值是auto , max-width/height的默认值是none ,min-width/height的默认值是
      auto。
14.超越！important 的意思是当max-height/width比width加！important指定的宽度值小时，max-height/width会覆盖！important （同理min-height/width指定的值比！important还要大时，min-height/width也会覆盖！important）。当min-width和max-width冲突时 或者min-height和max-height冲突时，min-*优先级更高。
15.内联元素的内联特指外在盒子，和display:inline不是同一个概念，inline-block和inline-table 都是内联元素，从表现上来看，内联元素的特征是可以和文字在一行显示。浮动的元素并不是内联元素。
16.内联盒模型是内联世界深入的基础。
a.   内容区域：是一种围绕文字看不见的盒子，其大小仅受字符本身特性控制。本质上是一个字符盒子，像图片这样的替换元素，内容区域可以看作元素本身。
b.内联盒子，内联盒子不会让内容成块显示，而是排成一行，这里的“内联盒子”实际指的就是元素的"外在盒子"。用来决定元素是内联还是块级。该盒子又可以细分为“内联盒子”和“匿名内联盒子”两类
c.行框盒子。每一行就是一个行框盒子，每一个"行框盒子"又是由一个一个的内联盒子组成的
d.包含块。由一行一行的行框盒子组成。
17.幽灵空白节点：在HTML5文档声明中，内联元素的所有的解析和渲染就如同每一个行框盒子的面前有一个“空白节点”一样，这个空白节点永远透明，不占据任何宽度，看不见也无法通过脚本获取，但又确确实实存在，表现如同文本节点一样。
18.替换元素顾名思义就是内容可以被替换，通过修改某个属性值呈现的内容就可以被替换的元素就被称为替换元素。例如img(改变src的属性值图片会被改变,object标签的data属性指向一个url ,video标签url属性指向一个静态资源，iframe标签的src属性指向文档的url, textarea标签设置rows和cols可以定义文本显示的几行几列，input标签type属性值不同显示不同组件)
19.替换元素具有以下特征：
a. 内容不受页面上的css影响。如果需要改变替换元素的外观，需要类似appearance属性，或者浏览器自身暴露的一些样式接口，例如::ms-check{} ,但是直接input[type='checkbox']{}却无法改变内间距，背景色等样式。
b.有自己的尺寸。很多替换元素在没有明确尺寸设定的情况下，默认的尺寸（不包含边框）是300像素*150像素，如video,iframe,canvas等，也有少部分替换元素为0像素如img。表单元素的替换元素的尺寸则和浏览器有关没有明显规律。
c.在css属性上有自己的一套规则，例如vertical-align属性，默认值为baseline对于文本来说是以字母x的下边缘作为基线，对于替换元素来说则是元素的下边缘。
20.所有替换元素都是内联水平元素，即替换元素和替换元素，替换元素和文字都可以在一行显示。
21.替换元素的尺寸计算规则：
a.固有尺寸指的是替换内容原本的尺寸。例如图片、视频作为一个独立文件存在的时候，都是有自己的宽度和高度的，这个宽度和高度就是固有尺寸。对于表单类替换元素，固有尺寸可以理解为不加修饰的默认尺寸。
b.HTML尺寸，只能通过HTML的原生属性改变，这些HTML属性包括img标签的width和height ,input的size属性、textarea的cols和rows属性。
c.CSS尺寸特指可以通过css的width和height或者max-width/min-width和max-height/min-height设置的尺寸，对应盒尺寸中的content box
d.如果没有CSS尺寸和HTML尺寸，则使用固有尺寸作为最终宽高。
e.如果没有CSS尺寸，有HTML尺寸，则使用HTML尺寸作为最终宽高。
f.如果有CSS尺寸，则最终尺寸由CSS属性决定。
g.如果d,e ,f都不满足则宽高一般以300*150像素作为固有尺寸，img可能不符合。
h.内联替换元素和块级替换元素使用上面同一套尺寸计算规则（这也是为什么替换元素设置display： block，并不会100%容器的原因）。
22.img元素的固有尺寸本来不能被修改，我们之所以可以通过修改width和height来修改宽高是因为css的object-fit属性的值是fill , 如果设置为none则没有变化。设置为contain则设置的宽高保持图片原有的宽高比例。
23.img标签不带任何属性的，不同浏览器有不同的理解。firefox视为普通的内联元素设置display为block会具有流体特性，chrome需要设置alt属性不为空，才会被视为普通内联元素，IE则始终都是替换元素不能转为非替换元素。
24.替换元素的after和before伪元素都不能被显示出来。
25. 如果非替换元素标签添加content属性，会变成替换元素，目前只有chrome、Safari、Opera等浏览器支持直接在元素设置content属性，其他浏览器都是只支持before或者after伪元素时设置content属性。content属性改变的只是视觉的呈现，用户无法选中，无法保存,无法被屏幕阅读设备读取，也无法被搜索引擎抓取。不能左右:empty伪类，如果元素不存在子节点，但是设置的content属性不为空也可以被:empty伪类选中。content如果是使用操作符生成的内容通过js无法获取到。
26. content图片生成，指的是直接用url功能符显示图片，url功能符中的图片地址不仅可以是常见的png, jpg格式。还可以是ico图片，svg文件以及base64url地址，但不支持css3渐变背景图。
27. content生成开启闭合符号，具体用法是在before伪元素中的content属性值设置为open-quote,在after伪元素中的content属性值设置为close-quote，在元素中设置quotes属性值为具体的字符。
28.  content内容可以通过attr功能符指定，attr功能符中为属性名称。
29.content还可以生成计数器。counter-reset主要作用是给计数器起一个名字，顺便指定从哪个数字开始计数：.xxx {  counter-reset: wangxiaoer  2  }; counter-reset的计数基数可以是负数。counter-reset 可以多个计数器同时命名 .xxx { counter-reset: wangxiaoer 2 wangxiaosan 3 } 。 counter-reset 还可以设置为none和inherit。取消重置以及继承重置。counter-increment 计数器递增，counter-increment的1个或者多个关键字，后面可以跟数字表示每次计数变化的值。普照源（counter-reset）是唯一的，每普照（counter-increment）一次,普照源增加一次计数值。
30.方法counter和counters的作用是显示计数，用法counter(name，style)，style参数的值就是lcss属性ist-style-type的值，例如：disc、circle、square、decimal等等。
31.counter方法也可以级联使用，也就是说，一个content属性可以有多个counter方法。
32.counters方法用于级联计数，比例目录的1.2 ，1.3等需要用到counters方法。counters的基本用法是：counters(name,string)。其中string参数表示子序号的连接字符串。普照源是唯一的，要想实现目录的嵌套计数，必须让每个容器拥有一个“普照源”。通过子辈对父辈的counter-reset重置、配合counters()方法才能实现计数嵌套的效果。
33.counters也是支持style参数的，counters(name,string,style)。
34. 内联元素的padding不仅在水平方向上会影响布局，在垂直方向上也会影响布局。之所以看起来垂直方向上没有影响，是因为内联元素没有可视宽度(clientWidth)和可视高度的说法(clientHeight)的说法，clientWidth和clientHeight永远都是0，垂直方向上的行为表现完全受line-height和vertical-align的影响。视觉上没有改变上一行和下一行之间的间距，因此，给我们的感觉是垂直方向上的padding没有起作用。如果给元素加一个背景色或者边框，就可以看到尺寸空间确实受影响了。
35.CSS中还有很多其他场景或者属性会出现这种不影响其他元素的布局而是出现重叠的现象，比如relative元素定位，盒阴影box-shadow以及outline等。这些层叠现象看似类似，但实际是有区别的。一类是纯视觉层叠，不影响外部尺寸，另一类则会影响外部尺寸。box-shadow以及outline属于前者，而内联元素的padding是属于后者。区分的方式，如果父容器设置overflow:auto，层叠区超出父容器的时候，没有滚动条出现，则是纯视觉的；如果出现滚动条，则会影响尺寸、影响布局。
36.内联元素垂直padding的应用：
a.增加可点击区域的大小。
b.生成类似 "登录 | 注册"管道符
c.当地址url使用锚点定位文档位置时，可以让标题距离浏览器视口顶部一点距离，而不破坏原来的布局。
37.对于非替换元素的内联元素，不仅padding不会加入行盒高度的计算，margin和border也都是如此，都是不计算高度，但实际上在内联盒周围发生渲染。
38.padding属性是不支持负值，padding是支持百分比的，padding百分比计算规则无论是水平方向还是垂直方向都是相对于宽度计算的
39.padding百分比应用在内联元素上，1、同样相对于宽度计算。2、默认的高度和宽度细节有差异。3、padding会断行。
40.内联元素的垂直padding会让幽灵空白节点显现，这就是为什么空的内联元素设置padding:50%并不是一个正方形的原因，只需设置内联元素的font-size：0就可以取消幽灵空白节点显示。
41.元素尺寸的相关概念:
a.元素尺寸：对应jQuery中的$().width()和$().height()。包括padding和border就是border box的尺寸。在原生的DOM API中对应offsetWidth和offsetHeight
b.元素内部尺寸：对应jQuery中的$().innerWidth() 和$().innerHeight方法表示元素的内部区域尺寸。包括padding但不包括border，也就是元素padding box的尺寸，对应DOM API的clientWidth和clientHeight，有时候也称"元素可视尺寸"
c.元素外部尺寸：对应jQuery中的$().outerWidth()和$().outerHeight()。不仅包括padding和border，还包括margin , 也就是margin box的尺寸
42.元素的外部尺寸可能是为一个负数，把元素的外部尺寸理解为"元素占据的空间的尺寸"可能比较容易理解。
43.只有元素是"充分利用可用空间"状态的时候，margin才可以改变元素的可视尺寸。
44.CSS世界的默认的流方向是水平方向，因此，对于普通流体元素，margin只能改变元素水平方向的尺寸，但是对于具有拉伸特性的绝对定位元素，则水平和垂直方向都可以
45.元素默认是水平流，可以通过设置writing-mode属性将水平流改为垂直流。
46.元素底部留白最好使用子元素的margin-bottom ， 不要使用父元素的padding-bottom， 因为有的浏览器可能会忽略padding-bottom的值。
47.和padding属性一样，margin的百分比值无论是水平方向还是垂直方向都是相对于宽度计算的。
48.内联元素垂直方向的margin是没有任何影响的，既不会影响外部尺寸，也不会影响内部尺寸。  
49.margin合并的条件，一是必须是块级元素，不包括浮动和绝对定位的元素。二是只发生在垂直方向上，(严格来说只发生在和当前文档流垂直的方向上)
50.margin合并的3种情况：
a.相邻兄弟元素margin合并。
b.父级元素和第一个或者最后一个子元素合并。（解决父子margin合并的问题办法：1. 父元素使用块级格式化上下文。2.和第一个元素的margin-top合并父元素设置border-top或者padding-toppadding-top，和最后一个元素的margin-bottom合并父元素设置border-bottom或者padding-bottom。3.父元素和第一个元素或者最后一个元素之间添加内联元素分割。4.如果和最后一个元素发生margin-bottom合并，父元素可以设置height、max-height或者min-height
c.空级块级元素的margin合并。例如：.father {overflow: hidden;}  .son { margin :1em 0} ； 此时.father所在的这个div的高度是1em ,因为margin-top和margin-bottom合在一起了。（处理方法：1.垂直方向设置border或者padding, 里面添加内联元素，设置height或者min-height)
51.margin合并规则，正正取最大值，正负值相加，负负取最负值。
52.当margin值为auto时，可能会触发自动计算。margin:auto自动计算的前提条件：当width或者height为auto时，元素具有对应方向的自动填充特性（默认流为水平方向，要垂直方向具有自动填充，需要改变父元素的流方向writing-mode）。满足这个前提条件下，auto的计算规则如下:
a.如果一侧为定值，一侧为auto ,则auto为剩余空间大小。
b.如果两侧均是auto, 则平分剩余空间大小。
c.如果没有剩余空间，auto将会为0, (margin的默认值为0)
53.margin无效的情形：
a.display值为inline的内联非替换元素，margin是无效的，而内联替换元素的margin是有效的，且不会发生margin合并。所以图片永远不会发生margin合并。
b.表格中的<tr>和<td>元素或者设置display为table-cell或table-row的元素的margin是无效的。如果计算值是table-caption、table、或者inline-table则没有此问题，可以通过margin控制外边距。first-letter伪元素也可以解析margin.
c.绝对定位非定位方位的margin无效，例如 img { top:10%;left:30%};此时如果添加margin-right:30px则不会生效。
d.定高容器的子元素的margin-bottom，定宽子元素的margin-right。例如：在默认流下，其定位方向是左侧及上方，此时只有margin-left和margin-top可以影响元素的定位。但是如果通过一些属性改变了定位方向，如果float:right或者绝对定位元素的right右侧定位，则反过来margin-right可以影响元素的定位，margin-left只能影响兄弟元素
54.border-width属性不支持百分比，outline、box-shoadow、text-shadow等，都不支持百分比值。
55.border-width除了直接使用数值1px , 还可以使用关键字。thin相当于1px ， medium相当于3px , thick相等于4px。
56.border-width的默认值是3px, 这是因为border-style为double至少为3px才能生效。
57.border-style为dashed虚线时，不同浏览器的显示不同。chrome和firefox虚线颜色区的宽高比是2：1。在IE浏览器二者都是2：1
58.border-style为double的表现规则，双线宽度永远相等，中间宽度（加一或者减一）
59.当没有指定border-color的颜色值时，会使用当前元素的color值作为边框色。具有类似特性的CSS属性还有outline、box-shadow和text-shadow等
60.默认background设置背景图片是相对于padding-box定位的，如果要增加可点击的区域的大小，可以使用border设置透明边框。
61.字母x和CSS世界的基线，字母x的下边缘就是基线。line-height行高的定义就是两个基线的间距，vertical-align的默认值就是基线。
62.x-height属性指的是小写字母x的高度，也就是基线和中线之间的距离。
63.从上到下与文字布局相关的5个线：
a.上行线，是小写字母所能到达的最上的边界，如字母h。
b.大写字母线，是大写字母的最上的边界，大写字母高度就是大写字母线与基线之间的距离。
c.中线。
d.基线，vertical-align:baseline就是相对于基线对齐的。中线与基线之间的距离就是x-height的值。
e.下行线，是小写字母所达到的最下边界如字母p.
64.vertical-align：middle是以1/2x-height的位置对齐，相当于小写字母x的交叉点。
65.css中的ex单位，ex是一个尺寸单位，1ex就是1个x-height
66.对于非替换元素的纯内联元素，其可视高度完全是由line-height来决定的。padding和border属性对其完全没有影响，这也是我们平常口中的"盒模型"约定俗成是说的块级元素的原因。
67.行距 = line-height值 - font-size值。半行距为其一半
68.半行距如果是小数，文字上边缘半行距相向取整，3.5px则为3px ， 下边缘向上取整，3.5px为4px。
69.line-height不能直接影响块级元素和内联替换元素的高度，但是在内联替换元素和内联非替换元素混排的时候，line-height确定的是行框盒子的最小高度，line-height可以通过影响内联非替换元素的高度进而影响块级元素的高度
70. 单行文字垂直居中，只需要设置内联元素的line-height不需要同时设置容器的height与内联元素的line-height一致。多行文字居中，将文字用标签包裹设置display为inline-block， 外层容器设置line-height
71.line-height可以设置数值、百分比值和长度值。设置数值和百分比值都是相对于当前元素的font-size来计算的，长度值如果设置em为单位也是相对与当前的font-size计算。但是这三种情况在继承上有不同，如果是数值，则继承到子元素时是相对于子元素的font-size计算。如果是百分比值或长度值，继承到子元素时，是以上级元素的计算值来继承。
72.在每个行框盒子前都存在幽灵空白节点，如果没有设置内联元素为display:inline-block，则行框盒子的最终高度是以组成行框盒子的最大高度，包括前面的幽灵空白节点来确定的，如果设置内联元素设置display为inline-block此时内联元素的行框盒子是独立的（只能对幽灵空白节点有效），不会和幽灵空白节点合并计算。
73.vertical-align属性支持以下几类值：
a.线类，如baseline(默认值)、top、middle、bottom
b.文本类，如text-top、text-bottom
c.上标下标类，如sub、super
d.数值百分比类，如20px,2em,20%等根据计算值的不同，相对与基线偏移，正值向上偏移，负值向下偏移。百分比值是相对于line-height计算的。
74.vertical-align的作用前提是内联元素（inline、inline-block、inline-table）以及display为table-cell的元素。由于浮动和绝对定位会让元素块状化，因此会让vertical-align无效
75.一个div定高的容器里放置一个图片（容器高度大于图片高度），将图片设置vertical-align ：middle ， 此时图片并不会垂直居中。是因为前面的幽灵空白节点太小，给div设置一个较大的line-height会解决问题。
76.同75的例子，如果给外面的容器设置display:table-cell和vertical-align为middle，不管div里的元素是内联元素还是块状元素，vertical-align都会生效。
77.在div容器定义line-height为32px, 里面有一个span元素设置字体为24px,div容器的高度并不是32px , 幽灵空白节点和span标签的font-size不同，font-size越大的字体基线越偏下，此时相对于基线对齐，所以当字号不一样时，会发生上下偏移原理如下图。解决方法，div也设置font-size24p或者改变垂直对齐方式为顶部对齐等。

78.一个div容器里放一张图片，图片高度并不能铺满整个容器，图片底部会出现一部分留白。原因是图片默认相对于幽灵空白节点的基线对齐，行高产生的多余的间隙会转移到图片下面。解决这个问题有4种方法：
a.图片块状化，这样幽灵空白节点就不存在了，也没有了line-height和vertical-align的问题
b.容器设置line-height足够小，消除幽灵空白节点下面的半行距。
c.font-size足够小，但是要与line-height组合使用，line-height的值必须要数字或者百分比。原理同b
d.图片设置其他vertical-align值。比如top、bottom、middle
79.vertical-align属性的默认值是baseline，在文本之类的内联元素就是x的下边缘，内联替换元素就是替换元素的下边缘。但是，如果是inline-block元素，如果里面没有内联元素，或者overflow不是visible，则该元素的基线就是margin的底边缘。否则其基线就是元素里面最后一行内联元素的基线。
80.vertical-align的top、bottom是设置元素与当前行框盒子的对齐方式。     
81.vertical-align:top/bottom的含义：
a.内联元素，元素顶部/底部和当前行框盒子的顶部/底部对齐。
b.table-cell元素，元素顶/底padding边缘和表格行的顶部/底部对齐。
82.vertical-align：middle底含义：
a.内联元素 , 元素的中心点和行框盒子基线往上1/2x-height处对齐。
b.table-cell元素，单元格填充盒子相对于外面表格行居中对齐。
83.vertical-align:text-up，盒子的顶部和父级内容区域的顶部对齐，vertical-align:text-bottom，盒子的底部和父级内容区域的底部对齐，
84.文本类属性值（text-up , text-bottom)的垂直对齐与左右文字的大小和高度都没有关系，而所有线性类属性值的定位都会受到兄弟内联元素的影响。
85.浮动的本质就是为了实现文字的环绕效果。
86.浮动的特性：
a.包裹性
b.块状并格式化上下文。块状化的意思是float的属性值不为none ,则其display计算值就是block或者table（只有inline-table浮动之后会变为table ，其他都是变为block) 。此时text-align和vertical-align都是无效的，只对内联元素有效。
c.破坏文档流
d.没有任何margin合并
87.float会让父元素的高度塌陷。父元素高度塌陷会让跟随的内容可以和浮动元素在一个水平线上显示，跟随的内容如果有行框盒子，则浮动元素和行框盒子不会发生重叠。
88.伪元素first-line会操作行框盒子的第一行。
89.浮动锚点和浮动参考，浮动参考的定义是浮动元素参考对齐的实体，这个实体是行框盒子。如果浮动元素前后是块状元素，将产生浮动锚点，浮动锚点的作用是产生行框盒子。
90.clear属性的默认值是none , clear:both和left和right没有什么差别，一般都使用both替换left或者right。
91.clear属性是让自身不能和前面的浮动元素相邻，注意这里的“前面的”3个字，也就是clear属性相对于"后面的"浮动元素是不闻不问的。因为要不与前面的浮动元素相邻，所以clear属性的作用对象必须是块级元素，不能是内联级元素。如果是::after伪元素或者before伪元素，必须家display：block才能生效。
92.clear:both的作用是不让自己和浮动元素在一行显示，并不是真正意思上的清除浮动，浮动元素依旧存在。
93.处理浮动元素父元素，高度塌陷问题。一种方式:浮动元素后面的兄弟元素使用clear清除浮动,另外一种可以触发BFC。
94.BFC块级格式化上下文，是CSS世界的结界，如果一个元素具有BFC, 内部元素在怎么翻江倒海，都不会影响外部元素，所以BFC元素是不可能发生margin重叠的。（不会与任何元素有重叠，除非绝对定位强制与其重叠）BFC也可以清除浮动带来的影响。
95.BFC的触发：
a.<html>根元素
b.float不为值不为none.
c.overflow的值为auto，scorll , 或者hidden
d.display的值为table-cell、table-caption或者inline-block中的任何一个
e.position的值不为relative和static
96.虽然触发BFC的条件有很多但是能兼顾流体特性和自适应性的属性并不多，下面就分别做一下说明：
a.float:left , 浮动元素本身BFC化，然而浮动元素有破坏性和包裹性，失去了本身自适应的流体特性。
b.position: absolute ， 脱离文档流，和非定位元素很难玩到一块去。
c.overflow: hidden 。 目前来说的最佳实践。
d.display:inline-block，会让元素尺寸包裹收缩，对于IE6、IE7是比overflow：hidden更好的声明。
e.display： table-cell：表现的像单元格一样，和display:inline-block一样，会跟随内部元素的宽度显示。宽度设置的再大，实际宽度也不会超过表格的宽度，所以一般将宽度给一个很大的值，这样就表现的和水平元素自适应空间一模一样了。
f.display:table-row对width无感，无法自适应剩余容器的空间。
g.display:table-caption。此属性一无是处
97.清除浮动的影响，最合适的属性是overflow:hidden而不是clear , 但是overflow的本职工作是裁剪。
98.当子元素内容超出容器宽度高度限制的时候，裁剪边界是容器的border-box的内边缘，而不是padding-box的内边缘。
99.在chrome浏览器下，如果容器是可以滚动的，则容器的padding-bottom也算在容器的滚动范围之内，IE和firefox浏览器则会忽略padding-bottom。所以在开发时要尽量避免滚动容器设置padding-bottom，除了样式不一样外，还可能导致scrollHeight不一样。
100.overflow-x和overflow-y支持的属性值和overflow一模一样。
a.visible 默认值，超出容器范围也会显示出来
b.hidden  裁剪
c.scrollHeight  滚动区域一直在
d.auto 不足以滚地时没有滚动条，可以滚动时滚动条出现
101.如果overflow-x和overflow-y属性中的一个值设置为visible而另外一个值设置为scroll、auto或者hidden，则visible的样式表现会如同auto。也就是说除非overflow-x和overflow-y的值都是visible。否则visible会被当成auto解析。但是hidden、scroll和auto这3个值是可以共存的
102.HTML中有两个元素是默认可以产生滚动条的，一个是根元素<html>, 另一个是<textarea>元素。因为从IE8起这两个元素的默认overflow值是auto。
103.在PC端无论什么浏览器，滚动条都来自<html>，而不是<body> 
104.在PC端窗体的滚动高度可以用，document.documentElement.scorllTop来获取，在移动端则需要使用document.body.scrollTop来获取。
105.滚动条会占用容器可以的宽度和高度，滚动条的宽度为17px 。在移动端则不会有这样的问题，因为移动端的滚动条是悬浮的状态。
106.自定义webkit浏览器滚动条样式,：
a.整体部分，：：-webkit-scrollbar;(常用属性，血槽宽度)
b.两端按钮，：：-webkit-scrollbar-button,
c.外层轨道，：：-webkit-scrollbar-track（常用属性，背景槽）
d.内层轨道，：：-webkit-scrollbar-track-piece,
e.滚动滑块，：：-webkit-scrollbar-thumb,（常用属性，拖动条）
f.边角，：：-webkit-scrollbar-corner
107.依赖overflow的样式，文字超出容器范围点点点效果，.text { text-overflow : ellipsis ; white-space: nowrap ; overflow: hidden }
108.URL地址的锚点，可以使用location.hash获取，实现锚点跳转的方法有两种，一种是<a>标签以及name属性，另一种是利用元素的id属性。
a.<a href="#1"> 发展历程</a> // 通过点击这个    <a  name="1"> </a>//跳转到这里来
b.<a href="#1"> 发展历程</a> // 通过点击这个    <h1  id="1">发展历程</h1>//跳转到这里来
109.锚点定位行为的触发条件：
a.URL地址中的锚链与锚点元素对应并有交互行为。
b.可focus的锚点元素处于focus状态。
110.如果URL的锚链是一个很简单的# , 则定位行为发生时是定位到页面顶部，因此返回顶部的HTML一般都这样写：
a.<a href="#">返回顶部</a>
b.<a href="javascript:">返回顶部</a>
111.URL锚链和focus都是锚点定位，但是两者还是有区别，URL锚链是定位到浏览器窗体的上边缘，而"focus"是让元素在窗体范围内显示即可，不一定是上边缘。
112.锚点定位也会发生在普通元素上，定位行为是由内向外的，由内向外是指普通元素和窗体同时可滚动的时候，会由内向外触发，锚点定位到的元素会先在普通元素滚动到能显示的位置，然后窗体也会滚动，直到锚点定位的元素位于窗体上边缘。
113.设置了overflow:hidden的元素也是可以滚动的（虽然没有表现出滚动条，但是锚点定位时，也会定位到对应的元素），overflow:hidden跟overflow:scroll和overflow:auto的区别是有没有那个滚动条，元素设置overflow：hidden声明，里面的内容高度溢出的时候，滚动依然存在，仅仅是滚动条不存在。
114.锚点定位的本质是改变scrollTop和scrollLeft的值。
115.当position：absoluteh和float同时存在时，float将无效。position:absolute和float都具有包裹性和自适应性（自适应父容器的最大宽度）。
116.包含块的定义规则：
a.根元素，很多场景可以看成是html，被称为是"初始包含块"。其尺寸等同于浏览器的窗口大小。
b.对于其他元素，如果该元素的position是relative或者static，则包含块是由其最近的块容器祖先盒子的content box组成的。
c.如果元素是position：fixed则"包含块"是初始包含块
d.如果元素的position：abosolute，则包含块是由最近的position不为static的祖先元素建立。
117.和常规元素相比，absolute的包含块具有以下3个明显的差别。
a.内联元素也可以作为"包含块"所在的元素
b.“包含块”所在的不是父级元素，而是最近的position值不为，static的祖先元素或者根元素。
c.边界是padding box而不是content box
118.内联元素的"包含块"是由“生成的”前后内联盒子组成的，与里面内联盒子的细节没有任何关系。内联元素的"包含块"相对稳固，不会受line-height等属性影响。
119.跨行的内联元素包含块产生未定义行为，firefox只覆盖第一行，chrome则可以跨行显示，如果内联元素的最后一个内联盒子的右边缘比第一个内联盒子的左边缘还要靠左，则包含块的宽度并不为负，按0来处理。
120.对于普通元素height：100%和height：inherit的表现是一样的，都是由父元素决定，如果对于绝对定位元素。inherit是继承父元素的高度。100%是其包含块的100%高度
121.当只有position：absolute属性不存在top/left/right/bottom等定位属性时，元素是在当前位置显示。无依赖绝对定位本质是相对定位，仅仅是不占CSS流的尺寸空间而已
122.text-align可以改变absolute元素的对齐效果，这是"幽灵空白节点"和"无依赖绝对定位"共同作用的结果。
123.overflow对绝对定位元素的裁剪，绝对定位元素不总是被父级的overflow元素裁剪，尤其是当overflow在绝对定位元素及其包含块之间的时候。
124.如果overflow的属性值不是hidden而是auto或者scroll，即使绝对定位元素宽高比overflow元素还大，也都不会出现滚动条。
125.position：fixed元素的包含块是根元素。
126.absolute与clip，clip属性要想起作用，元素必须是绝对定位或者固定定位。clip属性的两个无法替代的使用场景：
a.fixed固定定位裁剪，对于position:fixed元素由于其包含块是根元素，无法通过overflow属性裁剪，这时需要运用clip属性来进行裁剪。clip用法，rect(top right bottom left)
b.最佳可访问性隐藏，<a href="/"   class="logo"><h1>CSS世界</h1></a> 如果设置h1 { position:absolute ;  clip:rect(0 0 0 0)}此时可以隐藏文字，但是还是能被屏幕阅读设备捕捉到。
127.在chrome浏览器下，clip属性仅仅是决定那一部分是可见的，对于原来占据的空间并无影响。非可见部分无法响应点击事件
128.absolute的流体特性，具有流体特性的前提条件是，对立方向同时发生定位，如同时设置left、right 。 top 、bottom 等。
129.绝对定位元素的margin:auto的填充规则和普通流体元素是一样的：
a.如果一侧定值，一侧auto，auto为剩余空间大小。
b.如果两侧均是auto, 则平分剩余空间。
130.relative对absolute的限制，虽然relative、absolute和fixed都能对absolute产生限制。但是只有relative可以让元素保持在正常的文档流中。
131.relative定位的两大特性
a.相对自身，relative在使用top , left等定位属性在进行定位时，是相对与自身原来的位置进行定位。
b.无侵入，当relative进行定位偏移的时候，一般情况下不会影响周围元素的布局。
132.relative的top,left,right，bottom百分比值是相对与包含块计算的。位移是相对与自身原来的位置。如果包含块的高度是auto，则计算值为0，偏移无效。
133.relative的top,bottom，left和right同时生效时是"你死我活"的表现，由于文档流默认从左至右，从上到下，所以bottom和right属性会失效。
134.relative最小化影响原则:
a.尽量不使用relative, 如果想定位某些元素，看看能否使用"无依赖绝对定位".
b.如果场景受限，一定要使用relative， 则该relative务必最小化（影响最小化，将需要定位的元素，用div包含起来，使用position：relative）。
135.position:fixed, fixed的包含块是根元素，relative对fixed定位没有任何限制作用。和无依赖绝对定位一样，也存在无依赖固定定位。
136.css世界的层叠规则，影响css层叠顺序的元素有很多，并不止z-index。
137.层叠顺序：从低到高依次为：层叠上下文background border ->负z-index ->block块状水平盒子->float浮动盒子->inline水平盒子->z-index:auto或者z-index:0 不依赖z-index的层叠上下文->正z-index
138.层叠准则：
a.谁大谁上:当具有明显的层叠水平标识的时候，在同一个层叠上下文领域，层叠水平值大的覆盖层叠水平值小的
b.后来居上：当元素的层叠水平一致，层叠顺序相同的时候，在DOM流处于后面的元素会覆盖前面的元素。
139.层叠上下文的特性：
a.层叠上下文的层叠水平比普通元素要高。
b.层叠上下文可以阻断元素的混合模式。
c.层叠上下文可以嵌套，内部层叠上下文及其子元素均受制于外部层叠上下文
d.每个层叠上下文和兄弟元素独立，也就是说，当进行层叠变化或渲染的时候，只需要考虑后代元素。
e.每个层叠上下文都是自成体系的，当元素发生层叠的时候，整个元素被认为是在父层叠上下文的层叠顺序中。
140.层叠上下文的创建：
a.页面根元素天生具有层叠上下文
b.z-index值为数值的定位元素。对于position值为relative/absolute以及firefox下含有position:fixed声明的定位元素，当其z-index值不是auto的时候，会创建层叠上下文。
c.元素为flex布局元素(父元素为display:flex或者inline-flex）,同时z-index不为auto
d.元素具有opacity属性但值不是1
e.元素的transform值不是none
f.元素的mix-blend-mode不是normal
g.元素的filter属性值不是none
h.元素的isolation值是isolate
i.元素的will-change属性值是c-h的任意一个(例如:will-change:opacity)
j.元素的-webkit-overflow-scrolling设为touch
      以上如果层叠上下文元素不依赖z-index数值，则其层叠顺序是z-index:auto，可看成是z-index：0级别
141.z-index不犯二原则，对于非浮层元素，避免设置z-index值，z-index值没有任何理由会超过2。
142.在css中1em的计算值等同于当前元素所在的font-size计算值，可以想象成当前元素中汉字的高度。
143.font-size的关键字属性值，：
a.相对尺寸关键字，指的是相对于当前font-size计算:larger大一点，smaller小一点
b.绝对尺寸关键字, xx-large(好大好大，与h1相当) ， x-large(好大，与h2相当) ， large(大，与h3相当) ， medium（中，与h4相当），small（小，与h5相当），x-small(好小，与h6相当)， xx-small(好小好小)
144.当font-size设定的值如果小于12px会被当成12px来处理，但是有一个值例外就是0, 当font-size为0时此时就是0px
145.font-family的默认值由操作系统和浏览器共同决定，例如windows和OS X下chrome浏览器的字体不一样，同一台window系统的chrome和firefox浏览器的默认字体不一样。
146.font-family的值可以字体族和字体名，如果是字体名需要用双引号包起来。也可以指定多个值，如果是字体族则需要用逗号隔开，如果字体族是serif和sans-serif一定要写在最后，因为在大多数浏览器下，后面的所有的字体会被忽略
147.字体单位ch和em ex rem 一样是一个相对单位，和ch相关的是字符0, 1ch表示的是一个字符0的宽度。
148.font-weight支持的值：
a.normal , bold 平常用的 
b.lighter ， bolder相对与父级元素。
c.精准控制：100~900
149.font-style支持的值：
a.normal
b.italic
c.oblique
      其中italic和oblique都表示斜体的意思，区别在于，oblique是单纯的让文字倾斜，italic是使用当前字体的斜体字体。如果当前字体没有斜体字体，则解析为oblique。
150.font-variant具有两个属性值，normal和small-caps ， 其中small-caps就是可以让英文字符表现为小体型大写字母。
151.font属性值的格式为：[font-style || font-variant ||  font-weight ] ? font-size [ / line-height] ? font-family , 其中font-size和font-family是必须的。font会破坏部分属性属性的继承性， 例如 .font { font: 400 30px 'Microsoft Yahei'} ,line-height会被重置为normal。 font缩写不能使用inherit等全局关键字。
152.font使用关建字，caption | icon | menu | message-box | small-caption | status-bar。如果将font属性设置为其中的一个值，就等同于设置font为操作系统该部件对应的font
153.