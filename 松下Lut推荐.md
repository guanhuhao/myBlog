# 松下S5M2实时Lut设置
## x-log曲线 与 Lut
### 什么是log曲线？
由于人眼对亮度的感知呈**对数关系**，为了捕捉更广泛的亮度范围，**提高动态范围**（最亮的与最暗的差距成为动态范围），相机通过使用对数函数来模拟人眼对光线明暗变化，使得相机能够记录更多的亮部和暗部细节。但在log模式下原始照片/视频素材通常会显得比较灰暗，对比度和饱和度都较低（俗称灰片），这是因为Log曲线将更多的数据信息压缩到了可管理的范围内，为后期调色和分级提供了更大的灵活性。因此在后期需要对色彩进行还原。
不同相机厂商根据自身相机的特点以及镜头等光学元件的控制，对于自家产品开发了不同的log曲线，主流的有索尼的`S-log`，佳能的`C-log`，尼康的`N-log`曲线以及松下的`V-Log`曲线。

### 什么是LUT？
由于log曲线为了尽可能多的**保留高光/阴影细节信息**，对原始信息使用log曲线进行压缩存储，因此在后期时需要将信息还原为正常的色彩空间。此时需要将log色彩空间转化到正常色彩空间（例如Rec.709）.此时色彩查找表 Look Up Table （简称**LUT**）便起到色彩转换的作用。 特别的Lut不仅能将log的低饱和照片还原为正常的颜色，同时还能够**赋予照片特殊的风格**，例如对于自然景色，我们希望天更蓝，草更绿，花更鲜艳此时可以在映射的时候对于这些景色对应的色彩进行强化，以满足不同风格照片的需求

## 松下实时Lut预览
一般的拍摄流程是先拍摄log的照片后后期再套lut进行还原，但是存在拍摄时无法确定后期效果，拍摄时缺少正反馈（没法给模特小姐姐直接看 误），同时无法对于拍摄场景以及环境及时确定拍摄风格的问题。因此比较理想的情况是在拍摄时能及时看到后期套lut的大致效果，这项功能称之为机内实时Lut。这项功能大多搭载在在各家高端机型上，而松下将该功能下放至：55M2，S5M2X，G9M2，S9等机型上。
同时松下相机的屏幕素质以及优秀的色彩科学，使得使用松下相机拍摄时可以更好的获得及时反馈。

### 怎么使用实时Lut
1. 下载Lut文件
> lut曲线实质在于建立色彩之间转化的映射关系，有官方以及个人制作的优秀Lut可供选择，对于松下官方提供了`Lumix lab`这个app共用户自行上传Lut以及下载(免费且素质不错),同时该lut也可以套给部分别的相片使用如vivo，苹果的部分机型

2. Lut格式转化
> 下载的lut文件一般为.cube文件，但松下的机内监看目前**仅支持.vlt**，需要使用达芬奇（Davinci）进行转化，教程可见[此](https://www.douyin.com/search/%E8%BE%BE%E8%8A%AC%E5%A5%87cube%E6%A0%BC%E5%BC%8F%E8%BD%AC%E6%88%90vlt?modal_id=7215421948728249638)
PS：需要注意一下几点
>+ 1. 名称仅包含大小写英文（不能有中文，全角符号，空格，横线等特殊字符）
>+ 2. 名称控制在8个字母以内（包含8）

3. 将转化好的.vlt文件放在SD卡**根目录**（注意一定是根目录！！！否则识别不到）

4. 机内设置
将装有.vlt的SD卡插入相机，一次 摁下Menu按钮 -> 齿轮（设置） -> 第一个子标签页 -> LUT库。此时会看到除了`Vlog_709`的其他选项，点击其中一个选择加载，选择装有.vlt的卡槽，在选择需要加载的vlt，以此确定即可，若显示无法无法设置此菜单一般为.vlt识别不到，检查.vlt格式是否正常，是否放在sd卡根目录等操作

5. 设置V-log监看
这里介绍一种比较好的设置，有效利用松下多余的空快捷按钮，[参考链接在此](https://www.bilibili.com/video/BV1ZdhkeWEow/?spm_id_from=333.880.my_history.page.click&vd_source=e7f814ba70aedd6653447be83f3dc2d8)
长摁轮盘任意方向键(例如↓) -> 2 -> 最后一个图标 -> Lut选择，这一步操作完成了将该方向键设置为Lut选择的快捷方式
然后摁该快捷键即可完成lug监看切换，**需要注意的是需要将格调选为V-log模式**

### 部分Lut推荐
下面推荐一些自己选的感觉还不错的Lut，基本都是在Lumix lab中找的，可以根据名字自己找一下

<!-- |名称|tag|样片|
|---|---|---|
|Blue sky|`人像` `小清新` `青色调`| [](./pic/松下lut/Blue%20Sky.jpg)|
|Vintage Vibes|`胶片风` `复古` `偏黄`|[](./pic/松下lut/Vintage%20Vibes.jpg)|
|Vintage Blues|`人像` `顺光` `显白`|[](./pic/松下lut/Vintage%20Blues.jpg)|
|Warming Light|`人像` `暖色调` `显白`|[](./pic/松下lut/Warming%20Light.jpg)|
|Style For Camping|`露营` `胶片氛围感` |[](./pic/松下lut/Style%20For%20Camping.jpg)|
|Natural LUMIX|`自然` `偏绿` `明亮`|[](./pic/松下lut/Natural%20LUMIX.jpg)|
|Camp-CinemaLUT|`露营` `浓郁` `色彩重` |[](./pic/松下lut/Camp-CinemaLUT.jpg)|
|T-Natural|`人像` `柔和` `自然` |[](./pic/松下lut/T-Natural.jpg)|
|T-Nostalgy|`胶片` `年代感`|[](./pic/松下lut/T-Nostalgy.jpg)|
|Retrostyle709|`709` `高对比度` `浓郁` `质感`|[](./pic/松下lut/Retrostyle709.jpg)| -->

|名称|tag|样片|
|---|---|---|
|Blue sky|`人像` `小清新` `青色调`| ![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Blue-Sky.jpg)|
|Vintage Vibes|`胶片风` `复古` `偏黄`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Vintage-Vibes.jpg)|
|Vintage Blues|`人像` `顺光` `显白`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Vintage-Blues.jpg)|
|Warming Light|`人像` `暖色调` `显白`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Warming-Light.jpg)|
|Style For Camping|`露营` `胶片氛围感` |![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Style-For-Camping.jpg)|
|Natural LUMIX|`自然` `偏绿` `明亮`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Natural-LUMIX.jpg)|
|Camp-CinemaLUT|`露营` `浓郁` `色彩重` |![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Camp-CinemaLUT.jpg)|
|T-Natural|`人像` `柔和` `自然` |![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/T-Natural.jpg)|
|T-Nostalgy|`胶片` `年代感`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/T-Nostalgy.jpg)|
|Retrostyle709|`709` `高对比度` `浓郁` `质感`|![](https://guanhuhao.github.io/picx-images-hosting/LumixLut/Retrostyle709.jpg)|
