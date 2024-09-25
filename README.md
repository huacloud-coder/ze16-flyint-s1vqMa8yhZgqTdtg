[合集 \- .net(2\)](https://github.com)[1\.CRUD最佳实践PasteForm及项目模板的制作视频，让重复的CRUD更加简单直接\[附带源码和视频](三)09\-09](https://github.com/pastespider/p/18404029)2\.PasteForm最佳CRUD实践，实际案例PasteTemplate详解(一)09\-24收起
本文将介绍soft.pastecode.cn出品的PasteForm，PasteForm是贴代码使用Dto思想实现的CRUD的一个组件，或者说输出一个思想！
为啥我觉得是最佳的CRUD呢？先结合你的实际项目解答下以下问题：


1\.如果有一个系统，有100个表，你的管理端需要多少页面？别和我说100个表很多，需求复杂点的随随便便上100个数据库表的！


2\.新的需求下来了，说XX表要新增一个字段，默认为100，新增的时候输入，不支持修改，作为你目前的管理端你需要几个步骤来实现？关键点你如何做版本过渡的问题？


3\.做开发的都是到，表和表之间一般是有关系的，虽然不一定会建立外表链，比如BindUserGrade的表中的UserId一般就是表示UserInfo表的Id,这就牵扯到录入，更新，和显示的问题了，其实我们希望看到的是显示UserInfo中的UserName而不是干巴巴的一个UserId对吧！


4\.一个表我们在管理端的时候一般会涉及到数据的列表显示，比较常用的就是table表格对吧，对应的有新增，编辑，查看，删除，那作为管理端你如何控制他们的权限？权限往往涉及2大块，a.前端页面的显示否，b.后端接口中的权限判定


... .. .


让我们一起来看下PasteForm的案例项目PasteTemplate是如何处理以上问题的！


案例代码 [https://gitee.com/pastecode/paste\-template/tree/master/example/PasteTemplate](https://github.com):[楚门加速器p](https://tianchuang88.com)
从上面下载代码后，直接启动，然后，应该是localhost:22222端口 [http://localhost:22222/page/index.html](https://github.com)


## 登陆页面


![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924213101595-1466159162.png)
登陆页面没什么好说的，因为不涉及找回密码，注册等，只有图形验证码和登陆


## 管理端


![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924213204340-1227556118.png)
登陆后，看到如上图
左侧菜单采用动态的模式，菜单由权限读取，系统在首次启动的时候会把默认的菜单写入到数据库，如果当前账号有root\-root的权限，则会返回所有的菜单，也就是root\-root表示超级权限的意思,拥有系统的所有权限！
由于其他表的信息比较简单，我就从测试表的数据来举例子
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924213517730-1396323770.png)


### 页面组成


PasteForm的整个体系其实只有4个页面，
1\.上图的index.html页面，作为管理端的菜单页面
2\.右侧区域的数据表格table显示和他上方的搜索区域组成的pasteform/index.html页面
3\.对应的表的编辑和新增页面，新增和编辑是同一个页面，这里命名为pasteform/view.html，也就是form表单页面
4\.有些时候表的数据多大，我们不希望在table中全部显示，比如博文的正文，这就需要有一个页面查看详细的，pasteform/detail.html


#### index.html


管理端的主页面，大概是左右布局，100%绝对定位，是为了满足右侧子页面的flex布局，主要点是左侧的tree，内容是从API读取的，也就是不同账号登陆后看到的菜单是不一样的，具体的看角色对应的菜单(权限)


### pasteform/index.html


![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924214044199-169793676.png)
如上图，你看到的内容，可以说整个页面都是后端的Dto控制的
1\.比如搜索区域有多少个搜索项，搜索项的交互(daterange,date,outer,select,selects,word等)
2\.中间区域的表格，包含了新增，编辑，删除，详情的基本4个菜单，然后就是自定义的一些按钮，比如上图的 添加子集!
3\.然后是表格区域内的排序，注意看表格header中的ID 排序 层级 其实哪些字段支持排序，在后端的Dto中也就一行代码的事情！
4\.表格中的一些交互，比如上图的switch,点击后是可以直接修改的，操作后会和后端API进行交互，如果返回非200则会重置状态
5\.按钮区域的提交按钮，有些时候表格的按钮需要条件判断，那也是支持的，比如age\>18的显示按钮1，age\<12的显示按钮2等！
6\.表格的自定义显示，有时候我们希望一个表格显示多个字段，比如换行啥的，也是可以实现的


### pasteform/view.html


表单主页面，作为新增或者编辑的页面，支持几乎常见的录入
1\.基础录入包括默认值等，比如text,number,switch,select等
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924214752110-214131187.png)


2\.也支持特殊的格式，比如daterange,richtext,textarea,selects,image,file等
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924214727244-1465778945.png)


3\.高级支持,外表输入，比如选择其他表的一个对象作为这个表单的一个录入
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924214925787-414775336.png)
点击父级后面的输入框，是弹出一个页面，选择一个对象，作为这个输入框的值，值一般包含2个，一个是id，一个是显示的
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924215021261-1388329446.png)


4\.高级支持，参数输入,比如权限列表中的添加子集，那么打开的应该是添加页面，这个时候父级是不要输入的，由url参数传递过去
![image](https://img2024.cnblogs.com/blog/3266034/202409/3266034-20240924215142802-977466140.png)


还有更多的功能等你发觉，上面说得一大堆功能，其实都是Dto中配置的
啥叫Dto?
接触过ABPvNext的应该比较数据，其实就是不同的Model,比如UserInfo这个表，一般会创建对应的4个Dto
UserInfoAddDto:作为UserInfo新增的数据模型
UserInfoUpdateDto:作为UserInfo的编辑的数据模型
UserInfoDto:一般作为详细显示的数据模型
UserInfoListDto:一般作为表格，列表的数据展示用
这几个模型可以配置互转，就是使用AutoMapper!


为了实现更加灵活的CRUD，我们只需要在对应的Dto中的字段上添加对应的属性即可，由于需求比较特殊，我定义了一个属性



```
    /// 
    /// 前端用数据类型 
    /// 
    [AttributeUsage(AttributeTargets.Property | AttributeTargets.Class, AllowMultiple = true)]
    public class ColumnDataTypeAttribute : Attribute
    {
        /// 
        /// image region navigator select selects dateplan datetime等
        /// 
        public string Name { get; set; } = "";

        /// 
        /// 
        /// 
        public string Args1 { get; set; } = "";

        /// 
        /// 
        /// 
        public string Args2 { get; set; } = "";

        /// 
        /// 参数3
        /// 
        public string Args3 { get; set; } = "";

        /// 
        /// 参数4
        /// 
        public string Args4 { get; set; } = "";

        /// 
        /// 
        /// 
        public string ErrorMessage { get; set; } = "";

        /// 
        /// 按照要求文档填写参数
        /// 
        /// 
        /// 
        /// 
        /// 
        /// 
        /// 
        public ColumnDataTypeAttribute(string name = "", string args1 = "", string args2 = "", string args3 = "", string args4 = "", string ErrorMessage = "")
        {
            this.Name = name;
            this.Args3 = args3;
            this.Args1 = args1;
            this.Args2 = args2;
            this.Args4 = args4;
            this.ErrorMessage = ErrorMessage;
        }

        /// 
        /// 
        /// 
        /// 
        public override string ToString()
        {
            return string.Format("{0}-{1}-{2}-{3}-{4}", Name, Args1, Args2, Args3, Args4);
        }
    }


```

然后是在需要限定的字段添加这个或者多个这个属性，比如



```
    ///
    ///测试表 用于测试CURD的表
    ///
    public class TestTableAddDto
    {
        ///
        ///姓名 模拟短文本输入
        ///
        [MaxLength(32)]
        [Required]
        [PasteMark("test", "name")]
        public string Name { get; set; }

        ///
        ///头像 模拟图片上传
        ///
        [MaxLength(128)]
        [PasteMark("test", "head")]
        [ColumnDataType("image", "1", "head", "60*60")]
        public string Head { get; set; }

        ///
        ///年龄 模拟输入number
        ///
        [Range(5, 90, ErrorMessage = "年龄必须限定在5~90之间!")]
        public int Age { get; set; }

        ///
        ///文本区域 模拟文本区域的输入
        ///
        [MaxLength(128)]
        public string Desc { get; set; }

        ///
        ///富文本 模拟富文本，前端HTML的是使用wangEditv5
        ///
        public string Blog { get; set; }

        ///
        ///加入日期 模拟必填时间的输入
        ///
        public DateTime JoinDate { get; set; }

        /// 
        /// 文件
        /// 
        [ColumnDataType("file", "/api/app/Upload/UpImage?type=file")]
        public string Fpath { get; set; } = "";

        ///
        ///单选 一般表示状态，内定的，有点像Enum,关于Enum后续会支持
        ///
        [ColumnDataType("mark", "test", "datetype")]
        [ColumnDataType("select", "[{\"name\":\"日类型\",\"value\":0},{\"name\":\"月类型\",\"value\":1},{\"name\":\"年类型\",\"value\":2}]")]
        public int DateType { get; set; }

        /// 
        /// 复选 多个之间用逗号隔开
        /// 
        [ColumnDataType("selects", "[{\"name\":\"日类型\",\"value\":\"day\"},{\"name\":\"月类型\",\"value\":\"month\"},{\"name\":\"年类型\",\"value\":\"year\"}]", ",")]
        public string TypeStrs { get; set; }

        /// 
        /// 复选数组
        /// 
        [ColumnDataType("selects", "[{\"name\":\"日类型\",\"value\":\"day\"},{\"name\":\"月类型\",\"value\":\"month\"},{\"name\":\"年类型\",\"value\":\"year\"}]")]
        public string[] Types { get; set; }

        ///
        ///角色ID 这里一般用于外表，就是选择其他表的一个数据作为输入内容
        ///
        [ColumnDataType("outer", "gradeInfo", "", "id", "name")]
        public int GradeId { get; set; }

        ///
        ///成绩 模拟前端限定2位小数
        ///
        public double Score { get; set; }

        /// 
        /// 字符串多图
        /// 
        [ColumnDataType("image", "3", "img", "60*60")]
        public string Img2 { get; set; }

        /// 
        /// 数组图片
        /// 
        [ColumnDataType("image", "3", "img", "60*60")]
        public string[] Img3 { get; set; }

        /// 
        /// 会员周期 会员生效区间
        /// 
        [ColumnDataType("daterange", "dateStart", "dateEnd")]
        public DateTime DateStart { get; set; } = DateTime.Parse("2024-09-01 00:00:00");

        /// 
        /// 会员周期 会员生效区间
        /// 
        [ColumnDataType("hidden")]
        public DateTime DateEnd { get; set; } = DateTime.Parse("2024-10-01 00:00:00");

    }

```

看上面的是测试表的Dto的属性的写法，由于配置ColumnDataType属性有多个参数，为了防止写错，你也可以自定义，然后继承于ColumnDataType就行了，比如我的



```
 /// 
 /// 
 /// 
 public class PasteMarkAttribute : ColumnDataTypeAttribute
 {

     /// 
     /// 书签属性
     /// 
     /// 书签模块,示例product
     /// 书签项,示例code
     public PasteMarkAttribute(string _model, string _value)
     {
         base.Name = "mark";
         base.Args1 = _model;
         base.Args2 = _value;
     }
 }

 /// 
 /// 日期区间
 /// 
 public class PasteDaterangeAttribute : ColumnDataTypeAttribute
 {

     /// 
     /// 日期区间
     /// 
     /// 开始日期字段
     /// 结束日期字段
     public PasteDaterangeAttribute(string _sdate="sdate",string _edate="edate")
     {
         base.Name = "daterange";
         base.Args1 = _sdate;
         base.Args2 = _edate;
     }
 }


 /// 
 /// 
 /// 
 public class PasteLeftAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// 
     /// 
     public PasteLeftAttribute()
     {
         base.Name = "class";
         base.Args1 = "fleft";
     }
 }

 /// 
 /// 
 /// 
 public class PasteSwitchAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// 
     /// 
     public PasteSwitchAttribute()
     {
         base.Name = "switch";
     }
 }

 /// 
 /// 同hidden 表示在UI中隐藏这个对象
 /// 
 public class PasteHiddenAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// 
     /// 
     public PasteHiddenAttribute()
     {
         base.Name = "hidden";
     }
 }

 /// 
 /// 配置菜单或者是条件菜单
 /// 
 public class PasteMenuAttribute : ColumnDataTypeAttribute
 {

     /// 
     /// 
     /// 
     /// 
     /// 
     /// 
     /// 
     public PasteMenuAttribute(string _name, string _script, string _iconfont = "", bool _box = false)
     {
         base.Name = "menu";
         base.Args1 = _name;
         base.Args2 = _script;
         base.Args3 = _iconfont;
         if (_box)
         {
             base.Args4 = "box";
         }
     }
 }


 /// 
 /// 配置菜单或者是条件菜单
 /// 
 public class PasteIfMenuAttribute : ColumnDataTypeAttribute
 {
     //[ColumnDataType("ifmenu", "item.isReady", "[重置状态](\"javascript:;\" "\\"如果之前创建失败，修改信息后，可以重置，重置后可以重新创建\\"")", "", "")]
     /// 
     /// 条件按钮
     /// 
     /// 
     /// 
     /// 是否在按钮盒子中
     public PasteIfMenuAttribute(string _expresion, string _insertHtml, bool _box = false)
     {
         base.Name = "ifmenu";
         base.Args1 = _expresion;
         base.Args2 = _insertHtml;
         //base.Args3 = _iconfont;
         if (_box)
         {
             base.Args3 = "box";
         }
     }
 }


 /// 
 /// 用于表格中，显示另外一个字段的某一个值的表达式
 /// 
 public class PasteOuterDisplayAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// 表格中用于显示某一个字段
     /// 
     /// 示例extendService?.name || ''
     public PasteOuterDisplayAttribute(string _expression)
     {
         base.Name = "outerdisplay";
         //base.Args1 = _cloumnName;
         base.Args2 = _expression;
     }
 }

 /// 
 /// 
 /// 
 public class PasteDisableAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// 
     /// 
     /// 禁用新增
     /// 禁用编辑
     /// 禁用删除
     public PasteDisableAttribute(bool _unadd = false, bool _unedit = false, bool _undel = false)
     {
         base.Name = "disable";
         if (_unadd)
         {
             base.Args1 = "add";
         }
         if (_unedit)
         {
             base.Args2 = "edit";
         }
         if (_undel)
         {
             base.Args3 = "del";
         }
     }
 }

 /// 
 /// 单位属性 为计量添加单位
 /// 
 public class PasteUnitAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// Unit的属性
     /// 
     /// 示例元
     public PasteUnitAttribute(string _unit)
     {
         base.Name = "unit";
         base.Args1 = _unit;
     }
 }


 /// 
 /// Select的属性
 /// 
 public class PasteSelectAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// Select的属性
     /// 
     /// 示例[{name,value,selected}]
     public PasteSelectAttribute(string _selectvalues)
     {
         base.Name = "select";
         base.Args1 = _selectvalues;
     }
 }

 /// 
 /// Selects的属性 可以多选的
 /// 
 public class PasteSelectsAttribute : ColumnDataTypeAttribute
 {
     /// 
     /// Selects的属性
     /// 
     /// 示例[{name,value,selected}]
     public PasteSelectsAttribute(string _selectvalues)
     {
         base.Name = "selects";
         base.Args1 = _selectvalues;
     }
 }

 /// 
 /// Image的属性
 /// 
 public class PasteImageAttribute : ColumnDataTypeAttribute
 {

     /// 
     /// Image的属性
     /// 
     /// 可以传多少张
     /// 存放的路径
     /// 转化的大小,示例60*60,1920*0
     public PasteImageAttribute(int _num = 1, string _path = "", string _size = "")
     {
         base.Name = "image";
         base.Args1 = _num.ToString();
         base.Args2 = _path;
         base.Args3 = _size;
     }
 }

 /// 
 /// 
 /// 
 public class PasteOuterAttribute : ColumnDataTypeAttribute
 {

     /// 
     /// 链接外表显示和输入用
     /// 
     /// 外表名称，示例serviceInfo
     /// 显示的时候的表达式，示例extendService.name || ''
     /// 回传的列的名称，示例id
     /// 显示的列表的名称，示例name
     public PasteOuterAttribute(string className, string extendName = "", string _keyColumn = "id", string _showColumn = "name")
     {
         base.Name = "outer";
         base.Args1 = className;
         base.Args2 = extendName;
         base.Args3 = _keyColumn;
         base.Args4 = _showColumn;
     }
 }



```

案例有了，那么每个name是干嘛用的？


# ColumnDataTypeAttribute


# ColumnDataType的配置说明


## image


相对于后面的 head 来说，这里是大图模式,在ListDto中表示使用图片的模式渲染




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 数字 | 1 | 图片数量 |
| args2 | 字符 | cate | 存放在什么位置，上传图片的时候会附带到参数type中 |
| args3 | 字符 | 60\*60 | 图片是否需要压缩，压缩的宽高，不压缩的设置为0,比如60\*0 |
| args4 | 字符 | small | small,normal,big表示图片的大小三个规格，默认normal，如果要返回格式，则由dataFrom决定 |


## head 弃用，使用image


使用方式同 image 这里表示的是小图标模式




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 数字 | 1 | 图片数量 |
| args2 | 字符 | cate | 存放在什么位置，上传图片的时候会附带到参数type中 |
| args3 | 字符 | 60\*60 | 图片是否需要压缩，压缩的宽高，不压缩的设置为0,比如60\*0 |
| args4 | 字符 | arr或str | 默认值str对应字段的类型，是array类型还是string类型，如果是string类型多个之间用,隔开 |


## file


其实可以使用image的接口，他们2个的返回格式都是一样的，wangEditor的返回格式，主要是UI上不一样，毕竟文件没法预览




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | /api/app/Upload/UpImage | 表示上传的路径，默认是/api/app/Upload/UpFile,你也可以自己修改他 |


## region


小程序中的地区选择，可以配置精确度，到区还是到县




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | region | 表示地址选择的层级，可选region和sub\-district |
| args2 | 字符 | str | 可用值str或者arr 表示返回的数据类型,str的时候用,隔开 |


## outer


表示一个值需要从外表获取，编辑的时候如何显示? 比如fatherId,extendRole




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | cateInfo | 外表的名称，对应模板的path,或者路径，路径一定附带了/字符示例./abc.html |
| args2 | 字符 | extendCates | 表示显示的数据，需要和下面2个配合，是一个当前的扩展，目标数组要配置hidden |
| args3 | 字符 | id | 获取返回对象的属性，一般为id |
| args4 | 字符 | name | id的友好名称显示,这里指的是外表，比如cateId，需要打开catelist页面，选择后，返回cate,则name作为友好显示,id作为实际值 |


## outers


outer的复数版本，表示可以从外部列表中选择多个，比如在创建账号的时候给他绑定多个角色，就用这个！




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | cateInfo | 外表的名称，对应模板的path,或者路径，路径一定附带了/字符示例./abc.html |
| args2 | 字符 | extendCates | 表示显示的数据，需要和下面2个配合，是一个数组，目标数组要配置hidden |
| args3 | 字符 | id | 获取返回对象的属性，一般为id |
| args4 | 字符 | name | id的友好名称显示,这里指的是外表，比如cateId，需要打开catelist页面，选择后，返回cate,则name作为友好显示,id作为实际值 |


## outerdisplay


ListDto中用于外表的显示，比如有字段cateInfoId,对应的ExtendCateInfo要标记为outerdisplay,args2配置为extendCateInfo?.name \|\| '',否则会显示为\[object object]




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | cateInfoId | 表示这个字段的值，一般不显示 |
| args2 | 字符 | extendCateInfo?.name \|\| '' | 表示显示的名称，友好名称，需要后端支持,在前端会处理成.display |


## navigator


表示这个值需要使用页面从另外一个列表中获取，这里表示小程序端的,建议使用outer outerdisplay




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | cate.name \|\| '' | 表示显示cate.name或者空这个一般用于ListDto中 |
| args2 | 字符 | cateInfo | 外表的名称，对应模板的path，可以为空 |
| args3 | 字符 | /pages/cate/index/?model\=select | 如果对应的表不用模板，则直接表示路径 |


## datetime


默认的yyyy\-MM\-dd HH:mm:ss的格式




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | yyyy\-MM\-dd HH:mm:ss | 表示时间使用的格式 |


## hidden


表示隐藏这个字段，一般是主键ID，或者外表链接过来的会这配置，比如需要给cate添加子项，则添加由cate那边过来
这个也适用于ListDto




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | bind | 如果不填表示隐藏，如果填写了表示页面的query中model\=xxx的时候不隐藏,表示非xxx的时候隐藏，xxx的时候不隐藏 |


## password


密码框模式


## query


如果从query中获取到了数据，则对应的字段隐藏,注意不要配置hidden,因为查询的时候是从表单读取数据的，hidden的话是不写入到form中的，那样查询就没法获取query来的值了




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | cateid | 表示使用url中的哪个参数读取值 |


## readyonly


表示这个字段是只读的，一般是编辑的时候生效


## richtext


如果是字符串，没有设置maxlength，默认就会变更成richtext，也可以手动强制配置


## textarea


如果是字符串，设置maxlength，且设置的值大于128，默认就会变更成textarea，也可以手动强制配置


## text


对于一些长度过长的，会被判定为textarea，或者richtext的可以使用这个强制换行成text


## fentoyuan


这个是指后台单位为分，又需要前端进行元输入的，注意小数位数为2位小数，也就是分的格式为int


## select


表示单选，比如权限类型，一般是指固定类型的，一般不修改的那种情况，也可以表示状态等




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | \[{"name":"大","value":"1"},{"name":"小","value":"2"}] | 表示单选的可选值，name是显示 value是值 |
| args2 | 字符 |  | 表示值得类型，这里是单个，跟随主类型走 |


## selects


表示多选，这个表示的是页面上的多选，需要列表显示，然后是可以多选！关键在于最后读取数据的时候，需要判断是什么格式的！




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | \[{"name":"大","value":"1"},{"name":"小","value":"2"}] | 表示单选的可选值，name是显示 value是值 |
| args2 | 字符 | , | 如果值类型不是数组，则返回字符串，用这个字符拼接,也就是分割字符 |


## sort


表示排序，表示字段的顺序,一般表格比较会使用这个




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 数字 | 0 | 小的排在前面，默认为0 |


## link


这个用于表格显示，一般表示用于显示外表的数据,这个将弃用，使用outerdisplay替换




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | extendCate.name | 显示的外表链接，示例extendCate?.name \|\| ''表示显示cate.name或者空这个一般用于ListDto中 |


## width


表示这个字段在表格得宽度，可以为\*或者对应得数字,是表格得列的宽度的权重,这个适用于ListDto




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | 60 | 表示这个列的宽度，可以为数字也可以是\*比如3\*这样 |


## orderby


表示基于哪个字段进行排序,这个一般是ListDto表示表格中，可以基于哪个字段进行排序查询




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | id | 表示使用id正序排序 |
| args2 | 字符 | id desc | 表示使用倒叙排序 |


## datalist


前端表示使用datalist作为选择数据源，前端需要把datalist的id赋值给datalistid，默认为字段name,这个规则适用于表单和QueryDto




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | \[{"name":"正常","value":"1","selected":true}] | JSON的字符串，也可以为空 |
| args2 | 字符 | data\_pro | 表示调用哪个datalist数据，表示datalist的id,和args1互斥 |
| args3 | 字符 | read\_pro\_datalist() | 表示需要使用eval执行哪个函数，一般和args2配合使用，和args1互斥 |


## daterange


主字段需要设置为daterange,其他字段需要设置hidden,在最后组合数据的时候，会基于参数生成对应的,应该要设置为可null格式




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | sdate | 表示开始时间，最后会传送yyyy\-MM\-dd 00:00:00的格式数据 |
| args2 | 字符 | edate | 表示结束时间，如果你选择2024\-08\-31,最后上送的会是2024\-08\-31 00:00:00 |
| args3 | 字符 | yyyy\-MM\-dd 00:00:00 | 表示时间的格式化，默认使用yyyy\-MM\-dd 00:00:00 |


## disable


特殊限定，限定于class的，表示禁用这个模型的哪些功能，这个一般用于ListDto，因为这些功能都在列表页面




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | add | 表示忽略新增，也就是不显示新增按钮 |
| args2 | 字符 | edit | 表示忽略编辑，表示列表中没有编辑的这个选项,有些数据只能看，不需要新增和编辑 |
| args3 | 字符 | del | 表示这个表没有删除，页面UI中不需要删除按钮 |


## menu


用于列表后面的菜单 比如[*\\args1*](https://github.com)




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | 编辑 | 按钮的名称 |
| args2 | 字符 | open\_view(\<%:\=item.id%\>); | onclick事件的代码 |
| args3 | 字符 | Hui\-iconfont\-menu | 样式名称 |
| args4 | 字符 | box | 默认不写，如果写box表示要方入到menubox中 |


## ifmenu


基于条件模式的菜单，使用于Listdto中
示例: \[ColumnDataType("ifmenu", "item.age\=\=8", "[条件2", "box")]
数据对象就是item表示当前这行数据](https://github.com)




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | isReady \=\= true | if的表达式中()内的内容 |
| args2 | 字符 |  | 这里填写条件后的菜单代码，注意引用需要使用{ |
| args3 | 字符 | box | 默认不写，如果写box表示要方入到menubox中 |


## htmltemplate


已抛弃，使用下方的html替代,如果要显示自定义的表格，则使用这个，表示直接显示信息模板注意里面的带参使用{{:\=item.name}}这样的形式




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | {{:\=item.name}}{{:\=item.desc}} | 需要在td中显示的html代码，支持模板的写法 |


## html


如果要显示自定义的表格，则使用这个，表示直接显示信息模板注意里面的带参使用{{:\=item.name}}这样的形式




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | {{:\=item.name}}{{:\=item.desc}} | 需要在td中显示的html代码，支持模板的写法 |


## linkquery


表示把某些参数穿透给子页面，比如服务\-\>文件，则可以在服务A页的时候查看文件列表B页，在文件列表中可以直接新增文件C页,这个时候我们希望对应的服务ID直接由参数传入
也就是在打开C的时候，希望把B的参数传递给C,注意这个linkquery是放在ListDto中的Class上的，也就是和disable一样




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | 需要传递的参数 | 多个之间用,隔开，比如serviceid,ftype |


## object


适用于表单页面，表示从另外一个表单新增数据，比如用户的收货地址，在表单的时候，打开一个新的表单，进行信息填写后，返回一个obj，这个时候是没有写入数据库的,所以在返回的时候需要显示
和outer有点像，不过回传的是一个object类型！如果是编辑的时候？需不需要上传到API表示编辑了?打开表单的时候会传递model\=object这个参数过去，表示叫子表单不要做API入库操作
注意这个子模型也是需要建立对应的API的，不过不需要建立新增和编辑的接口，因为被上一级涵盖了！




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | roleInfo | 一般使用path也可以使用页面的路径 |
| args2 | 字符 | id | 新增的时候无用，主要是编辑的时候，基于这个id和path去数据库查询新的数据 |
| args3 | 字符 | name | 表示显示的是这个object的哪个字段，一般在编辑的时候可见 |


## objects


object的复数版本，表示一个集合，比如一个会员有多个爱好，新增的时候，打开子表单，填写多个爱好的object信息体返回,在显示的时候，如果一个字段不足以显示???是否支持多个字段联动显示?




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | roleInfo | 一般使用path也可以使用页面的路径 |
| args2 | 字符 | id | 新增的时候无效，编辑的时候表示从数据库查询信息，也作为删除的key使用 |
| args3 | 字符 | name | 表示显示的是这个object的哪个字段，一般在编辑的时候可见 |


## mark


字段详细说明，或者是文档，这里是做一个描点，需要使用公共函数自己实现具体的要求，一般的是项目下面的模块的某一个字段的说明，一个项目往往这个是一样的，然后就是哪个模块的哪个字段，所以有2个字段
会在title后面生成一个span class\="tapmark" onclick\="global\_tap\_mark(\_modek,\_code)";




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | product | 一般作为模块使用，比如商品的这个模块 |
| args2 | 字符 | code | 模块下的一个字段的说明，比如code这个字段的用法，案例说明等 |


## template


作为有些表的特殊布局，就是自定义布局表格部分的内容，包括表头和表身2个部分,注意需要实现选择的功能，除非这个表用不到选择这个功能
文件存放于pasteform/template.html，就只有一个template.html页面，里面的都是模板代码




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | template\_user\_head | 表格的表头部分的script的id |
| args2 | 字符 | template\_user\_body | 表格的表身部分的模板的script的id |


## style


针对一些特殊的设定，比如textarea，有些地方我们要设置高一些




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | height:500px; | 就是style里面的内容 |


## class


针对一些自定义的class，可以通过这个来配置




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | btngo | 表示这个对象要添加的class |


## button


用于表单中，的按钮，用意是让用户点击后触发函数,global\_form\_button\_click(this,className,name);
注意按钮的value为当前字段的value，比如当前页面的path\=nginxRoute



```
        /// 
        /// Http模板 表示使用Http模式的模板的内容
        /// 
        [ColumnDataType("button")]
        public string Temp1 { get; set; } = "点击导入";

```

则上面会生成的页面内容为



```
Http模板：点击导入

```

点击导入点击后，会调用global\_form\_button\_click(this,'nginxRoute','temp1');
注意由于返回的首单次会小写，所以是temp1而不是Temp1


## unit


用于显示单位，比如MB，KG，个等，一般在Int Long Double等Number地方后显示,也可以作为表格中显示?




| 字段 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| args1 | 字符 | MB | 表示这个对象的单位 |


## 待处理


1\.列表中可编辑的对象 比如switch?
handler\_switch\_change(elc){
// className columnName id state
}


2\.外表选择的时候，是否支持多选模式?
多选模式的时候，如何加载已经选中的，已经选中的如何删除?难不成要搞一个选定区域?


3\.Int32\[] Int64\[] String\[]
这种数据的显示和最后更新


4\.表单数据的提交前的数据格式的校验！


5\.daterange如何设定
单个变成多个！


6\.有些数据不需要新增，和编辑 ，虽然可以使用API控制，但是在前端有这么个按钮存在，也是很突兀的，所以得处理


7\.管理员拥有多个角色，这个多个角色的显示和编辑问题！
selects 可以设定返回多个
list设定显示的方式 关键在于如何每次编辑的时候不是覆盖！还是写一个单个删除的接口?


8\.enum类型的显示问题？其实就是enmu变种到attribute的select!!!


9\.从角色绑定权限来说，角色列表，点击权限(给这个角色绑定权限),打开的权限列表是所有的，需要绑定的勾选，不要的取消勾选
model\=bind 则有一些字段这个模式下才显示
hidden.args1表示某一个模式，如果为空表示都隐藏


### 待确认问题


1\.必填，如果是int的类型的，是不是只能非0，还是不生效?


2\.对于menu的支持，参数因该是{{}}控制的，而不是\<%%\>


3\.enum类型的返回，是返回值，还是对应的名称?是否支持enum转select？


4\.有时候表单只是为了新增一个数据，不入库那种！outform model\=outform 返回组合的 ...待定


5\.比如查看某一个列表\-\-子项列表\-\-子项添加
linkquery cateid,abcid


6\.单位转化，比如后台是分，前端需要输入元 那么就是在输入的时候是元，显示是元 提交的时候是分！！！
fentoyuan 分转元 默认2位小数点


7\.关于是否需要显示详情页面的问题，因为有时候表格数据是显示不全的，比如richtext，这个时候可能需要一个detail.html页面
如果要呈现，则需要对应的模型Dto的属性,生成器默认是去除的，则需要自己在Domain中添加/template/dto.html作为生成模型替换生成器默认的模型！
页面的呈现上和表单类型，把输入框等渲染成直接显示的即可！
然后再由ListDto中，添加菜单detail的支持！


8\.在列表页面中，如果一个对象被标记为query,如果从参数中获取到了值，则这个查询项进行隐藏操作


9\.如果列表页面的当前状态为model，则上方的搜索区域默认隐藏


10\.关于引入字段说明的事情，比如一个表单100个字段，各个字段表示什么意思，一般的做法是点击对应的？然后弹出或者是跳转到某一个站点，显示这个介绍
mark由此而来


11\.是否加入自模型，就是表格数据展示的样式，是否引入自定义的模板？？？
默认是使用内置的二次模板，如果是引用外部的化，则需要整个，也就是表头和表身都需要！
需要外部写一个html的template页面，然后使用Jquery的load加载进去！
在ListDto中，使用template进行标注，标注args1为表头样式的模板 args2为表身部分的内容


12\.添加对自定义样式的支持 style class


13\.添加对menubox的支持，针对很多按钮的情况


14\.表单中需要特意操作的，添加函数支持 把表单的数据传递过去 由自定义函数决定执行结果!global\_form\_action(elc,className,name,location.search);
项目路由中的 载入模板 模板测试等！


15\.搞一个占位的？有时候的排版等，需要空开一个位置


16\.可以搞一个属性继承于基础属性，这样在输入的时候会好些，比如PasteMarkAttribute:ColumnDataTypeAttribute;


17\.窗口的弹出大小问题，有些情况需要大的 有些情况小的 有些情况要动态的！


18\.查询的select和表格的select冲突的问题！！！


19\.表格的不同模式下，不同字段的显示问题！


篇幅有限，下章讲接收后端的API如何编写,然后是如何使用代码生成器实现这个CRUD


有问题的，欢迎评论区提问，我会一一回答


