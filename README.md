# geoGAN-trainer

The following steps and code are attributed to:
+ YG's 
+ https://github.com/alantian/ganshowcase
  

## (Optional) crop images
Follow along with cropping here: https://ellennickles.site/blog?category=Neural+Aesthetic


## (Optional) resize images from 128x128 to 64x64

```
for f in /Users/joeyklee/Desktop/geoGAN-trainer/images/santiago_urban/*; do 
        gdal_translate -of PNG -outsize 64 64 -r cubic $f "${f%.*}-s64.png"
done

```
Then get all those images and put them into a folder. In this case `santiago_urban64`


## Create a virtualenv

```
virtualenv /geoGAN-trainer
```

## Activate your virtualenv

```
source /geoGAN-trainer/bin/activate
```

## 2. install deps

```
pip install tensorflow image chainer spell
pip install tensorflowjs==1.1.2 h5py numpy keras
pip install --no-deps tensorflowjs
pip install tensorflow_hub
```

<!-- tensorflow==tf-nightly-2.0-preview>=2.0.0.dev20190502 h5py==2.8.0 numpy==1.15.1 six==1.11.0  tensorflow-hub==0.3.0 -->


## 3. Convert images to .npz file

```bash
python datatool.py --task dir_to_npz --dir_path images/santiago_urban64 --npz_path npzdata/img_santiago_urban_64.npz --size 64
```


## 4. spell login

```
spell login
# username
# password
```

## 5. run the thing on spell

This will take anywhere from 2 - X hours depending on the machine and number of images

```
spell run --machine-type V100 \
            --framework tensorflow \
            --pip image \
            --pip chainer \
            --pip cupy \
    "python chainer_dcgan.py \
    --arch dcgan64 \
    --image_size 64 \
    --adam_alpha 0.0001 \
    --adam_beta1 0.5 \
    --adam_beta2 0.999 \
    --lambda_gp 1.0 \
    --learning_rate_anneal 0.9 \
    --learning_rate_anneal_trigger 0 \
    --learning_rate_anneal_interval 5000 \
    --max_iter 100000 --snapshot_interval 5000 \
    --evaluation_sample_interval 100 \
    --display_interval 10 \
    --npz_path npzdata/img_santiago_urban_64.npz \
    --out out_chainer_dcgan64"
```


## 6. convert the Generator file to a tfjs model

python dcgan_chainer_to_keras.py --arch dcgan64 --chainer_model_path out_chainer_dcgan64/Generator_95000.npz --keras_model_path out_chainer_dcgan64/Keras_Generator_95000.h5 --tfjs_model_path out_chainer_dcgan64/tfjs_Generator_95000