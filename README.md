# first-test001rasppypico
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C
import framebuf
import time

WIDTH = 128
HEIGHT = 64

i2c = I2C(1, scl = Pin(27), sda = Pin(26), freq=400000)
display = SSD1306_I2C(WIDTH, HEIGHT, i2c)

images = []
x = 163
for n in range(1, x):
    with open('/ani/frame_{:03d}.pbm'.format(n),'rb') as f:  #open folder and image
        print('/ani/frame_{:03d}.pbm'.format(n))
        f.readline() # Magic number
        f.readline() # Creator comment
        f.readline() # Dimensions
        data = bytearray(f.read())
    fbuf = framebuf.FrameBuffer(data, 64, 64, framebuf.MONO_HLSB) #adjust accordingly the width and height
    images.append(fbuf)

while True:
    z = 80
    for i in images:
        display.invert(1)
        display.rotate(0)
        display.blit(images[z], 64, 0)
        display.blit(i, 0, 0)
        if z <= (x-1):
            z = z + 1
        if z >= (x-1):
            z = 0
        
        display.show()
        time.sleep(0.1)
        print('{:03d}'.format(z))
