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
