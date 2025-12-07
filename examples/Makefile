CXX = g++
CXXFLAGS = -std=c++17 -Wall -Wextra -O2

# OpenCV (try opencv4 first, fallback to opencv)
OPENCV_PKG := $(shell pkg-config --exists opencv4 && echo opencv4 || echo opencv)
OPENCV_CFLAGS := $(shell pkg-config --cflags $(OPENCV_PKG))
OPENCV_LDFLAGS := $(shell pkg-config --libs $(OPENCV_PKG))

# Librtsplink
RTSPLINK_CFLAGS := $(shell pkg-config --cflags librtsplink 2>/dev/null || echo -I/usr/local/include/librtsplink)
RTSPLINK_LDFLAGS := $(shell pkg-config --libs librtsplink 2>/dev/null || echo -L/usr/local/lib -lrtsplink)

TARGET = camera_example
SRCS = main.cpp
OBJS = $(SRCS:.cpp=.o)

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(RTSPLINK_LDFLAGS) $(OPENCV_LDFLAGS)

%.o: %.cpp
	$(CXX) $(CXXFLAGS) $(RTSPLINK_CFLAGS) $(OPENCV_CFLAGS) -c $< -o $@

clean:
	rm -f $(TARGET) $(OBJS)

