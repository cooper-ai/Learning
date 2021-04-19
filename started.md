`Linux setup scripts`

conda create -n open-mmlab python=3.7 -y

conda activate open-mmlab

conda install pytorch=1.6.0 torchvision cudatoolkit=10.1 -c pytorch

pip install mmcv-full -f https://download.openmmlab.com/mmcv/dist/cu101/torch1.6.0/index.html

git clone https://github.com/open-mmlab/mmsegmentation.git

cd mmsegmentation

pip install -e . # or "python setup.py develop"

mkdir data

ln -s $DATA_ROOT data

`verification`

python demo/image_demo.py demo/demo.png configs/deeplabv3plus/deeplabv3plus_r50-d8_512x1024_40k_cityscapes.py \

checkpoints/deeplabv3plus_r50-d8_512x1024_40k_cityscapes_20200605_094610-d222ffcd.pth --device cuda:0 --palette cityscapes

`Test PSPNet and visualize the results. Press any key for the next image.`

python tools/test.py configs/pspnet/pspnet_r50-d8_512x1024_40k_cityscapes.py \
    checkpoints/pspnet_r50-d8_512x1024_40k_cityscapes_20200605_003338-2966598c.pth \
    --show

`test a datasets with a single GPU`
python tools/test.py
configs/deeplabv3plus/deeplabv3plus_r50-d8_512x1024_40k_cityscapes.py
checkpoints/deeplabv3plus_r50-d8_512x1024_40k_cityscapes_20200605_094610-d222ffcd.pth
--show-dir deeplabv3plus_r50-d8_512x1024_40k_cityscapes_results

`CPU memory efficient test DeeplabV3+ on Cityscapes (without saving the test results) and evaluate the mIoU.`
python tools/test.py
configs/deeplabv3plus/deeplabv3plus_r50-d8_512x1024_40k_cityscapes.py
checkpoints/deeplabv3plus_r50-d8_512x1024_40k_cityscapes_20200605_094610-d222ffcd.pth
--eval-options efficient_test=True
--eval mIoU

`CPU memory efficient test pspnet on Cityscapes (without saving the test results) and evaluate the mIoU. `
python tools/test.py configs/pspnet/pspnet_r50-d8_512x1024_40k_cityscapes.py checkpoints/pspnet_r50-d8_512x1024_40k_cityscapes_20200605_003338-2966598c.pth --eval-options efficient_test=True --eval mIoU


[安装桌面环境并配置VNC] https://cloud.videojj.com/bbs/topic/272/%E5%AE%89%E8%A3%85%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83%E5%B9%B6%E9%85%8D%E7%BD%AEvnc%E8%BF%9E%E6%8E%A5
#启动 vncserver 之前需要把 tensorboard 服务或者 jupyter 服务停掉，然后把vncserver 用对应的端口启动。
supervisorctl stop tensorboard     # 关闭 tensorboard

# 启动 vncserver 的命令
vncserver :1 -desktop X -auth /root/.Xauthority -geometry 1920x1080 -depth 24 -rfbwait 120000 -rfbauth /root/.vnc/passwd -fp /usr/share/fonts/X11/misc/,/usr/share/fonts -rfbport 6006

# 关闭 vncserver 的命令
vncserver -kill :1


#vnc非正常关闭导致。服务器上pid文件及锁文件没有被删除，需要手动删除后重新启动vnc
rm -f ~/.vnc/*.pid
rm -rf /tmp/.X1*
rm -rf /tmp/.X11-unix
