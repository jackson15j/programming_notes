Bash Notes
==========

Dump of Bash notes and one-liners.


ffmpeg
------

* [FFMPEG:Convert video files to GIF]:

```bash
 ffmpeg -y -ss <secs_to_start_from> -t <duration_in_secs> -i <source_video> -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png && ffmpeg -ss <secs_to_start_from> -t <duration_in_secs> -i <source_video> -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" <output.gif>
 ffmpeg -y -ss 8 -t 4 -i P1020397.MP4 -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png && ffmpeg -ss 8 -t 4 -i P1020397.MP4 -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" output4.gif
```


[FFMPEG: Convert video files to GIF]: https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality#556031
