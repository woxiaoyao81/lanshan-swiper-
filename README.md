## 来由
最近项目想实现卡片式轮播，开始由于项目使用uViewUI，于是想通过u-swiper实现，结果发现它不支持自定义，于是在DCloud官方插件市场找到不错的轮播组件<https://ext.dcloud.net.cn/plugin?id=2376>，介绍是
提供基础式、卡片式、链式、自定义各种样式，支持大部分应用场景，关键是支持自定义，刚好符合我项目需求，就直接使用了。加载后编译微信小程序成功，但编译APP时遇到不能实现自定义了，查了vuejs的slot语法，查
了uView里使用的自定义代码，发现几个问题：
>- 插槽在template是slots，而不是vuejs官方写的$slots，后来发现作者在data中定义了slots，也不知道slots和$slots怎么通信的
>- 属性类型为Number的在赋值后，在编译为APP时报错，提示要String。Number类型属性参加合并运算，如框架中是+'rpx'都报错；没合并运算则正常，如播放时间间隔。
>- 自定义范例`<template v-slot="{data}">`在编译时提示data属性，不推荐使用

## V3编译器对插槽支持情况
尽管我修改了正确的插槽定义，并核对了vuejs官网和uView中代码，但发现它编译后，微信小程序没问题，而APP就是无法成功，最后才想到uniapp对vuejs支持情况，原来V3编译APP时是不支持$slots，实在不理解。最后
通过属性间接实现了插槽的判断。修改后测试了**微信小程序、APP(安卓)和H5均正常**。

## 修改内容
>- 增加haveSlot属性。当需要自定义时设置为true，自定义时格式为
>> `<ls-swiper :list="list" :haveSlot="true">`
>> `<template v-slot="{elem}"> XXX </template>`
>> `</ls-swiper>`
>- 将参加合并运算的属性类型修改为[Number,String]，其修改参考了uView
>- 将组件传父组件的变量改为elem。
>- 优化了一些属性设置，如loop默认为true,dots默认为true等，减少日常调用时需要设置很多属性
