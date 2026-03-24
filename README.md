<!-- <h2 align="center"> <a href="https://rover-xingyu.github.io/TTT3R">PAS3R: Pose-Adaptive Streaming 3D Reconstruction for Long Video Sequences</a> -->
<!-- </h2> -->

<h2 align="center">PAS3R: Pose-Adaptive Streaming 3D Reconstruction for Long Video Sequences</h2>

<div align="center">
  <video src="https://github.com/user-attachments/assets/654c1058-f229-4bbd-be7b-8bea0cffd7b8" 
         width="100%" 
         controls="controls" 
         muted="muted" 
         autoplay="autoplay" 
         loop="loop" 
         style="max-width: 800px;">
  </video>
</div>

## Getting Started

### Installation

1. Clone PAS3R.
```bash
git clone https://github.com/xlbjyxqc/PAS3R.git
cd PAS3R
```

2. Create the environment.
```bash
conda create -n pas3r python=3.11 cmake=3.14.0
conda activate pas3r
conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia  # use the correct version of cuda for your system
pip install -r requirements.txt
# issues with pytorch dataloader, see https://github.com/pytorch/pytorch/issues/99625
conda install 'llvm-openmp<16'
# for evaluation
pip install evo
pip install open3d
```

3. Compile the cuda kernels for RoPE (as in CroCo v2).
```bash
cd src/croco/models/curope/
python setup.py build_ext --inplace
cd ../../../../
```

### Download Checkpoints

CUT3R provide checkpoints trained on 4-64 views: [`cut3r_512_dpt_4_64.pth`](https://drive.google.com/file/d/1Asz-ZB3FfpzZYwunhQvNPZEUA8XUNAYD/view?usp=drive_link).

Our optimized checkpoints: [`pas3r_512_dpt_4_64.pth`](https://drive.google.com/file/d/1RuMT47ijEykyp9p3Cigu62vx7-gVY7so/view?usp=drive_link)

To download the weights, run the following commands:
```bash
cd src
gdown --fuzzy https://drive.google.com/file/d/1Asz-ZB3FfpzZYwunhQvNPZEUA8XUNAYD/view?usp=drive_link
gdown --fuzzy https://drive.google.com/file/d/1RuMT47ijEykyp9p3Cigu62vx7-gVY7so/view?usp=drive_link
cd ..
```

### Inference Demo

To run the inference demo, you can use the following command:
```bash
# input can be a folder or a video
# the following script will run inference with PAS3R and visualize the output with viser on port 8080
CUDA_VISIBLE_DEVICES=6 python demo.py --model_path MODEL_PATH --size 512 \
    --seq_path SEQ_PATH --output_dir OUT_DIR --port 8080 \
    --model_update_type pas3r --frame_interval 2 \
    --downsample_factor 1000 --vis_threshold 5.0

# Example:
CUDA_VISIBLE_DEVICES=6 python demo.py --model_path src/cut3r_512_dpt_4_64.pth --size 512 \
    --seq_path examples/outdoor.mp4 --output_dir tmp/taylor --port 8080 \
    --model_update_type pas3r --frame_interval 2 \
    --downsample_factor 100 --vis_threshold 6.0

CUDA_VISIBLE_DEVICES=6 python demo.py --model_path src/cut3r_512_dpt_4_64.pth --size 512 \
    --seq_path examples/taylor.mp4 --output_dir tmp/taylor --port 8080 \
    --model_update_type pas3r --frame_interval 2 \
    --downsample_factor 100 --vis_threshold 10.0 
```
Output results will be saved to `output_dir`.


### Evaluation
Please refer to the [eval.md](eval/eval.md) for more details.

## Acknowledgements
Our code is based on the following awesome repositories:

- [CUT3R](https://github.com/CUT3R/CUT3R)
- [DUSt3R](https://github.com/naver/dust3r)
- [MonST3R](https://github.com/Junyi42/monst3r.git)
- [TTT3R](https://github.com/Inception3D/TTT3R)
- [Infinite VGGT](https://github.com/AutoLab-SAI-SJTU/InfiniteVGGT)
- [Viser](https://github.com/nerfstudio-project/viser)

We thank the authors for releasing their code!

## Citation

If you find our work useful, please cite:

```bibtex
@misc{xu2026pas3r,
      title={PAS3R: Pose-Adaptive Streaming 3D Reconstruction for Long Video Sequences}, 
      author={Lanbo Xu and Liang Guo and Caigui Jiang and Cheng Wang},
      year={2026},
      eprint={2603.21436},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2603.21436}, 
}
```
