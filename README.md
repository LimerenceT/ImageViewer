<h1>ImageViewer</h1>

![releasesvg] ![apisvg] [![license][licensesvg]][license]

<h2>关于</h2>

图片预览器，支持图片手势缩放、拖拽等操作，`自定义View`的模式显示，自定义图片加载方式，更加灵活，易于扩展，同时也适用于RecyclerView、ListView的横向和纵向列表模式，最低支持版本为Android 3.0及以上...  

<h2>功能</h2>

- 图片的基本缩放、滑动
- 微信朋友圈图片放大预览
- 微信朋友圈图片拖拽效果
- 今日头条图片拖拽效果
- 图片加载进度条
- 可自定义图片索引与图片加载样式

<h2>传送门</h2>

- [自定义属性](#1)
- [事件监听器](#2)
- [自定义UI](#3)
- [添加依赖](#4)
- [简单示例](#5)
- [超巨图加载解决方案](#6)

<h2>推荐</h2>

- [AutoGridView][AutoGridView] 宫格控件，QQ空间九宫格、普通宫格模式、点击添加照片...

<h2>项目演示</h2>

![demo-simple]  ![demo-custom]  
![demo-land]  ![demo-port]
  
<h2>apk体验</h2>

### [点我][demo-apk]

<h2 id="1">自定义属性</h2>  

| 属性名 | 描述 |    
| :---- | :---- |    
| ivr_showIndex | 是否显示图片位置 |  
| ivr_playEnterAnim | 是否开启进场动画 | 
| ivr_playExitAnim | 是否开启退场动画 |   
| ivr_duration | 进场与退场动画的执行时间 |    
| ivr_draggable | 是否允许图片拖拽 |    
| ivr_dragMode | 拖拽模式（simple：今日头条效果 | agile：微信朋友圈效果） |  

<h2 id="2">事件监听器</h2>    
| setOnItemClickListener(OnItemClickListener listener) | item 的单击事件 |
| setOnItemLongListener(OnItemLongPressListener listener) | item 的长按事件 |
| setOnItemChangedListener(OnItemChangedListener listener) | item 的切换事件 |
| setOnDragStatusListener(OnDragStatusListener listener) | 监听图片拖拽状态事件 |
| setOnBrowseStatusListener(OnBrowseStatusListener listener) | 监听图片浏览器状态事件 |
  
<h2 id="3">自定义UI</h2>  
- 自定义索引UI
框架中内置默认索引视图`DefaultIndexUI`，如要替换索引样式，可继承抽象类`IndexUI`,并在使用`watch(...)`方法前,调用下列方法加载自定义的indexUI
```java
loadIndexUI(@NonNull IndexUI indexUI)
```
- 自定义加载进度UI
框架中内置默认加载视图`DefaultProgressUI`，如要替换加载样式，可继承抽象类`ProgressUI`,并在使用`watch(...)`方法前,调用下列方法加载自定义的progressUI
```java
loadProgressUI(@NonNull ProgressUI progressUI)
```
  
<h2 id="2">自定义方法</h2>    


<h2 id="3">添加依赖</h2> 

- Gradle
```Java
   Step 1:

   allprojects {
       repositories {
           ...
           // 如果添加依赖时，报找不到项目时（项目正在审核），可以添加此句maven地址，如果找到项目，可不必添加
           maven { url "https://dl.bintray.com/albertlii/android-maven/" }
       }
    }
    
    
   Step 2:
   dependencies {
      compile 'indi.liyi.view:image-viewer:3.0.0'
   }
```  

- Maven 
```Java
   <dependency>
      <groupId>indi.liyi.view</groupId>
      <artifactId>image-viewer</artifactId>
      <version>3.0.0</version>
      <type>pom</type>
   </dependency>
```

<h2 id="4">简单示例</h2>

#### XML 中添加 ImageViewer
```
  <indi.liyi.viewer.ImageViewer
        android:id="@+id/imageViewer"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

#### 代码中设置 ImageViewer
```Java
  // 图片浏览的起始位置
  imageViewer.setStartPosition(position);
  // 图片的数据源
  imageViewer.setImageData(mImageList);
  // 目标 View 的位置以及尺寸等信息
  imageViewer.setViewData(mViewDatas);
  // 自定义图片的加载方式
  imageViewer.setImageLoader(new ImageLoader() {
        @Override
        public void displayImage(final int position, Object src, final ImageView view) {
               Glide.with(SimplePreviewActivity.this)
                    .load(src)
                    .into(new SimpleTarget<Drawable>() {
                          @Override
                          public void onLoadStarted(@Nullable Drawable placeholder) {
                                 super.onLoadStarted(placeholder);
                                 view.setImageDrawable(placeholder);
                          }

                          @Override
                          public void onLoadFailed(@Nullable Drawable errorDrawable) {
                                 super.onLoadFailed(errorDrawable);
                                 view.setImageDrawable(errorDrawable);
                          }

                          @Override
                          public void onResourceReady(Drawable resource, Transition<? super Drawable> transition) {
                                 view.setImageDrawable(resource);
                                 mViewDatas.get(position).setImageWidth(resource.getIntrinsicWidth());
                                 mViewDatas.get(position).setImageHeight(resource.getIntrinsicHeight());
                          }
                     });
   }});
   // 开启图片浏览
   imageViewer.watch();
```
<h2 id="5">超巨图解决方案（进退场动画需重写，且不支持微信朋友圈拖拽，今日头条效果仍然支持）</h2>

1. 使用 [SubsamplingScaleImageView](SubsamplingScaleImageView) 代替 PhotoView（推荐）
2. 或者使用 [BigImageView](BigImageView) 代替 ScaleImageView

<h2>赞赏</h2>

如果你感觉 `ImageViewer` 帮助到了你，可以点右上角 "Star" 支持一下哦！:blush:

<h2>LICENSE</h2>

Copyright 2017 liyi

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.



[releasesvg]: https://img.shields.io/badge/version-3.0.0-brightgreen.svg
[apisvg]: https://img.shields.io/badge/sdk-14+-brightgreen.svg
[licensesvg]: https://img.shields.io/badge/license-Apache--2.0-blue.svg
[license]:http://www.apache.org/licenses/LICENSE-2.0

[AutoGridView]:https://github.com/albert-lii/AutoGridView
[demo-simple]:https://github.com/albert-lii/ImageViewer/blob/new/snapshot/demo_simple.gif
[demo-custom]:https://github.com/albert-lii/ImageViewer/blob/new/snapshot/demo_custom.gif
[demo-land]:https://github.com/albert-lii/ImageViewer/blob/new/snapshot/demo_land.gif
[demo-port]:https://github.com/albert-lii/ImageViewer/blob/new/snapshot/demo_port.gif
[demo-apk]:https://github.com/albert-lii/ImageViewer/blob/new/apk/release/app-release.apk

[SubsamplingScaleImageView]:https://github.com/davemorrissey/subsampling-scale-image-view
[BigImageView]:https://github.com/Piasy/BigImageViewer



