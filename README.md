# RSGL_UI
![The RSGL logo](https://github.com/ColleagueRiley/RSGL/blob/main/RSGL_logo.png?raw=true)

A Single-header UI extension for RSGL 
---
RSGL is a modular simple-to-use cross-platform graphics library. It attempts to combine the freedoms of low libraries with modern C techniques. Thus offering simplicity and convenience.

The RSGL.h header itself does not handle rendering nor input devices itself, both must be handled externally.  

`RSGL_gl.h` is used by default for rendering, it renders via OpenGL 1.0 - 4.4.

`RSGL_rgfw.h` is used by default for handling input devices. There is also a example GLFW backend, `examples/advanced/RSGL_glfw.h`

# Build statuses
![Linux workflow](https://github.com/ColleagueRiley/RSGL_ui/actions/workflows/linux.yml/badge.svg)
![Windows workflow windows](https://github.com/ColleagueRiley/RSGL_ui/actions/workflows/windows.yml/badge.svg)
![MacOS workflow windows](https://github.com/ColleagueRiley/RSGL_ui/actions/workflows/macos.yml/badge.svg)

# Features
- Easily swappable rendering module 
- Easily swappable the backend module
- Small widget library with dynamic GUI via a convenient styling system
- (RSGL_rgfw.h OR RSGL_glfw.h) (for RSGL_ui.h) Supports multiple platforms, Windows, MacOS, Linux, etc

# Contacts
- email : ColleagueRiley@gmail.com 
- discord : ColleagueRiley
- discord server : https://discord.gg/pXVNgVVbvh

# RSGL_ui 
RSGL_ui widgets are WIP, the supported widgets include\
RSGL_alignRect, RSGL_button (checkboxes, toggle buttons, combo boxes, sliders and radio buttons) and widget containers \

RSGL_textBox and RSGL_expandableRect are currently WIP.

# Using RSGL_ui

## header-only
  To link RSGL you must add the line.
  ```c
  #define RSGL_IMPLEMENTATION
  ```
  in exactly one file which includes the `RSGL.h` header.
  
  You could also use the command line argument `-D RSGL_IMPLEMENTATION`.

  Optionally, RSGL can be compiled into a .o or .so file and link it.

## Compiling RSGL
  Compiling RSGL isn't required. But if you want to, here's how.

  ```sh
  make
  ```
  
  This will compile RSGL into a static library into a shared library.
  You can also compile RSGL by hand.

### compiling by hand
1) Compile the library into an object file by running `gcc -x c -c RSGL.h -fPIC`.
2) After you compile the library into an object file, you can also turn the object file into a static or shared library.
3) To compile statically run `ar rcs RSGL.a RSGL.o`
4) To compile RSGL into a shared library, run `gcc -shared RSGL.o <system libs>`
```
  windows:
    gcc -shared RSGL.o  -lshell32 -lgdi32 -lwinmm -o RSGL.dll
  linux:
    gcc -shared RSGL.o -lX11 -lXcursor -lXrandr -o RSGL.so
  macos:
    gcc -shared RSGL.o -framework Foundation -framework AppKit -framework CoreVideo
```

# Examples

## a basic example

```c
#define RSGL_IMPLEMENTATION
#include "RSGL_rgfw.h" // load whatever windowing backend you want

int main(void) {
    RGFW_setGLSamples(12);
    RGFW_window* win = RGFW_createWindow("name", RGFW_RECT(500, 500, 500, 500), RGFW_CENTER);

	RSGL_init(RSGL_AREA(win->r.w, win->r.h), RGFW_getProcAddress);

	RSGL_setFont(RSGL_loadFont("Super Easy.ttf"));
	
    float slider_value = 0.0;	
	b8 slider_grabbed = 0,
	   checkbox = 0,
	   toggle = 0,
	   combo_open = 1;

	size_t combo_index = 0;
	u8 selected = 0, index;


    while (RGFW_window_shouldClose(win) == false) {
        while (RSGL_checkEvent(win)) {
            if (win->event.type == RGFW_quit) {
                break;
            }
        }
		
		RSGL_openBlankContainer(RSGL_RECT(0, 0, win->r.w, win->r.h));
		
		RSGL_widgetAlign(RSGL_ALIGN_CENTER | RSGL_ALIGN_MIDDLE);
		RSGL_labeledButton("generic", RSGL_RECTF(50, 50, 100, 50), RSGL_OFFSET_Y | RSGL_STYLE_DARK | RSGL_STYLE_ROUND);
		
		RSGL_widgetRounding(RSGL_POINT(20, 20));
		RSGL_toggleButton(RSGL_RECTF(50, 50, 100, 50), RSGL_STYLE_DARK | RSGL_STYLE_ROUND | RSGL_OFFSET_Y, &toggle);
	
		RSGL_widgetPolygonPoints(8);
		
		RSGL_widgetColor(RSGL_RGB(200, 0, 0), RSGL_RGB(200, 0, 0), RSGL_RGB(255, 0, 0), RSGL_RGB(200, 0, 0));
		RSGL_widgetOutline(10, RSGL_RGB(100, 0, 0), RSGL_RGB(100, 0, 0), RSGL_RGB(100, 0, 0), RSGL_RGB(100, 0, 0));

		if (RSGL_button(RSGL_RECTF(200, 200, 200, 200), RSGL_SHAPE_POLYGON) == RSGL_PRESSED) {
			RSGL_label("other text", RSGL_RECTF(200, 200, 200, 200));
		}
		else {
			RSGL_label("text", RSGL_RECTF(200, 200, 200, 200));
		}
				
		RSGL_widgetAlign(RSGL_ALIGN_LEFT | RSGL_ALIGN_MIDDLE);
		
		char* combos[3] = {"comboBox 0", "comboBox 1", "comboBox 2"};
		RSGL_combobox(RSGL_RECTF(200, 50, 200, 50), RSGL_STYLE_DARK, combos, 3, &combo_open, &combo_index); 

		RSGL_checkbox(RSGL_RECTF(50, 250, 50, 50), RSGL_STYLE_DARK, &checkbox);	 		
	
		RSGL_widgetPolygonPoints(100);
		RSGL_radioButtons(RSGL_RECTF(50, 320, 32, 32), 3, RSGL_STYLE_DARK | RSGL_SHAPE_POLYGON, &selected, &index); 
		
		RSGL_widgetRounding(RSGL_POINT(10, 10));
		RSGL_slider(RSGL_RECTF(160, 450, 260, 15), RSGL_STYLE_DARK | RSGL_STYLE_ROUND | RSGL_SLIDER_CIRCLE, &slider_value, &slider_grabbed);
		
		RSGL_widgetOutline(0, RSGL_RGB(255, 255, 255),  RSGL_RGB(255, 255, 255),  RSGL_RGB(255, 255, 255),  RSGL_RGB(255, 255, 255));
		RSGL_widgetAlign(RSGL_ALIGN_LEFT | RSGL_ALIGN_DOWN);

		RSGL_label(RSGL_strFmt("slider : %i", (i32)(slider_value * 100)), RSGL_RECTF(50, 450, 370, 15));
		
        RSGL_clear(RSGL_RGB(20, 20, 60));
		RGFW_window_swapBuffers(win);
    }

	RSGL_free();
    RGFW_window_close(win);
}
```

This example can be compiled with :\
Linux:\
`gcc <file.c> -lX11 -lGLX -lm -o example`\
Windows :\
`gcc <file.c> -lopengl32 -lgdi32 -lshell32 -o example`\
MacOS :\
`gcc <file.c> -lm -framework Foundation -framework AppKit -framework OpenGL -framework CoreVideo -o example`

Both of the included makefiles  (`Makefile` or `examples/Makefile`) use the Unlicense license so feel free to copy from either of them if you wish. 

## compiling 
these can be compiled by running `make examples` in the current directory\

or by using the makefile in `examples/`\

The Makefile in `examples/`  allows you to compile all the examples using `make`,\
compile one specific example using `make <example>` or\
run `make debug` which compiles and runs each example in debug mode

Ensure you're running the example in the `./examples` folder so the fonts are properly loaded. 

NOTE: Most of these examples use `RSGL_rgfw.h`, but you can use whatever backend you want.

### RSGL_ui.h
![example screenshot 2](https://github.com/ColleagueRiley/RSGL/blob/main/screenshot2.PNG?raw=true)
![example screenshot 3](https://github.com/ColleagueRiley/RSGL/blob/main/screenshot3.PNG?raw=true)
![example screenshot 4](https://github.com/ColleagueRiley/RSGL/blob/main/screenshot4.PNG?raw=true)
![example screenshot 5](https://github.com/ColleagueRiley/RSGL/blob/main/screenshot5.PNG?raw=true)

## advanced/glfw.c
`examples/advanced/glfw.c` is an example that shows how you can use RSGL with GLFW

This example requires GLFW to be installed.\
You can download GLFW [here](https://www.glfw.org/download)

## button.c 
`examples/button.c` is an example that shows off how to create and manage buttons using RSGL,
these include, a default style button, a checkbox, a toggle button, radio buttons, a combo box, a slider and a custom button

## styles.c
`examples/styles.c` is an example that shows off button styles, there are groups of buttons for each style.
There is also a switch button that allows you to toggle dark mode.

## container.c
`examples/container.c` is an example that shows how to create and manage a widget container

# License
RSGL uses the Zlib/libPNG license, this means you can use RSGL freely as long as you do not claim you wrote this software, mark altered versions as such and keep the license included with the header.

```
Permission is granted to anyone to use this software for any purpose,
including commercial applications, and to alter it and redistribute it
freely, subject to the following restrictions:
  
1. The origin of this software must not be misrepresented; you must not
   claim that you wrote the original software. If you use this software
   in a product, an acknowledgment in the product documentation would be
   appreciated but is not required. 
2. Altered source versions must be plainly marked as such, and must not be
   misrepresented as being the original software.
3. This notice may not be removed or altered from any source distribution.
```