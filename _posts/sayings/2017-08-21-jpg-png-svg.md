---
title:  "在网页上使用JPG、PNG和SVG：新手指南"
date: 2017-08-21 16:37:01 +0800
tags: 色
keywords: jpg png svg
description: 
---
> 本文转载自：[众成翻译](http://www.zcfy.cc)   Cheesecake Labs网站 <br />
> 译者：[lunasun](http://www.zcfy.cc/@lunasun)<br />
> 审校: [lizheming](http://www.zcfy.cc/@lizheming)<br />
> 链接：http://www.zcfy.cc/article/3211<br />
> 原文：https://cheesecakelabs.com/blog/jpg-png-svg-web-beginners-guide/

如今，图像已经成为网络不可或缺的一部分。但情况并非一贯如此。直到1993年，Mosaic浏览器才在网页内容中加入图像。有些图像格式像GIF和JPEG当时已经存在，而PNG和SVG直到90年代才出现。图像用途多样，如：显示图片、品牌、插图、图表以及许多其他内容。

由于图片格式多样以及繁多的应用场景，如何选择正确的图片格式变得更加困难。LOGO应该是选择SVG还是PNG？而截图是选JPEG好还是PNG好？在不生成过大文件的前提下，文件的最优质量是多少？了解每个图像格式的工作原理以及它们各自的利弊可以帮助回答这些问题。

在过去几年中，数字化设计和前端开发里大量的研究和测试工具已经帮助我们搞清楚了这些问题。在本文中，我将展示一下每种格式的工作原理，它们各自的优点以及在网页使用时的压缩与保存方法。

## JPEG

JEPG由联合图像专家小组（Joint Photographic Experts Group）于1992年创建，并以创建者命名。JPEG是一种有损光栅图像格式，这意味着每次压缩保存JPEG时，一些信息将发生不可逆转地丢失。

JPEG利用人眼感知的缺陷 - 对亮度比对颜色更敏感 - 使用了一种压缩算法来丢弃我们不太擅长获取的信息，因此属于“有损格式”。压缩率的设置将决定最终保存文件的大小和质量。 JPEG压缩技术远远不止这些，如果你想深入了解，可以看看大卫·奥斯丁（David Austin）的[这篇文章](http://www.ams.org/samplings/feature-column/fcarc-image-compression) 。

### JPEG的用途

因为JPEG适用于亮度和色彩压缩，所以在照片，以及其他写实或者带阴影的图像（如绘画和3D渲染）上使用效果良好。这就是为什么它是多年来最流行的存储图片的格式。出于同样的原因，JPEG不适宜用在矢量图片，如徽标，几何图形，截图等方面。

![](http://p0.qhimg.com/t01398b40b8f6abc4f5.jpg)

照片，以及绘画等复杂的或带阴影的图像，是使用JPEG的很好的例子。

### 压缩JPEG

作为有损格式，JPEG文件的压缩率与最终图像质量呈反比。在像Photoshop这样的工具中保存JPEG时，你会看到一个从0到100的质量设置。Photoshop设置了一些图像质量范围：

*   低 — 10%

*   中 — 30%

*   高 — 60%

*   非常高 — 80%

*   最佳 — 100%

![](http://p0.qhimg.com/t012169e30054b70e86.png)

最佳 100% (61 KB), 非常高 80% (29 KB).

![](http://p0.qhimg.com/t01bb2104631aff1ec2.png)

高 60% (16 KB), 中 80% (7 KB).

![](http://p0.qhimg.com/t014e2fd2515f4d8222.png)

低 100% (6 KB), 最低 0% (3 KB).

Web页面上建议使用在50％到60％质量之间的JPEG，因为它能兼顾不错的图像质量和较小的文件尺寸。删除元数据也可以减少JPEG文件体积。还有如[TinyJPG](https://tinyjpg.com/)的在线工具，以及桌面应用程序如 [ImageOptim](https://imageoptim.com/mac) (Mac) 和 [RIOT](http://luci.criosweb.ro/riot/) (Windows)都可以用来压缩图片。在Photoshop里，可以通过在“导出”中选择“元数据：无”或“存储为Web所用格式（旧版）”来完成压缩。对图像整体或者部分进行模糊处理也会产生较小的文件，[不信的话你可以看这里](https://responsivedesign.is/articles/reducing-image-sizes)。请注意，由于JPEG是有损格式，即使以100％的质量保存相同的文件，因为压缩算法在同一图像上一次又一次地应用，多次之后也会导致图像质量的降低。但这一变化可能不会显示在文件大小的改变上。

## PNG

可移植网络图形（Portable Network Graphics）也是一种自1995年以来就一直存在的光栅图像格式。它与JPEG不同，因为它是一种无损格式，并且是目前网络上最常见的无损格式。这意味着由于它的压缩算法，当文件被保存和压缩时，不会丢失任何信息。

PNG有很多很酷的特性，如：

*   透明通道 - 意味着每个像素可以具有不同的透明度;

*   8位文件可以使用基于调色板的颜色模型（也称为索引颜色） - 这意味着如果减少颜色数量，文件可能更小;

*   依据[libPNG](http://www.libpng.org/pub/png/)的说法，PNG压缩效率比GIF高25％

*   [二维隔行扫描](https://nuwen.net/png.html) — 图像会在加载过程中渐进显示，而不是只有当图像完全加载时才显示。你必须谨慎使用此选项，因为它会增加文件大小。

有关PNG更多特性、历史和技术信息的完整列表，请查看[libpng的页面](http://www.libpng.org/pub/png/).

### PNG的用途

PNG对于线条图，LOGO，图标和颜色较少的图像非常适合。颜色复杂的照片和图像使用PNG格式将生成巨大的文件。PNG另一个y优点是支持透明背景。在这种情况下，即使是复杂的图片仍然需要使用PNG，因为JPEG中无法实现图片透明。

![](http://p0.qhimg.com/t01246f280f26aac20c.png)

PNG可以很好地用在线条作品，LOGO和图标上。 ([漫画作者：xkcd](https://xkcd.com/327/))

### 压缩PNG

因为PNG中的压缩算法是无损的，你可以选择性地减少它的颜色，从而通过外部工具减小图片尺寸。 [Pngquant](http://pngquant/)就是一个很好的工具，它可以在保证透明度不变的情况下同时减少文件大小。请注意，这一过程会创建一个8位文件，即该文件最多可以有256种颜色。可能看起来不多，但是用这么多颜色足以获得很好的效果。

![](http://p0.qhimg.com/t01d6b922bc085ebe88.png)

左边的24位图像 (149 KB) 和右边8位，256色图像 (54 KB) — 缩小了63.7%

对于大多数PNG使用场景（线图，图形，图标），256色是足够的。因此，可以通过减少调色板中的颜色数量来进一步减少文件大小。 使用GUI工具是个不错的选择，如[Pngyu](https://nukesaq88.github.io/Pngyu/) 或 [ImageAlpha](https://pngmini.com/)，这些工具允许你预览生成的文件。 下面的例子显示了如何在不会显著影响质量的前提下，将调色板减少到32种颜色。在类似的例子中，图像很难被自动化地压缩——因为需要不断预览和测试来达到最佳效果——同时使用最少的颜色和产生最小的文件尺寸。就像JPEG一样，也有用于压缩PNG的在线工具，如：[TinyPNG](https://tinypng.com/).

![](http://p0.qhimg.com/t0178994ce911c2b4aa.png)

在这个示例中，LOGO从原始的24位PNG（10 KB）减少到8位，32色版本（2 KB，缩减80％），并且没有丢失任何明显的细节。

### 那么，GIF怎么样呢？

图形交换格式（Graphics Interchange Format）也是一种位图格式，并且比本文中提到的其它格式都出现地更早。它于1989年由[Steve Wilhite](https://en.wikipedia.org/wiki/Steve_Wilhite)创建, 在PNG创建前都是最流行的8位图像格式。GIF与PNG具有类似的特性，但有一些缺点：

*   仅支持256种颜色;

*   [一维隔行交错](https://nuwen.net/png.html) — 图像会渐进显示，但不够平滑;

*   与PNG相比压缩性能差

*   二进制透明度 - 像素只能是100％透明或100％可见;

*   有歧义的发音 😛

GIF因动画而出名并被广泛使用。但是，现在即使是动画也可以通过其它的方式完成，而且文件大小更小：例如使用SVG和Javascript，PNG序列帧或视频。所以，除非有非常特殊的原因必须使用GIF，否则我更建议大家使用PNG或SVG。

## SVG

可伸缩矢量图形（Scalable Vector Graphics）与前面讲的栅格格式不同，顾名思义，它是矢量格式。这意味着它不会基于像素存储数据，而是通过记录坐标的形式存储图形信息。SVG使用基于XML的语义化标签结构，这有点像HTML。由于是DOM结构，你可以通过ID获取SVG元素，并操纵它们。这带来了很多可能性，例如使用[JavaScript](http://snapsvg.io/demos/#coffee)和[CSS](https://css-tricks.com/animating-svg-css/) 修改并对元素进行动画操作或者创建[响应式图形](http://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)。

请看这个例子：[#1 – 咖啡机 – CSS3制作SVG动画](https://codepen.io/jonathansilva/pen/rLABRz/) ，作者乔纳森·席尔瓦（Jonathan Silva）([@jonathansilva](https://codepen.io/jonathansilva)) 发表于 [CodePen](https://codepen.io/)。

就像其它矢量格式，SVG图片能不丢失任何细节地放大到任何大小。打个比方，同一个图标，可以以多种尺寸使用，并且在任何屏幕分辨率（比如Retina显示器）中都将看起来很清晰，而不需要存成多个文件。

![](http://p0.qhimg.com/t011f76149d2246de20.png)

矢量图片 (右) 能够在保持图片质量的前提下任意放大

### SVG的用途

SVG在线条艺术，LOGO，图标，插画和数据可视化方面用途广泛。但它不适用于写实图像和有许多细节的复杂图片。在一些情况下，SVG和PNG都能很好地达到同一个目的。对于线条艺术，SVG通常能生成较小的文件。但是这不是必然的，实际情况会根据矢量图像究竟有多少个锚点，它甚至可能会生成比PNG更大的文件。 SVG真正出色的地方是数据可视化。由于可以使用JavaScript来操纵和创建矢量动画，诸如[D3](https://d3js.org/)之类的库提供了[无限的可能性](https://github.com/d3/d3/wiki/Gallery).

![](http://p0.qhimg.com/t017ebd67f020d1129d.png)

LOGO, 图标和数据可视化是SVG使用的优秀范例。

### 压缩SVG

大多数情况下，使用如SVGz（GZipped SVG）等工具来压缩在Web网页中使用的SVG文件是不必要的。你可以（并且应该）在服务器上开启 Gzip 来实现压缩功能，虽然可能效果寥寥。比较好的方法应该是通过清除SVG矢量图形中不必要的锚点、元素和属性来减少文件大小。锚点绘制了矢量图像，因此，你需要确保已移除的锚点不会影响矢量图形的最终形状。如果您使用Adobe Illustrator编辑SVG，请确保使用导出>导出为...而不是文件>另存为...进行保存，因为这样才能生成一个最小化的文件，当然它还有[其它优点](https://helpx.adobe.com/illustrator/how-to/export-svg.html)。在Sketch里, 注意不要使用不必要的文件夹，因为它们也会作为额外的标签保存到SVG中。
![](http://p0.qhimg.com/t018df9faf4a6b32855.png)

清理不必要的节点是缩减SVG尺寸的一种途径。

元素标签是包含在SVG文件内的所有内容，包括开始和结束标签。矢量编辑软件，如Adobe Illustrator和Sketch可能会到处含有非必要元素和属性的SVG。SVG压缩器可用于删除这种多余的信息。[Compressor](https://compressor.io/)和[SVGOMG](https://jakearchibald.github.io/svgomg/)等在线工具可以完成此工作。如果你是开发人员，而且不习惯清理和压缩SVG，可以用自动执行工具[SVGO](https://github.com/svg/svgo)，如果你是设计师，可以与该项目的开发人员谈谈SVG的优化，通过使用自动化的方式来避免你手动完成优化。

在下面的例子里，这个从Sketch里导出的图标有1364B大小。同一个图标在清理和压缩后，就只剩460B — 缩小了66%。

请在 [CodePen](https://codepen.io/)上看这个例子：[来自Sketch的SVG](https://codepen.io/brunomuler/pen/oBdMZL/) 作者布鲁诺·穆勒（Bruno Müller）([@brunomuler](https://codepen.io/brunomuler)) 。

优化前：
```
<?xml version="1.0" encoding="UTF-8"?>
    <svg width="20px" height="20px" viewBox="0 0 20 20" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
        <!-- Generator: Sketch 42 (36781) - http://www.bohemiancoding.com/sketch -->
        <title>ic_shopping_cart_black_24px</title>
        <desc>Created with Sketch.</desc>
        <defs></defs>
        <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
            <g id="Artboard" transform="translate(-51.000000, -61.000000)">
                <g id="ic_shopping_cart_black_24px" transform="translate(50.000000, 59.000000)">
                    <g id="Group">
                        <path d="M7,18 C5.9,18 5.01,18.9 5.01,20 C5.01,21.1 5.9,22 7,22 C8.1,22 9,21.1 9,20 C9,18.9 8.1,18 7,18 Z M1,2 L1,4 L3,4 L6.6,11.59 L5.25,14.04 C5.09,14.32 5,14.65 5,15 C5,16.1 5.9,17 7,17 L19,17 L19,15 L7.42,15 C7.28,15 7.17,14.89 7.17,14.75 L7.2,14.63 L8.1,13 L15.55,13 C16.3,13 16.96,12.59 17.3,11.97 L20.88,5.48 C20.96,5.34 21,5.17 21,5 C21,4.45 20.55,4 20,4 L5.21,4 L4.27,2 L1,2 Z M17,18 C15.9,18 15.01,18.9 15.01,20 C15.01,21.1 15.9,22 17,22 C18.1,22 19,21.1 19,20 C19,18.9 18.1,18 17,18 Z" id="Shape" fill="#000000" fill-rule="nonzero"></path>
                        <polygon id="Shape" points="0 0 24 0 24 24 0 24"></polygon>
                    </g>
                </g>
            </g>
        </g>
    </svg>
```
[优化后的SVG](https://codepen.io/brunomuler/pen/BpxPWY/)：
```
`<svg height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 18c-1.1 0-1.99.9-1.99 2S5.9 22 7 22s2-.9 2-2-.9-2-2-2zM1 2v2h2l3.6 7.59-1.35 2.45c-.16.28-.25.61-.25.96 0 1.1.9 2 2 2h12v-2H7.42c-.14 0-.25-.11-.25-.25l.03-.12.9-1.63h7.45c.75 0 1.41-.41 1.75-1.03l3.58-6.49c.08-.14.12-.31.12-.48 0-.55-.45-1-1-1H5.21l-.94-2H1zm16 16c-1.1 0-1.99.9-1.99 2s.89 2 1.99 2 2-.9 2-2-.9-2-2-2z"/><path d="M0 0h24v24H0z" fill="none"/></svg>`
```

## 文末思考

如同任何其他技术一样，图像格式也在不断发展。作为网页设计师和开发人员，我们的主要考虑的是浏览器兼容性。几年前，在IE6为主流浏览器的时代，PNG还不能使用透明通道。在不久的将来，也许我们会使用新的格式，如 [Google’s WebP](https://developers.google.com/speed/webp/) 或者其它仍未被创建出来的图片格式。

了解如何使用和优化每种图片格式将确保更好的用户体验。因为用户将能够更早地预览和接收内容，同时减少带宽的使用。它还为设计人员提供了绘制创建动画和响应式页面的新机会。

我希望这篇文章有助于解决一些网络上关于图像格式的诸多不确定性问题。如果你还有任何问题或建议，请在下方发表评论或与我联系。另外，如果觉得本文对你有帮助，不要忘了分享。
​                