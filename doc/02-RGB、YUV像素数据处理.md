
## YUV

学习内容：https://blog.csdn.net/leixiaohua1020/article/details/50534150

### YUV420P

YUV420P 使用的是 4:2:0 采样，每四个Y共用一组UV分量

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
生成单独存储的 Y、U、V 文件在 yuv 播放起中均可以单独播放，Y 文件大小为 U、V 文件的 4 倍，