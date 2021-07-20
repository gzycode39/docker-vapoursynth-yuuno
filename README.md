# Docker-VapourSynth-Yuuno

## FRDS压制简明教程

1. 自行准备Linux环境与docker环境，包括docker-compose
2. `git clone https://github.com/gzycode39/docker-vapoursynth-yuuno.git`
3. `cd docker-vapoursynth-yuuno`
4. 将docker-compose.yml中的`/YOUR/ENCODE/PATH`修改为你的工作目录（即原盘所在目录），假设原盘名为`HLWYYDS.2021.HLW`，宿主机目录结构为`/home/hlw/HLW/HLWYYDS.2021.HLW/`，则工作目录应为`/home/hlw/HLW`
5. `docker-compose up -d`
6. `docker exec -it vapoursynth-yuuno /bin/bash`
7. `frds /encode/HLWYYDS.2021.HLW/`
8. 按照提示选择轨道
9. 耐心等待提取完成，在浏览器输入`你的机器ip:8888`（初始密码123），打开名为`HLWYYDS.2021.HLW`的文件，修改第9行`border=  xxx`切边条数，有需要可自行添加脏边修复代码，在浏览器中预览确认无误后，回到命令行界面按任意键继续
10. 进入宿主机目录`/home/hlw/HLW/out-HLWYYDS.2021.HLW`查看成片，种子及mediainfo位于`torrent`目录下
11. 发布成片

## Software Versions

- python3: 3.8.5
- eac3to: v3.34
- x265: v3.5
- mkvmerge: v53.0.0
- mediainfo: v19.09
- mktorrent: v1.1

## How to build the environment

```
docker-compose up -d
```

## How to enter the container

```
docker exec -it vapoursynth-yuuno /bin/bash
```

## How to encode a movie

Please refer to [WhaleHu/Encode-guide-frds](https://github.com/WhaleHu/Encode-guide-frds)

## How to use eac3to to demux video tracks, audio tracks, and so on

```
eac3to-demux /PATH/TO/BDMV
```

## How to encode TV series

```
encode_series "/PATH/TO/SERIES/REMUX"
```

## How to convert dts to ac3

```
eac3to "xxx.dtsma" "xxx.ac3"
```

## How to use yuuno

1. Access your_ip:8888, the default password is __123__.

2. Open the notebook __encode.ipynb__

3. to be continued

# Tutorial from Krita

### 一.环境准备

#### 1.Docker部署

##### ①.拉取文件：

```
git clone https://github.com/gzycode39/docker-vapoursynth-yuuno.git
```

##### ②.进入项目目录

```
cd docker-vapoursynth-yuuno
```

##### ③.启动容器：

```
docker-compose up -d
```

##### ④.进入容器：

```
docker exec -it vapoursynth-yuuno /bin/bash
```

##### ⑤.删除容器

```
docker-compose ps

docker-compose down
```

##### ⑥.更新镜像

```
docker pull yyfyyf/vapoursynth-yuuno:v0.X
X为最新版本号
```

#### 2.拉取原盘，压片前的装备

##### ①.拉取原盘文件至容器外对应/encode的目录下

##### ②.查看原盘目录结构

```
eac3to 目录
```

##### ③.找到对应的（最大的）mpls，并提取其全部文件到当前目录

```
eac3to-demux 目录
```

##### ④.进入jupyter notebook

```
ip:映射端口
```

### 二.压制样片

#### 1.使用现成脚本或新建python3脚本

```
先输一行%load_ext yuuno
然后下面输入%%vspreview再接脚本代码
run
```

#### 2.查看并记录原片及样片帧数，进行切黑边，修脏边等操作



#### 3.压制样片

把ipynb的脚本内容复制一份到vpy，修改sh脚本中的帧数为样片帧数，修改vpy为对应vpy

运行sh脚本，压制样片

观察码率



### 三.样片与原片进行对比

#### 1.在ipynb加入抽取对比



#### 2.对比样片图片及原片图片



#### 3.根据情况对crf值进行0.5步进微调



#### 4.可以则开始正片压制，不可则重新压制样片，重新进行对比



### 四.压制成片

#### 1.修改sh脚本中帧数为原片帧数，执行sh脚本进行压制

#### 2.等待压制完成



#### 五.进行成片与原片对比



### 六.混流封装及制种

#### 1.音轨转换

```
eac3to "原音轨.dtsma" 新音轨.ac3
```

#### 2.混流封装

```
mkvmerge -o "电影名.年份.bluray.1080p.x265.10bit.MNHD-FRDS.mkv" --title "电影名 [年份] Bluray 1080p MNHD-FRDS" --chapters "原盘目录.txt" --compression 0:none --default-duration 0:24000/1001fps --track-name 0:作者署名 成片.hevc --compression 0:none --default-track 0 --language 0:ISO编码 音轨.ac3 --compression 0:none --default-track 0 --language 0:chi --track-name 0:CHS "简中字幕.sup" --compression 0:none --language 0:chi --track-name 0:CHT "繁中字母.sup" --compression 0:none --language 0:原始字幕语言编码 --track-name 0:原始字幕语言编码 "原始语言字幕.sup"
```

#### 3.制种

```
mktorrent -a tracker地址 -l 22 -p 成片目录
```

