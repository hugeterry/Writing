---
ID: 335
post_title: Fresco保存图片
post_name: 'fresco%e5%ad%98%e5%82%a8%e5%9b%be%e7%89%87'
author: HugeTerry
post_date: 2016-02-28 19:19:53
layout: post
link: http://hugeterry.cn/dreams/335
published: true
tags:
  - Android
  - APP
  - Fresco
  - 趣刻
categories:
  - 学习总结
---
<a href="http://fresco-cn.org/" target="_blank">Fresco</a>是facebook自家出的一个强大的图片加载组件，因为缓存各方面强大的特性，所以在开发<a href="http://hugeterry.cn/dreams/324" target="_blank">趣刻</a>的时候，自己也将他加了进去

出现的第一个问题是用<code>RecyclerView</code>实现瀑布流的时候，因为<a href="http://fresco-cn.org/" target="_blank">Fresco</a>的SimpleDraweeView不能设置wrap_content，如果要使用则需设置setAspectRatio宽高比，所以除了获取网络加载图片的长宽之外，是比较难跟RecyclerView瀑布流结合的

第二个问题就是存储图片了，google了好久找不到答案，不过发现可以提取出缓存bitmap的方法，接着把bitmap存储就可以实现了，接下来贴代码：<!--?prettify linenums=true?-->
<pre class="prettyprint">ImageRequest imageRequest = ImageRequestBuilder
        .newBuilderWithSource(Uri.parse(url))
        .setProgressiveRenderingEnabled(true)
        .build();

ImagePipeline imagePipeline = Fresco.getImagePipeline();
DataSource&lt;CloseableReference&lt;CloseableImage&gt;&gt;
        dataSource = imagePipeline.fetchDecodedImage(imageRequest, context);

dataSource.subscribe(new BaseBitmapDataSubscriber() {
    @Override
    public void onNewResultImpl(Bitmap bitmap) {
        if (bitmap == null) {
            Toast.makeText(context, "保存图片失败啦,无法下载图片", Toast.LENGTH_SHORT).show();
        }
        File appDir = new File(Environment.getExternalStorageDirectory(), "Coderfun");
        if (!appDir.exists()) {
            appDir.mkdir();
        }
        String fileName = desc + ".jpg";
        File file = new File(appDir, fileName);
        try {
            FileOutputStream fos = new FileOutputStream(file);
            assert bitmap != null;
            bitmap.compress(Bitmap.CompressFormat.JPEG, 100, fos);
            fos.flush();
            fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        uri = Uri.fromFile(file);
        // 通知图库更新
        Intent scannerIntent = new Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE, uri);
        context.sendBroadcast(scannerIntent);

    }

    @Override
    public void onFailureImpl(DataSource dataSource) {
    }
}, CallerThreadExecutor.getInstance());</pre>