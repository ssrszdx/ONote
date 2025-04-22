# 修改excalidraw字体

> Excalidraw 是一个非常好用的手绘风格的绘图工具，我给自己部署了一个版本来降低自己写作配图的难度。如果你感兴趣，可以访问 draw.ixiqin.com 体验我自己维护的版本。
>
> 白宦成

我在使用 Excalidraw 时，最大的的问题就是没有中文的手写字体，这也是促使我自建的一个重要的原因。

​![](https://oss-emcsprod-public.modb.pro/image/auto/modb_20221108_272105a4-5f33-11ed-908c-fa163eb4f6be.png)  
效果

中文手写字体我选择的是 小赖字体 ，小赖字体可以免费商用，对于我这样的用途来说，更加的安全。

## 原理

实现的原理并不复杂，在页面中引入对应的字体文件，并为对应的字体设置对应的 Font Family 即可。

## 添加字体

在 Excalidraw 中添加字体，需要在 `public`​  
 目录中加入你的字体文件，并在 `public/fonts.css`​  
 中添加对应的字体引入，这样后续你的应用中就可以使用对应的字体来进行绘图。

需要注意的事，这里的 Font-Family 的值不要瞎填，后续会用到。

```
@font-face {  
	font-family: "XiaolaiSC";  
	src: url("XiaolaiSC-Regular.ttf");  
	font-display: swap;
}
```

此外，为了让浏览器可以提前加载字体，还可以在 `public/index.html`​  
 中添加如下代码来实现预加载。

```
<link rel="preload" href="XiaolaiSC-Regular.ttf" as="font" type="font/ttf" crossorigin="anonymous" >
```

## 添加常量

在 Excalidraw 中，组件使用的字体被定义在 `src/constants.ts`​  
 中的 `FONT_FAMILY`​  
 常量中，你需要在其中添加相应的 Font。这里就会用到你刚刚填写的 Font Family

```
export const FONT_FAMILY = {
    Virgil: 1,
    Helvetica: 2,
    Cascadia: 3,
    XiaolaiSC: 4,  这一行是新增的
};
```

## 在控制台上添加对应的调用

当你完成了这些基本的配置后，最后就简单了，只需要在 `src/actions/actionProperties.tsx`​  
 中的 `actionChangeFontFamily`​  
 中添加对应的 value 即可实现新字体的引入。

```
{
    value: FONT_FAMILY.XiaolaiSC,
    text: "小赖字体",
    icon: <FontFamilyLaiIcon theme={appState.theme} />,
},
```

参考代码：https://github.com/bestony/excalidraw/commit/f308dc32a958e4cb4fb4658cd9a5c9a19ad6d683
