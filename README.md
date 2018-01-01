# PixelTimes

![Drop](/images/drop.gif)
This project is a logical progression of [PixelTime](https://2dom.github.io/PixelTime/) and the
[P10 LED MATRIX](https://github.com/2dom/P10_matrix) library.

As the P10 library became more capable, PixelTime was simply a bit boring to look at. So I started playing around with 32x16 gif animations and decided that the P10 matrix would make an excellent little nerdy picture frame. To do this is rather straightforward, what ate most of my time was the latancy optimizations to reduce flicker to a minimum.

## Set-up
To get this to work you will need to perform the following steps
  * Install [Arduino](https://www.arduino.cc/en/Main/Software)
  * Install [ESP8266 core for Arduino](https://www.arduino.cc/en/Main/Software)
  * Install the [SPIFFS upload tool](http://esp8266.github.io/Arduino/versions/2.0.0/doc/filesystem.html).
  * Download the the [P10_MATRIX library](https://github.com/2dom/P10_matrix) and move it to your libraries folder. Connect your LED matrix as described on the  library web-page.
  * Connect your ESP8266/NodeMCU to your computer, open the PixelTimes project in Arduino and select the "ESP8266 Sketch Data Upload" from the tools menu. This will write some some initial animations to the EPS8266/NodeMCU's flash memory.
  * Compile PixelTimes.ino and flash to ESP8266/NodeMCU

## Create your own animations
You can really use any video file as a source for your animations, however, low-res gif animations probably make the most sense. A fantastic source for 16x16 animations is [eboy](https://db.eboy.com/pool/everything/1). [Draw](http://www.drawbang.com/) also has a few usable ones. Since the P10 matrix is 32x16 you will have to modify these animations to fit the display (I used gimp). Once you have a bunch of 32x16 gifs in a folder you simply execute (see image_to_array.sh in gifs folder).

```bash
#!/bin/bash
for i in *.gif;
do
  base_name=$(basename "$i" .gif)
  ffmpeg -i $i -vf 'lutyuv=y=gammaval(1.4)' -vcodec rawvideo -pix_fmt rgb24 $base_name.rgb
done
```
Finally, you copy all the .rgb files in the /data folder of the PixelTimes Arduino project and re-upload via ESP8266 Sketch Data Upload tool.
