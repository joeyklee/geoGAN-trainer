# geoGAN

## Urban (trained on santiago)

1. resize images

```
for f in /Users/joeyklee/Desktop/geoGAN-trainer/images/santiago_urban/*; do 
        gdal_translate -of PNG -outsize 64 64 -r cubic $f "${f%.*}-s64.png"
done

```


1. 

```bash
python datatool.py --task dir_to_npz --dir_path images/santiago_urban/s64 --npz_path npzdata/img_santiago_urban_64.npz --size 64
```



spell upload images/bc/JPEG



pip install tensorflow
pip install image
pip install chainer


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
    --npz_path npzdata/img_bc_jpg_64.npz \
    --out out_chainer_dcgan64"