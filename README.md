# nvidia-docker image for EWBF's CUDA Zcash miner

This image build [EWBF's CUDA Zcash miner] from EWBF's Google Drive.
It requires a CUDA compatible docker implementation so you should probably go
for [nvidia-docker].
It has also been tested successfully on [Mesos] 1.2.1.

## Build images

```
git clone https://github.com/EarthLab-Luxembourg/docker-ewbf-cuda-zcash-miner
cd docker-ewbf-cuda-zcash-miner
docker build -t ewbf-cuda-zcash-miner .
```

## Test it locally

```
nvidia-docker run -it --rm ewbf-cuda-zcash-miner /root/miner --help
```

An example command line to mine using miningpoolhub.com:
```
nvidia-docker run -it --rm /root/miner --server europe.equihash-hub.miningpoolhub.com --port 20570 --user <login>.<worker_name> --pass x --fee 0 --api 0.0.0.0:$PORT0",

```


## Publish it somewhere

```
docker tag ewbf-cuda-zcash-miner docker.domain.com/mining/ewbf-cuda-zcash-miner
docker push docker.domain.com/mining/ewbf-cuda-zcash-miner
```

## Use it with Mesos/Marathon

Edit `mesos_marathon.json` to replace Zcash mining pool parameter, change application path as well as docker image address (if you dont want to use public docker image provided).
Then simply run (adapt application name here too):

```
curl -X PUT -u marathon\_username:marathon\_password --header 'Content-Type: application/json' "http://marathon.domain.com:8080/v2/apps/mining/ewbf-cuda-zcash-miner?force=true" -d@./mesos\_marathon.json
```

You can check CUDA usage on the mesos slave (executor host) by running `nvidia-smi` there:

```
Fri Aug 18 15:34:37 2017       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 375.66                 Driver Version: 375.66                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    Off  | 0000:82:00.0     Off |                  N/A |
| 46%   65C    P2   178W / 180W |    629MiB /  8114MiB |    100%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|    0     29111    C   /root/miner                                    627MiB |
+-----------------------------------------------------------------------------+
```

[EWBF's CUDA Zcash miner]: https://bitcointalk.org/index.php?topic=1707546.0
[nvidia-docker]: https://github.com/NVIDIA/nvidia-docker
[Mesos]: http://mesos.apache.org/documentation/latest/gpu-support/
