# ffmpeg 命令

* 提取一帧yuv
ffmpeg -i test.ts -ss 00:00:01 -vframes 1 -pix_fmt yuv420p b.yuv