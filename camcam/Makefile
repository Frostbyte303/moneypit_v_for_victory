
TOOLS:=mkpng mkyuv mkdetect mkchecker
CFILES:=$(wildcard *.c)
CPPFILES:=$(wildcard *.cpp)
C_O:=$(patsubst %.c,obj/%.o,$(CFILES))
CPP_O:=$(patsubst %.cpp,obj/%.o,$(CPPFILES))
LIBS:=-L/opt/vc/lib -lvcos -lmmal -lmmal_core -lmmal_util -lbcm_host -lpthread -lGL -lglut
OPT?=-O3
CFLAGS:=-I ../mpv_teensy -I/opt/vc/include -Wall -Werror $(OPT)
CPPFLAGS:=$(CFLAGS) -std=gnu++11
TOOL_O:=$(patsubst %,obj/%.o,$(TOOLS))
CAMCAM_O:=$(filter-out $(TOOL_O),$(C_O) $(CPP_O))

#test:	mkdetect mkchecker
#	#./mkchecker > checker.yuv
#	#./mkpng checker.yuv yuv
#	-rm -f /tmp/foo.png /tmp/square.png raw.png
#	./mkdetect dump /tmp/foo.png sample.yuv
#	feh /tmp/foo.png /tmp/square.png

all:	camcam $(TOOLS)

camcam:	$(CAMCAM_O)
	g++ -g -o $@ $(CAMCAM_O) $(LIBS) -std=gnu++11

mkpng:	obj/mkpng.o obj/imagewrite.o obj/yuv.o
	gcc -g -o $@ $^ -std=gnu11 -lm

mkyuv:	obj/mkyuv.o obj/imagewrite.o obj/yuv.o
	g++ -g -o $@ $^ -std=gnu++11 -lm

mkdetect:	obj/mkdetect.o obj/imagewrite.o obj/yuv.o obj/detect_inner.o obj/settings.o obj/project.o obj/queue.o
	g++ -g -o $@ $^ -std=gnu++11 -lm -lefence

mkchecker:	obj/mkchecker.o
	g++ -g -o $@ $^ -std=gnu++11 -lm

clean:
	rm -rf obj $(TOOLS)

obj/%.o:	%.c
	@mkdir -p obj
	gcc -g -c -o $@ $< -MMD $(CFLAGS) -std=gnu11

obj/%.o:	%.cpp
	@mkdir -p obj
	g++ -g -c -o $@ $< -MMD $(CPPFLAGS)

-include $(patsubst %.o,%.d,$(C_O) $(CPP_O))
