
## YUV

Y： 表示亮度，UV 表示色差和饱和度


YUV 格式： planer 模式， packet 模式，半 planer 模式

planer 模式 先把 Y ，紧接着 UV 
packet YUV 混合

YUV420 中，UV 数据量是 Y 的一半，总数据量为 w * h * 1.5;
YUV422 中，UV 水平方向数据量是 Y 的一半， 竖直方向数据量与 Y 相同;
YUV444 中，UV 数据量与 Y 相同

### YUV420P

YUV420P 使用的是 4:2:0 采样，每四个Y共用一组UV分量，其中每一帧中，前 4/6 中

```c
// YUV 4:2:0采样，每四个Y共用一组UV分量
// 视频宽高以及frame
// 其中 frame 表示一共有多少帧而不是一秒多少帧，例如 resource/sintel_640_360.yuv 文件大小为: 640 * 360, frame: 99, 24帧/s
int simplest_yuv420_split(const char *url, int w, int h, int frame)
{
    FILE *fp = fopen(url, "rb+");
    FILE *fp1 = fopen("/Users/noah-normal/Desktop/output_420_y.yuv", "wb+");
    FILE *fp2 = fopen("/Users/noah-normal/Desktop/output_420_u.yuv", "wb+");
    FILE *fp3 = fopen("/Users/noah-normal/Desktop/output_420_v.yuv", "wb+");

    unsigned char *pic = (unsigned char *)malloc( w * h * 3 / 2  );
    for (int i = 0; i < frame; ++i) 
    {
        fread(pic, 1, w * h * 3 / 2, fp );
        fwrite(pic, 1, w * h , fp1);
        fwrite(pic + w * h, 1, w * h / 4 , fp2);
        fwrite(pic + w * h * 5 / 4, 1, w * h /4 , fp3);
    }

    free(pic);
    fclose(fp);
    fclose(fp1);
    fclose(fp2);
    fclose(fp3);

    return 0;
}
```

生成单独存储的 Y、U、V 文件在 yuv 播放起中均可以单独播放，Y 文件大小为 U、V 文件的 4 倍，生成的文件均可以单独播放。


## 延伸资料：

学习内容：https://blog.csdn.net/leixiaohua1020/article/details/50534150

1. [YUV 格式详解，只看这一篇就够了](https://www.jianshu.com/p/538ee63f4c1c)
2. [YUV格式简介、YUV444、YUV422、YUV420](https://blog.csdn.net/yu540135101/article/details/107121769)
3. [图文详解YUV420 数据格式](https://markrepo.github.io/avcodec/2018/06/28/YUV/)