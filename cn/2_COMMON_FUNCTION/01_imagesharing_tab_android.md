### 共享图片

功能描述：在视频通话中将自己的图片文件分享给其他的参会人，以此提高沟通效率。

1. 开始共享图片：专属云和公有云的共享图片略有不同

   ```java
   // 开启图片选择GlideEngine
   public void getPicture(Fragment context){
           EasyPhotos.createAlbum(context, false, GlideEngine.getInstance())
                   .setCount(9)
                   .start(REQUEST_CODE);
   }
   // 需要创建一个GlideEngine继承自ImageEngine，具体可查看Demo
   GlideEngine.getInstance().getPicture(this);
   
   // 接收选择数据
   		@Override
       public void onActivityResult(int requestCode, int resultCode, Intent data) {
           if (resultCode == Activity.RESULT_OK) {
               if (requestCode == GlideEngine.REQUEST_CODE) {
                   // 从文件中选择图片并返回
                   ArrayList<Photo> selectList = data.getParcelableArrayListExtra(EasyPhotos.RESULT_PHOTOS);
                   if (selectList != null && selectList.size() > 0) {
                       isPicture = true;
                       isPDF = false;
                       for (Photo media : selectList) {
                           imagePathList.add(media.path);
                       }
   		                // ......这部分是本地预览的相关功能，具体可以参考demo，也可以自行处理
                     	// 发起双流
                       sendSharePicture();
                   }
               }
           }
       }
   		
   		/**
        * 发送分享的图片
        */
       private void sendSharePicture() {
           Bitmap bitmap = null;
         	// 针对图片进行裁剪，并满足16:9的大小，用于发送，工具可以参考sdkdemo中的BitmapUtil
           bitmap = BitmapUtil.formatBitmap16_9(imagePathList.get(pictureIndex), 1920, 1080);
           if (bitmap != null) {
             	sendPresentation(Bitmap bitmap);
           }
       }
   	
   
   ```
   
2. 关闭共享图片功能：

   ```java
   private void stopPresentation() {
           vcrtc.stopPresentation();
       }
   ```


### 共享PDF

共享PDF功能和共享图片功能类似，主要都是图片的共享，需要将PDF转化成图片资源文件再发起共享。（详细操作参考sdkDemo）

1. 初始化PDFUtil

```
pdfUtil = new PDFUtil(getActivity(), true, 1920, 1080);
```

2. 跳转选取pdf画面

```java
 Intent galleryIntent = new Intent(Intent.ACTION_GET_CONTENT);
 galleryIntent.setType("application/pdf");
 galleryIntent.addCategory(Intent.CATEGORY_OPENABLE);
 galleryIntent.setFlags(Intent.FLAG_ACTIVITY_BROUGHT_TO_FRONT);
 galleryIntent.putExtra(Intent.EXTRA_ALLOW_MULTIPLE, false);
 Intent chooserIntent = Intent.createChooser(galleryIntent, getString(R.string.share_choose_file));

 mActivity.startActivityForResult(chooserIntent, PDF_PICKER_REQUEST);
```

3. 接收返回的数据

```java
@Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode == Activity.RESULT_OK) {
            if (requestCode == PDF_PICKER_REQUEST) {
                // 从文件中选择pdf并返回
                Uri uri = data.getData();
                if (uri != null) {
                    try {
                      	// 将返回的uri设置给pdfUtil，同时返回pdf的第一页bitmap
                        pdfBitmap = pdfUtil.openFile(uri);
                      	// ......这部分是本地预览的相关功能，具体可以参考demo，也可以自行处理
                      	// 同发送图片
                        sendSharePicture();
                    } catch (Exception e) {
                        showToast("共享失败，请检测文件格式是否正确");
                        e.printStackTrace();
                    }
                }
            }
        }
    }


// 翻页
pdfUtil.openPage(int position);
```

