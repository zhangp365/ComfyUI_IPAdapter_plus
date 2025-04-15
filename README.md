# ComfyUI IPAdapter plus
[ComfyUI](https://github.com/comfyanonymous/ComfyUI) reference implementation for [IPAdapter](https://github.com/tencent-ailab/IP-Adapter/) models.

The IPAdapter are very powerful models for image-to-image conditioning. The subject or even just the style of the reference image(s) can be easily transferred to a generation. Think of it as a 1-image lora.

> [!IMPORTANT]  
> **2025.04.14** - I do not use ComfyUI as my main way to interact with Gen AI anymore as a result I'm setting the repository in "maintenance only" mode. If there are crucial updates or PRs I might still consider merging them but I do not plan any consistent work on this repo.

## Example workflows

The [examples directory](./examples/) has many workflows that cover all IPAdapter functionalities.

![IPAdapter Example workflow](./examples/demo_workflow.jpg)

## Video Tutorials

<a href="https://youtu.be/_JzDcgKgghY" target="_blank">
 <img src="https://img.youtube.com/vi/_JzDcgKgghY/hqdefault.jpg" alt="Watch the video" />
</a>

- **:star: [New IPAdapter features](https://youtu.be/_JzDcgKgghY)**
- **:art: [IPAdapter Style and Composition](https://www.youtube.com/watch?v=czcgJnoDVd4)**

The following videos are about the previous version of IPAdapter, but they still contain valuable information.

:nerd_face: [Basic usage video](https://youtu.be/7m9ZZFU3HWo), :rocket: [Advanced features video](https://www.youtube.com/watch?v=mJQ62ly7jrg), :japanese_goblin: [Attention Masking video](https://www.youtube.com/watch?v=vqG1VXKteQg), :movie_camera: [Animation Features video](https://www.youtube.com/watch?v=ddYbhv3WgWw)

## Installation

Download or git clone this repository inside `ComfyUI/custom_nodes/` directory or use the Manager. IPAdapter always requires the latest version of ComfyUI. If something doesn't work be sure to upgrade. Beware that the automatic update of the manager sometimes doesn't work and you may need to upgrade manually.

There's now a *Unified Model Loader*, for it to work you need to name the files exactly as described below. The legacy loaders work with any file name but you have to select them manually. The models can be placed into sub-directories.

Remember you can also use any custom location setting an `ipadapter` entry in the `extra_model_paths.yaml` file.

- `/ComfyUI/models/clip_vision`
    - [CLIP-ViT-H-14-laion2B-s32B-b79K.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/image_encoder/model.safetensors), download and rename
    - [CLIP-ViT-bigG-14-laion2B-39B-b160k.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/image_encoder/model.safetensors), download and rename
    - [clip-vit-large-patch14-336.bin](https://huggingface.co/Kwai-Kolors/Kolors-IP-Adapter-Plus/resolve/main/image_encoder/pytorch_model.bin), download and rename only for Kolors models
- `/ComfyUI/models/ipadapter`, create it if not present
    - [ip-adapter_sd15.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter_sd15.safetensors), Basic model, average strength
    - [ip-adapter_sd15_light_v11.bin](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter_sd15_light_v11.bin), Light impact model
    - [ip-adapter-plus_sd15.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter-plus_sd15.safetensors), Plus model, very strong
    - [ip-adapter-plus-face_sd15.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter-plus-face_sd15.safetensors), Face model, portraits
    - [ip-adapter-full-face_sd15.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter-full-face_sd15.safetensors), Stronger face model, not necessarily better
    - [ip-adapter_sd15_vit-G.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter_sd15_vit-G.safetensors), Base model, **requires bigG clip vision encoder**
    - [ip-adapter_sdxl_vit-h.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter_sdxl_vit-h.safetensors), SDXL model
    - [ip-adapter-plus_sdxl_vit-h.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter-plus_sdxl_vit-h.safetensors), SDXL plus model
    - [ip-adapter-plus-face_sdxl_vit-h.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter-plus-face_sdxl_vit-h.safetensors), SDXL face model
    - [ip-adapter_sdxl.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/sdxl_models/ip-adapter_sdxl.safetensors), vit-G SDXL model, **requires bigG clip vision encoder**
    - **Deprecated** [ip-adapter_sd15_light.safetensors](https://huggingface.co/h94/IP-Adapter/resolve/main/models/ip-adapter_sd15_light.safetensors), v1.0 Light impact model

**FaceID** models require `insightface`, you need to install it in your ComfyUI environment. Check [this issue](https://github.com/cubiq/ComfyUI_IPAdapter_plus/issues/162) for help. Remember that most FaceID models also need a LoRA.

For the Unified Loader to work the files need to be named exactly as shown in the list below.

- `/ComfyUI/models/ipadapter`
    - [ip-adapter-faceid_sd15.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid_sd15.bin), base FaceID model
    - [ip-adapter-faceid-plusv2_sd15.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plusv2_sd15.bin), FaceID plus v2
    - [ip-adapter-faceid-portrait-v11_sd15.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-portrait-v11_sd15.bin), text prompt style transfer for portraits
    - [ip-adapter-faceid_sdxl.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid_sdxl.bin), SDXL base FaceID
    - [ip-adapter-faceid-plusv2_sdxl.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plusv2_sdxl.bin), SDXL plus v2
    - [ip-adapter-faceid-portrait_sdxl.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-portrait_sdxl.bin), SDXL text prompt style transfer
    - [ip-adapter-faceid-portrait_sdxl_unnorm.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-portrait_sdxl_unnorm.bin), very strong style transfer SDXL only
    - **Deprecated** [ip-adapter-faceid-plus_sd15.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plus_sd15.bin), FaceID plus v1 
    - **Deprecated** [ip-adapter-faceid-portrait_sd15.bin](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-portrait_sd15.bin), v1 of the portrait model

Most FaceID models require a LoRA. If you use the `IPAdapter Unified Loader FaceID` it will be loaded automatically if you follow the naming convention. Otherwise you have to load them manually, be careful each FaceID model has to be paired with its own specific LoRA.

- `/ComfyUI/models/loras`
    - [ip-adapter-faceid_sd15_lora.safetensors](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid_sd15_lora.safetensors)
    - [ip-adapter-faceid-plusv2_sd15_lora.safetensors](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plusv2_sd15_lora.safetensors)
    - [ip-adapter-faceid_sdxl_lora.safetensors](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid_sdxl_lora.safetensors), SDXL FaceID LoRA
    - [ip-adapter-faceid-plusv2_sdxl_lora.safetensors](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plusv2_sdxl_lora.safetensors), SDXL plus v2 LoRA
    - **Deprecated** [ip-adapter-faceid-plus_sd15_lora.safetensors](https://huggingface.co/h94/IP-Adapter-FaceID/resolve/main/ip-adapter-faceid-plus_sd15_lora.safetensors), LoRA for the deprecated FaceID plus v1 model

All models can be found on [huggingface](https://huggingface.co/h94).

### Community's models

The community has baked some interesting IPAdapter models.

- `/ComfyUI/models/ipadapter`
    - [ip_plus_composition_sd15.safetensors](https://huggingface.co/ostris/ip-composition-adapter/resolve/main/ip_plus_composition_sd15.safetensors), general composition ignoring style and content, more about it [here](https://huggingface.co/ostris/ip-composition-adapter)
    - [ip_plus_composition_sdxl.safetensors](https://huggingface.co/ostris/ip-composition-adapter/resolve/main/ip_plus_composition_sdxl.safetensors), SDXL version
    - [Kolors-IP-Adapter-Plus.bin](https://huggingface.co/Kwai-Kolors/Kolors-IP-Adapter-Plus/resolve/main/ip_adapter_plus_general.bin?download=true), IPAdapter Plus for Kolors model
    - [Kolors-IP-Adapter-FaceID-Plus.bin](https://huggingface.co/Kwai-Kolors/Kolors-IP-Adapter-FaceID-Plus/resolve/main/ipa-faceid-plus.bin?download=true), IPAdapter FaceIDv2 for Kolors model. **Note:** Kolors is trained on InsightFace  **antelopev2** model, you need to [manually download it](https://huggingface.co/MonsterMMORPG/tools/tree/main) and place it inside the `models/inisghtface` directory.

if you know of other models please let me know and I will add them to the unified loader.

## Generic suggestions

There are many workflows included in the [examples](./examples/) directory. Please check them before asking for support.

Usually it's a good idea to lower the `weight` to at least `0.8` and increase the number steps. To increase adherece to the prompt you may try to change the **weight type** in the `IPAdapter Advanced` node.

## Troubleshooting

Please check the [troubleshooting](https://github.com/cubiq/ComfyUI_IPAdapter_plus/issues/108) before posting a new issue. Also remember to check the previous closed issues.

## Credits

- [IPAdapter](https://github.com/tencent-ailab/IP-Adapter/)
- [InstantStyle](https://github.com/InstantStyle/InstantStyle)
- [B-Lora](https://github.com/yardenfren1996/B-LoRA/)
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- [laksjdjf](https://github.com/laksjdjf/)
