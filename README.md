# glslViewer [![Build Status](https://travis-ci.org/patriciogonzalezvivo/glslViewer.svg?branch=master)](https://travis-ci.org/patriciogonzalezvivo/glslViewer)

![](http://patriciogonzalezvivo.com/images/glslViewer.gif)

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=4BQMKQJDQ9XH6)

Live-coding console tool that renders GLSL Shaders. Every file you use (frag/vert shader, images and geometries) are watched for modification, so they can be updated on the fly.

## Install

### Installing on Ubuntu

Install the GLFW 3 library and other dependencies:

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install libglfw3-dev git-core
```

Download the glslViewer code, compile and install:

```bash
git clone http://github.com/patriciogonzalezvivo/glslViewer
cd glslViewer
make
sudo make install
```

This was tested with Ubuntu 16.04.

These instructions may not work for all users.
For example, it seems that libglfw3-dev conflicts with the older libglfw-dev.
The previous Ubuntu install instructions direct you to
[download and compile glfw3 manually](http://www.glfw.org/docs/latest/compile.html#compile_deps_x11):

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install git-core cmake xorg-dev libglu1-mesa-dev
cd ~
git clone https://github.com/glfw/glfw.git
cd glfw
cmake .
make
sudo make install
```

### Installing on Debian testing (Buster)

Install the GLFW 3 library, build tools, and other dependencies:

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install libglfw3-dev xorg-dev libglu1-mesa-dev git-core dh-make fakeroot build-essential
```

Download the glslViewer code, compile and install. These instructions use deb-helper
to build a native debian package, to build a simple binary, follow the Ubuntu instructions above.

```bash
git clone https://github.com/patriciogonzalezvivo/glslViewer.git
cd glslViewer
fakeroot dh binary
cd ..
sudo dpkg -i glslviewer_1.5_amd64.deb
```
(The arch part of the .deb filename will differ on other cpu architectures.)

This was tested with the Debian testing distribution on January 28th 2018.

### Installing on Raspberry Pi

Get [Raspbian](https://www.raspberrypi.org/downloads/raspbian/), a Debian-based Linux distribution made for Raspberry Pi and then do:

```bash
sudo apt-get install glslviewer
```

Or, if you want to compile the code yourself:

```bash
cd ~
git clone http://github.com/patriciogonzalezvivo/glslViewer
cd glslViewer
make
sudo make install
```

### Installing on Arch Linux

```bash
sudo pacman -S glu glfw-x11
git clone http://github.com/patriciogonzalezvivo/glslViewer
cd glslViewer
make
sudo make install
```

Or simply install the AUR package [glslviewer-git](https://aur.archlinux.org/packages/glslviewer-git/) with an [AUR helper](https://wiki.archlinux.org/index.php/AUR_helpers).

### Installing on macOS

Use [Homebrew](https://brew.sh) to install glslViewer and its dependencies:

```bash
brew update
brew upgrade
brew install glslviewer
```

If you prefer to compile from source directly from this repository you need to [install GLFW](http://www.glfw.org), `pkg-config` first and then download the code, compile and install.

```bash
brew update
brew upgrade
brew tap homebrew/versions
brew install glfw3 pkg-config
cd ~
git clone http://github.com/patriciogonzalezvivo/glslViewer
cd glslViewer
make
make install
```

If `glfw3` was installed before, after running the code above, remove `glfw3` and try:

```bash
brew install glfw3 pkg-config
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
make
make install
```

### Install Python wrapper

GlslViewer now ships with a Python package. It's essentially a wrapper that allows to compile, benchmaark and render shaders from a Python script.

1. First make sure you have the setup tools

```bash
sudo apt install python-setuptools
```

2. Once you compile the binnaries do

```bash
sudo make install_python
```



## Using glslViewer

### 1. Loading a single Fragment shader

In the most simple scenario you just want to load a fragment shader. For that you need to:

* Run the app passing the shader as an argument

```bash
cd examples
glslViewer test.frag
```

* Then edit the shader with your favorite text editor.

```bash
vim test.frag
```

**Note**: In RaspberryPi you can avoid taking over the screen by using the `-l` flags so you can see the console. Also you can edit the shader file through ssh/sftp.

**Note**: On Linux and macOS you may used to edit your shaders with Sublime Text, if that's your case you should try this [Sublime Text 2 plugin that launch glslViewer every time you open a shader](https://packagecontrol.io/packages/glslViewer).

### 2. Loading a Vertex shader and a geometrie

![](http://patriciogonzalezvivo.com/images/glslViewer-3D.gif)

You can also load both fragments and vertex shaders. Of course modifying a vertex shader makes no sense unless you load an interesting geometry. That's why `glslViewer` can load `.ply` files. Try doing:

```bash
glslViewer bunny.frag bunny.vert bunny.ply
```

### 3. Loading Textures

You can load PNGs and JPEGs images to a shader. They will be automatically loaded and assigned to a uniform name according to the order they are passed as arguments: ex. `u_tex0`, `u_tex1`, etc. Also the resolution will be assigned to `vec2` uniform according to the texture uniform's name: ex. `u_tex0Resolution`, `u_tex1Resolution`, etc.

```bash
glslViewer test.frag test.png
```

In case you want to assign custom names to your textures uniforms you must specify the name with a flag before the texture file. For example to pass the following uniforms `uniform sampled2D imageExample;` and  `uniform vec2 imageExampleResolution;` is defined in this way:

```bash
glslViewer shader.frag -imageExample image.png
```

### 4. Other arguments

Beside for texture uniforms other arguments can be add to `glslViewer`:

* `-x [pixels]` set the X position of the billboard on the screen

* `-y [pixels]` set the Y position of the billboard on the screen

* `-w [pixels]` or `--width [pixels]`  set the width of the billboard

* `-h [pixels]` or `--height [pixels]` set the height of the billboard

* `-s [seconds]` exit app after a specific amount of seconds

* `-o [image.png]` save the viewport to an image file before

* `-l` to draw a 500x500 billboard on the top right corner of the screen that let you see the code and the shader at the same time. (RaspberryPi only)

* `-c`or `--cursor` show cursor.

* `--headless` headless rendering. Very useful for making images or benchmarking.

* `-I[include_folder]` add an include folder to default for `#include` files

* `-D[define]` add system `#define`s directly from the console argument

* `-[texture_uniform_name] [texture.png]`: add textures associated with different `uniform sampler2D`names

* `-vFlip` all textures after will be flipped vertically

*  `-v` verbose outputs

* `--help` display the available command line options

### Console IN commands

Once glslViewer is running the CIN is listening for some commands, so you can pass data through regular *nix pipes.

* ```int```, ```floats```, ```vec2```, ```vec3``` and ```vec4``` uniforms can be passed as comma separated values, where the first column is for the name of the uniform and the rest for the numbers of values the uniform have. **Note** that there is a distinction between ```int```and ```float```so remember to put ```.``` (floating points) to your values.

* ```date```: return content of ```u_date```, return the current year, month, day and seconds

* ```time```: return content of ```u_time```, the elapsed time since the app start

* ```delta```: return content of ```u_delta```, return the last delta time between frames

* ```fps```: return content of ```u_fps```, return the number of frames per second

* ```frag```: return the source of the fragment shader

* ```vert```: return the source of the vertex shader

* ```window_width```, ```window_height```, ```screen_size``` and ```viewport``` : return the size of the windows, screen and viewport (content of ```u_resolution```)

* ```pixel_density```: return the pixel density

* `mouse_x`, `mouse_y` and `mouse`: return the position of the mouse (content of `u_mouse`)

* `view3d`: returns the position of the camera, the up-vector and the center of the object;

* `screenshot [filename]`: save a screenshot of what's being rendered. If there is no filename as argument will default to what was defined after the `-o` argument when glslViewer was launched.

* `q`, `quit` or `exit`: close glslViewer


## glslViewer conventions

### Pre-Defined `uniforms`

* `uniform sampler2D u_tex[number]`: default textures names

* `uniform float u_time;`: shader playback time (in seconds)

* `uniform float u_delta;`: delta time between frames (in seconds)

* `uniform vec4 u_date;`: year, month, day and seconds

* `uniform vec2 u_resolution;`: viewport resolution (in pixels)

* `uniform vec2 u_mouse;`: mouse pixel coords

* `varying vec2 v_texcoord`: UV of the billboard ( normalized )

* `uniform vec3 u_eye`: Position of the 3d camera when rendering 3d objects

* `uniform vec2 u_view2d`: 2D position of viewport that can be changed by dragging

* `uniform vec3 u_eye3d`: Position of the camera

* `uniform vec3 u_centre3d`: Position of the center of the object

* `uniform vec3 u_up3d`: Up-vector of the camera

### Including dependent files with `#include`

You can include other GLSL code using a traditional `#include "file.glsl"` macro. 

*Note*: included files are not under watch so changes will not take effect until the main file is saved.

### Using multiple buffers

You can use multiple buffers by forking the code using `#ifdef BUFFER_[number]`, `#if defined (BUFFER_[number])` and/or `#elif defined (BUFFER_[number])`. Then you can fetch the content of those buffers by using the `uniform sampler2D u_buffer[number]` texture. Ex.:

```glsl
    uniform sampler2D   u_buffer0;
    uniform sampler2D   u_buffer1;

    varying vec2        v_texcoord;

    void main() {
        vec3 color = vec3(0.0);
        vec2 st = v_texcoord;

    #ifdef BUFFER_0
        color.g = abs(sin(u_time));

    #elif defined( BUFFER_1 )
        color.r = abs(sin(u_time));

    #else
        color.b = abs(sin(u_time));

        color = mix(color, 
                    mix(texture2D(u_buffer0, st).rgb, 
                        texture2D(u_buffer1, st).rgb, 
                        step(0.5, st.x) ), 
                    step(0.5, st.y));
    #endif
        
        gl_FragColor = vec4(color, 1.0);
    }
```

There is a more extense example on `examples/test_multibuffer.frag` and `examples/grayscott.frag`.

### Examples

* Open a Fragment shader:

```bash
glslViewer examples/test.frag
```

* Open a Fragment shader with an image:

```bash
glslViewer examples/test.frag examples/test.png
```

* Open a Fragment and Vertex shader with a geometry:

```bash
glslViewer examples/bunny.frag examples/bunny.vert examples/bunny.ply
```

* Open a Fragment that use two buffers as a Ping-Pong to do a reaction diffusion example:

```bash
glslViewer examples/grayscott.frag
```

* Open a Fragment that use OS defines to know what platform is running on:

```bash
glslViewer examples/test_platform.frag
```

* Change the value of a uniform by passing CSV on the console IN:

```bash
glslViewer examples/temp.frag
u_temp,30
u_temp,40
u_temp,50
u_temp,60
u_temp,70
```

* Create a bash pipe to animate the uniform value using the CPU temperature of your Raspberry PI:

```bash
examples/.temp.sh | glslViewer examples/temp.frag
```

* Run a headless instance of glslViewer that exits after 1 second outputting a high resolution PNG image:

```bash
glslViewer examples/mandelbrot.frag -w 2048 -h 2048 --headless -s 1 -o mandelbrot.png
```

* Make an SVG from a shader using [potrace](http://potrace.sourceforge.net/) and [ImageMagic](https://www.imagemagick.org/script/index.php):

```bash
glslViewer examples/cross.frag --headless -s 1 -o cross.png
convert cross.png cross.pnm
potrace cross.pnm -s -o cross.svg
```

## Using glslLoader

```glslLoader``` is a python script that is installed together with ```glslViewer``` binary which let you download any shader made with [The book of shaders editor (editor.thebookofshaders.com) ](http://editor.thebookofshaders.com/). Just pass as argument the ***log number***

![](http://patriciogonzalezvivo.com/images/glslGallery/00.gif)

```bash
glslLoader 170208142327
```


## Author

[Patricio Gonzalez Vivo](https://twitter.com/patriciogv): [github](https://github.com/patriciogonzalezvivo) | [twitter](https://twitter.com/patriciogv) | [website](http://patricio.io)


## Acknowledgements

Thanks to:

* [Karim’s Naaki](http://karim.naaji.fr/) lot of concept and code was inspired by his: [fragTool](https://github.com/karimnaaji/fragtool).

* [Doug Moen](https://github.com/doug-moen) he help to add the compatibility to ShaderToy shaders and some RayMarching features were added for his integration with his project: [curv](https://github.com/doug-moen/curv).

* [Yvan Sraka](https://github.com/yvan-sraka) for putting the code in shape and setting it up for TravisCI.
