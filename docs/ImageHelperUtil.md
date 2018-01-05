## Image Helper Tool:

/**Java  

    package com.lee.myapplication;

    import android.graphics.Bitmap;
    import android.graphics.BitmapFactory;
    import android.graphics.Matrix;
    import android.os.Environment;
    import android.util.Log;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.File;
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.text.SimpleDateFormat;
    import java.util.Date;

    /**
    * Created with Android Studio
    * Email: sielee@163.com
    * Date: 2016/1/13
    * Author: SieLee
    * CopyRight: MilesLife
    *
    * @Description: TODO
    */
    public class ImageHelperUtil {

    /**
    * inSampleSize比例压缩+Bitmap.compress()质量压缩
    *
    * @param filePath
    * @return
    */
    public static Bitmap comp(String filePath) {

    final BitmapFactory.Options options = new BitmapFactory.Options();
    //该值设为true那么将不返回实际的bitmap，也不给其分配内存空间这样就避免内存溢出了。
    //但是允许我们访问图片的信息这其中就包括图片大小信息(图片原始宽高: option.outWidth/options.outHeight)。
    options.inJustDecodeBounds = true;
    BitmapFactory.decodeFile(filePath, options);

    //Calculate inSampleSize, 压缩比例
    //inSampleSize > 1,要求解码器解码出原始图像的一个子样本,返回一个较小的bitmap,以节省存储空间。
    //inSampleSize 小于等于 1的同样处置为1,不压缩
    //如: inSampleSize = 2，则取出的缩略图的宽和高都是原始图片的1/2，图片大小就为原始大小的1/4。
    options.inSampleSize = calculateInSampleSize(options, 480, 800);

    // Decode bitmap with inSampleSize set
    options.inJustDecodeBounds = false;

    //取Bitmap位图时,inSampleSize 可能小于0，必须做判断
    Bitmap image = BitmapFactory.decodeFile(filePath, options);

    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    image.compress(Bitmap.CompressFormat.JPEG, 100, baos);
    //判断如果图片大于1M,进行压缩避免在生成图片（BitmapFactory.decodeStream）时溢出
    if (baos.toByteArray().length / 1024 > 1024) {
    baos.reset();//重置baos即清空baos
    image.compress(Bitmap.CompressFormat.JPEG, 50, baos);//这里压缩50%，把压缩后的数据存放到baos中
    }
    ByteArrayInputStream isBm = new ByteArrayInputStream(baos.toByteArray());
    BitmapFactory.Options newOpts = new BitmapFactory.Options();
    //开始读入图片，此时把options.inJustDecodeBounds 设回true了
    newOpts.inJustDecodeBounds = true;
    Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, newOpts);
    newOpts.inJustDecodeBounds = false;
    int w = newOpts.outWidth;
    int h = newOpts.outHeight;
    //现在主流手机比较多是800*480分辨率，所以高和宽我们设置为
    float hh = 800f;//这里设置高度为800f
    float ww = 480f;//这里设置宽度为480f
    //缩放比。由于是固定比例缩放，只用高或者宽其中一个数据进行计算即可
    int be = 1;//be=1表示不缩放
    if (w > h and w > ww) {//如果宽度大的话根据宽度固定大小缩放
    be = (int) (newOpts.outWidth / ww);
    } else if (h >w and h > hh) {//如果高度高的话根据宽度固定大小缩放
    be = (int) (newOpts.outHeight / hh);
    }
    if (be 小于等于 0)
    be = 1;
    newOpts.inSampleSize = be;//设置缩放比例
    //重新读入图片，注意此时已经把options.inJustDecodeBounds 设回false了
    isBm = new ByteArrayInputStream(baos.toByteArray());
    bitmap = BitmapFactory.decodeStream(isBm, null, newOpts);
    return compressImage(bitmap);//压缩好比例大小后再进行质量压缩
    }

    /**
    * 根据图片路径进行压缩图片
    *
    * @param srcPath
    * @return
    */
    public static Bitmap getImage(String srcPath, int size) {
    BitmapFactory.Options newOpts = new BitmapFactory.Options();
    //开始读入图片，此时把options.inJustDecodeBounds 设回true了,表示只返回宽高
    newOpts.inJustDecodeBounds = true;
    Bitmap bitmap = BitmapFactory.decodeFile(srcPath, newOpts);//此时返回bm为空

    newOpts.inJustDecodeBounds = false;
    //当前图片宽高
    float w = newOpts.outWidth;
    float h = newOpts.outHeight;
    //现在主流手机比较多是800*480分辨率，所以高和宽我们设置为
    float hh = 800f;//这里设置高度为800f
    float ww = 480f;//这里设置宽度为480f
    //缩放比。由于是固定比例缩放，只用高或者宽其中一个数据进行计算即可
    int be = 1;//be=1表示不缩放
    if (w > h and w > ww) {//如果宽度大的话根据宽度固定大小缩放
    //Log.d(Tag,"fileupload", "------原始缩放比例 --------" + (newOpts.outWidth / ww));
    be = (int) (newOpts.outWidth / ww);
    //有时会出现be=3.2或5.2现象，如果不做处理压缩还会失败
    if ((newOpts.outWidth / ww) > be) {

    be += 1;
    }
    //be = Math.round((float) newOpts.outWidth / (float) ww);
    } else if (h>w and h > hh) {//如果高度高的话根据宽度固定大小缩放
    //LogPrint.Print("fileupload","------原始缩放比例 --------" + (newOpts.outHeight / hh));
    be = (int) (newOpts.outHeight / hh);
    if ((newOpts.outHeight / hh) > be) {

    be += 1;
    }
    //be = Math.round((float) newOpts.outHeight / (float) hh);
    }
    if (be 小于等于 0) {

    be = 1;
    }
    newOpts.inSampleSize = be;//设置缩放比例
    //LogPrint.Print("fileupload","------设置缩放比例 --------" + newOpts.inSampleSize);
    //重新读入图片，注意此时已经把options.inJustDecodeBounds 设回false了
    bitmap = BitmapFactory.decodeFile(srcPath, newOpts);
    return compressImage(bitmap, size);//压缩好比例大小后再进行质量压缩
    }

    public static String saveBitmap(Bitmap bm) {
    //Log.e(TAG, "保存图片");

    Date date = new Date();
    SimpleDateFormat format = new SimpleDateFormat("yyMMddHHmmss"); // 格式化时间
    String imageFileName = format.format(date) + ".jpg";
    File f = new File(Environment.getExternalStorageDirectory().getAbsolutePath() + "/supershank",
    imageFileName);
    if (f.exists()) {
    f.delete();
    }
    try {
    FileOutputStream out = new FileOutputStream(f);
    bm.compress(Bitmap.CompressFormat.PNG, 80, out);
    out.flush();
    out.close();

    } catch (FileNotFoundException e) {
    } catch (IOException e) {
    }
    return f.getAbsolutePath();
    }

    public static Bitmap rotateBitmap(Bitmap bitmap, int rotate) {
    if (bitmap == null)
    return null;

    int w = bitmap.getWidth();
    int h = bitmap.getHeight();

    // Setting post rotate to 90
    Matrix mtx = new Matrix();
    mtx.postRotate(rotate);
    return Bitmap.createBitmap(bitmap, 0, 0, w, h, mtx, true);
    }

    //------------------------------------------以下所有方法为内部调用------------------------------------------

    /**
    * 设为指定宽高后的压缩Ratio(比例)
    *
    * @param options
    * @param reqWidth
    * @param reqHeight
    * @return
    */
    private static int calculateInSampleSize(BitmapFactory.Options options,
    int reqWidth, int reqHeight) {
    // Raw height and width of image
    final int height = options.outHeight;
    final int width = options.outWidth;
    int inSampleSize = 1;

    if (height > reqHeight || width > reqWidth) {

    // Calculate ratios of height and width to requested height and
    // width
    final int heightRatio = Math.round((float) height
    / (float) reqHeight);
    final int widthRatio = Math.round((float) width / (float) reqWidth);

    inSampleSize = widthRatio>heightRatio ? widthRatio : heightRatio;
    }

    return inSampleSize;
    }

    /**
    * 会影响图片质量
    *
    * @param image
    * @return
    */
    private static Bitmap compressImage(Bitmap image) {

    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    image.compress(Bitmap.CompressFormat.JPEG, 100, baos);//质量压缩方法，这里100表示不压缩，把压缩后的数据存放到baos中
    int options = 100;
    while (baos.toByteArray().length / 1024 > 100) { //循环判断压缩后图片是否大于100kb,大于继续压缩
    baos.reset();//重置baos即清空baos
    image.compress(Bitmap.CompressFormat.JPEG, options, baos);//这里压缩options%，把压缩后的数据存放到baos中
    options -= 10;//每次都减少10
    }
    ByteArrayInputStream isBm = new
    ByteArrayInputStream(baos.toByteArray());//把压缩后的数据baos存放到ByteArrayInputStream中
    Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, null);//把ByteArrayInputStream数据生成图片
    return bitmap;
    }

    /**
    * 会影响图片质量
    *
    * @param image
    * @param size
    * @return
    */
    private static Bitmap compressImage(Bitmap image, int size) {

    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    image.compress(Bitmap.CompressFormat.JPEG, 100, baos);//质量压缩方法，这里100表示不压缩，把压缩后的数据存放到baos中
    int options = 100;

    while ((baos.toByteArray().length / 1024) >= size) { //循环判断如果压缩后图片是否大于等于size,大于等于继续压缩
    //LogPrint.Print("fileupload","------ByteArray--------" + baos.toByteArray().length / 1024);
    baos.reset();//重置baos即清空baos
    options -= 5;//每次都减少5
    image.compress(Bitmap.CompressFormat.JPEG, options, baos);//这里压缩options%，把压缩后的数据存放到baos中
    //LogPrint.Print("fileupload","------压缩质量--------" + options);
    Log.i("TAG", baos.toByteArray().length + "");
    }
    ByteArrayInputStream isBm = new
    ByteArrayInputStream(baos.toByteArray());//把压缩后的数据baos存放到ByteArrayInputStream中
    Bitmap bitmap = BitmapFactory.decodeStream(isBm, null, null);//把ByteArrayInputStream数据生成图片
    return bitmap;
    }

    }
**/    