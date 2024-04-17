
Clone the repo
```
git clone https://github.com/wisleythewise/train_controlnet.git
```

Create a new enviroment

```
conda create --name training_controlnet python=3.9
conda activate training_controlnet
```

Install dependencies

```
pip install git+https://github.com/huggingface/diffusers.git transformers accelerate xformers wandb datasets torchvision torch matplotlib

```

Login to huggingface and pass you acces token using the following command, we use huggingface to push the model
```
huggingface-cli login
```

Login to wanddb and pass your acces token using the following command, we use wanddb to check the progress of the training

```
wandb login 
```


run the training script make sure you have provided the right paths

```
accelerate launch train_controlnet.py \
 --pretrained_model_name_or_path="runwayml/stable-diffusion-v1-5" \
 --controlnet_model_name_or_path="lllyasviel/sd-controlnet-seg" \
 --output_dir="/home/wisley/train_controlnet/output" \
 --dataset_name="/home/wisley/train_controlnet/data/JaspervanLeuven___nul_images_cropped" \
 --conditioning_image_column="conditioning_image" \
 --image_column="groundTruth" \
 --caption_column="caption" \
 --hub_model_id="JaspervanLeuven/controlnet5k" \
 --resolution=512 \
 --learning_rate=1e-5 \
 --train_batch_size=4 \
 --validation_steps=100\
 --num_train_epochs=8 \
 --tracker_project_name="controlnet11k" \
 --checkpointing_steps=500 \
 --report_to="wandb" \
 --push_to_hub \
 --set_grads_to_none \
 --gradient_checkpointing \
 --mixed_precision="no" \
 --cache_dir="/home/wisley/train_controlnet/cache/" \
 --validation_prompt="A driving scene"\
 --validation_image="/home/wisley/train_controlnet/validation.png"\
```

After training you can run inference in the model using the inference.py file specifying the hub_model_id