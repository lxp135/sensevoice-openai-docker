# sensevoice-openai-docker
基于SenseVoice的funasr版本进行openai格式的api发布，并以Docker部署

## Docker部署运行，Compose脚本示例
```
services:
  sensevoice:
    container_name: sensevoice-openai-docker
    user: "1000:1000"
    build:
      context: https://github.com/lxp135/sensevoice-openai-docker.git
      dockerfile: Dockerfile
    image: sensevoice-openai-docker:local
    networks:
      br0:
        ipv4_address: 192.168.50.90
    ports:
      - "8000:8000"
    volumes:
      - /mnt/user/appdata/sensevoice/iic:/app/iic
    environment:
      TZ: Asia/Shanghai
      DEVICE_TYPE: cpu
      VAD_ENABLE: "1"
      language: zh
      ncpu: "4"
      HOME: /app/iic
      XDG_CACHE_HOME: /app/iic/.cache
      MODELSCOPE_CACHE: /app/iic/modelscope
      MS_CACHE_HOME: /app/iic/modelscope
    restart: unless-stopped

networks:
  br0:
    external: true
```

### 指定推理方式
默认使用CPU
```
CPU：docker run时增加 -e DEVICE_TYPE=cpu
GPU：docker run时增加 -e DEVICE_TYPE=cuda:0
```

### 接口测试
```
curl --request POST 'http://127.0.0.1:8000/v1/audio/transcriptions' \
--header 'Content-Type: multipart/form-data' \
--form 'file=@audio/asr_example_zh.wav'
```
返回结果
{"text": "欢迎大家来体验达摩院推出的语音识别模型"}

### 其他应用接入
渠道类型使用OpenAI或者自定义渠道
模型填入whisper-1
代理填写对应的地址：http://ip:8000

### 终语
请施舍一个star吧
