# Docker-VapourSynth-Yuuno

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

## How to convert dts to ac3

```
eac3to "xxx.dtsma" "xxx.ac3"
```

## How to use yuuno

1. Access your_ip:8888, the default password is __123__.

2. Open the notebook __encode.ipynb__

3. to be continued
