<p align="center" width="100%">
    <img width="38%" src="https://user-images.githubusercontent.com/10411078/149529602-7dcbaf26-55cd-4588-8989-206b76d32f07.png">
</p>


![XIU](https://img.shields.io/:XIU-blue.svg)[![crates.io](https://img.shields.io/crates/v/xiu.svg)](https://crates.io/crates/xiu)
[![crates.io](https://img.shields.io/crates/d/xiu.svg)](https://crates.io/crates/xiu)
![RTMP](https://img.shields.io/:RTMP-blue.svg)[![crates.io](https://img.shields.io/crates/v/rtmp.svg)](https://crates.io/crates/rtmp)
[![crates.io](https://img.shields.io/crates/d/rtmp.svg)](https://crates.io/crates/rtmp)
![HTTPFLV](https://img.shields.io/:HTTPFLV-blue.svg)[![crates.io](https://img.shields.io/crates/v/httpflv.svg)](https://crates.io/crates/httpflv)
[![crates.io](https://img.shields.io/crates/d/httpflv.svg)](https://crates.io/crates/httpflv)
![HLS](https://img.shields.io/:HLS-blue.svg)[![crates.io](https://img.shields.io/crates/v/hls.svg)](https://crates.io/crates/hls)
[![crates.io](https://img.shields.io/crates/d/hls.svg)](https://crates.io/crates/hls)
![FLV](https://img.shields.io/:FLV-blue.svg)[![crates.io](https://img.shields.io/crates/v/xflv.svg)](https://crates.io/crates/xflv)
[![crates.io](https://img.shields.io/crates/d/xflv.svg)](https://crates.io/crates/xflv)
![MPEGTS](https://img.shields.io/:MPEGTS-blue.svg)[![crates.io](https://img.shields.io/crates/v/xmpegts.svg)](https://crates.io/crates/xmpegts)
[![crates.io](https://img.shields.io/crates/d/xmpegts.svg)](https://crates.io/crates/xmpegts)
[![](https://app.travis-ci.com/harlanc/xiu.svg?branch=master)](https://app.travis-ci.com/github/harlanc/xiu)
[![](https://img.shields.io/discord/894502149764034560?logo=discord)](https://discord.gg/gS5wBRtpcB)
![wechat](https://img.shields.io/:微信-harlancc-blue.svg)
![qqgroup](https://img.shields.io/:QQ群-24893069-blue.svg)


XIU是用纯Rust开发的一款简单和安全的流媒体服务器，目前支持流行的三大流媒体协议包括RTMP/HLS/HTTPFLV，可以单点部署，也可以用relay功能来部署集群。

## 功能

- [x] RTMP
  - [x] 发布直播流和播放直播流
  - [x] 转发：静态转推和静态回源
- [x] HTTPFLV
- [x] HLS
- [ ] SRT

## 准备工作
#### 安装 Rust and Cargo


[Document](https://doc.rust-lang.org/cargo/getting-started/installation.html)

## 安装和运行

有两种方式来安装xiu：
 
 - 直接用cargo来安装
 - 源码编译安装


### 用cargo命令安装

执行下面的命令来安转xiu:

    cargo install xiu
    
执行下面的命令来查看帮助信息:

    xiu -h
 
    A secure and easy to use live media server, hope you love it!!!

    Usage: xiu [OPTIONS] <--config <path>|--rtmp <port>>

    Options:
      -c, --config <path>   Specify the xiu server configuration file path.
      -r, --rtmp <port>     Specify the RTMP listening port(e.g.:1935).
      -f, --httpflv <port>  Specify the HTTP-FLV listening port(e.g.:8080).
      -s, --hls <port>      Specify the HLS listening port(e.g.:8081).
      -l, --log <level>     Specify the log level. [possible values: trace, debug, info, warn, error, debug]
      -h, --help            Print help
      -V, --version         Print version
    
### 源码编译安装

#### 克隆 Xiu

    git clone https://github.com/harlanc/xiu.git
 Checkout最新发布的版本代码：
 
    git checkout tags/<tag_name> -b <branch_name>
    
#### 编译

    cd ./xiu/application/xiu
    cargo build --release
#### 运行

    cd ./xiu/target/release
    ./xiu -h
    
## CLI

#### 说明

可以使用配置文件或者在命令行对服务进行配置。比如：

##### 通过配置文件进行配置

    xiu -c configuration_file_path

##### 通过命令行

    xiu -r 1935 -f 8080 -s 8081 -l info


#### 配置文件说明

##### RTMP
    [rtmp]
    enabled = true
    port = 1935

    # pull streams from other server node.
    [rtmp.pull]
    enabled = false
    address = "192.168.0.1"
    port = 1935

    # push streams to other server node.
    [[rtmp.push]]
    enabled = true
    address = "localhost"
    port = 1936
    [[rtmp.push]]
    enabled = true
    address = "192.168.0.3"
    port = 1935
    
##### HTTPFLV

    [httpflv]
    # true or false to enable or disable the feature
    enabled = true
    # listening port
    port = 8081

##### HLS
    [hls]
    # true or false to enable or disable the feature
    enabled = true
    # listening port
    port = 8080

##### Log

    [log]
    level = "info"
    [log.file]
    # 打开或者关闭输出日志到文件（注意：输出日志到控制台和文件只能2选1）.
    enabled = true
    # set the rotate
    rotate = "hour" #[day,hour,minute]
    # set the path where the logs are saved
    path = "./logs"

### 一些配置的例子

有一些现成的配置文件放在下面的目录：

    xiu/application/xiu/src/config

包括4个配置文件：

    config_rtmp.toml //只打开rtmp
    config_rtmp_hls.toml //打开 rtmp 和 hls
    config_rtmp_httpflv.toml //打开 rtmp 和 httpflv
    config_rtmp_httpflv_hls.toml //打开所有的 3 个协议
    

    
## 应用场景

##### 推流

可以用任何推流软件或者命令工具来推RTMP流，比如使用OBS或者用ffmpeg命令行：

    ffmpeg -re -stream_loop -1 -i test.mp4 -c:a copy -c:v copy -f flv -flvflags no_duration_filesize rtmp://127.0.0.1:1935/live/test


##### 播放

使用ffplay来播放 rtmp/httpflv/hls协议的直播流:

    ffplay -i rtmp://localhost:1935/live/test
    ffplay -i http://localhost:8081/live/test.flv
    ffplay -i http://localhost:8080/live/test/test.m3u8
    
##### 转发 - 静态转推

应用场景为边缘节点的直播流被转推到源站，配置如下：

边缘节点的配置文件config_push.toml:

    [rtmp]
    enabled = true
    port = 1935
    [[rtmp.push]]
    enabled = true
    address = "localhost"
    port = 1936
    
源站节点的配置文件config.toml:

    [rtmp]
    enabled = true
    port = 1936

启动两个服务:

    ./xiu config.toml
    ./xiu config_push.toml

将一路RTMP直播流推送到边缘节点，此直播流会被自动转推到源站，可以同时播放源站或者边缘节点的直播流：

    ffplay -i rtmp://localhost:1935/live/test
    ffplay -i rtmp://localhost:1936/live/test


    
##### 转发 - 静态回源

应用场景为播放过程中用户从边缘节点拉流，边缘节点无此流，则回源拉流，配置文件如下：

源站节点的配置文件为 config.toml:

    [rtmp]
    enabled = true
    port = 1935

 
边缘节点的配置文件为 config_pull.toml:

    [rtmp]
    enabled = true
    port = 1936
    [rtmp.pull]
    enabled = false
    address = "localhost"
    port = 1935

运行两个服务:

    ./xiu config.toml
    ./xiu config_pull.toml
    
直接将直播流推送到源站，到边缘节点请求此路直播流，边缘节点会回源拉流，可以同时播放边缘和源站节点上的直播流：

    ffplay -i rtmp://localhost:1935/live/test
    ffplay -i rtmp://localhost:1936/live/test
    
## Star History

[link](https://star-history.t9t.io/#harlanc/xiu)

## 鸣谢

 - [media_server](https://github.com/ireader/media-server.git)

## 其它

有任何问题请在issues提问，欢迎star和提pull request。你的关注可以让此项目走的更快更远。
 
