## NDK
java native interface  
android.useDeprecatedNdk=true

    1.编写Java文件:
    public class JniTest{
    static {
    System.loadLibrary("jary");
    }
    public native String getString();
    }

    2. 编译出此文件,Build-Make Project
    3. 依据.class文件输出.h文件:
    Terminal工具下,进入main目录下(会在此目录下生成jni文件夹)
    输入命令, javah -d jni -classpath [.class文件路径,最后一个debug文件使用空格代替'\',包名.java文件名]
    4. 编写.c文件:

    最后配置.gradle文件
    defaultConfig:
    ndk{
    moduleName "jary"         //生成的so名字
    abiFilters "armeabi", "armeabi-v7a", "x86"  //输出指定三种abi体系结构下的so库。
    stl "stlport_static"    //打开.c 的 debug , 下面第 4 点会讲到
    }
