# readme
# Model Scheduling and Sample Selection for Ensemble Adversarial Example Attacks

## Background
Deep neural networks (DNNs) are vulnerable to adversarial examples. Through fighting against an ensemble of pre-trained source models, an adversary may generate adversarial examples transferable to target models. Due to the limitation in computational resources (e.g., GPU memory), such ensemble adversarial example attacks usually cannot handle all source models and input samples. This problem brings a new research issue: How to optimally schedule models and select samples for ensemble attacks to improve adversarial example transferability? To shed light on this issue, we propose a new multi-stage ensemble attack method.
## Methodology
We propose a new multi-stage ensemble attack method, in which two general strategies for model scheduling and sample selection are designed. The first strategy schedules source models to be used in every stage, based on decision-boundary similarity and model diversity. The second strategy selects input samples to be processed by ensemble attacks, according to their sensitivity to adversarial perturbations.

## Usage
### Install
+ [Anaconda](https://www.anaconda.com/distribution/) 
+ Python3.7.4
+ Tensorflow 1.14.0
+ Tensorpack 0.10.1
+ Pillow 6.0.0
+ Scipy 1.2.1
+ Numpy  1.16.5
### Dataset and model checkpoints
We use images from ImageNet LSVRC 2012 Validation Set and resized them to 224x224. You can download the preprocessed images **[HERE](http://www.image-net.org/challenges/LSVRC/2012/)**.
 We adopt 13 models in our experiments, including 8 normally-trained models, i.e., Resnet-v2-{50, 101,152} , Vgg<sub>16</sub> , Vgg<sub>19</sub> ,Inc-v3, Inc-v4 and IncRes-v2 , and 2 adversarially-trained model, i.e., Inc-v3~adv~  and IncRes- v2~adv~, and 3 models trained with ensemble adversarial training, i.e., Inc-v3~ens3~, Inc-v3~ens4~ and IncRes-v2~ens~.We original download them and theirs checkpoints from [here](https://github.com/tensorflow/models/tree/master/research/slim) and [here](https://github.com/tensorflow/models/tree/master/research/adv_imagenet_models).
#### Our proposed method

We assign every network with an id, here is a table to provide ids for each network. 
ID | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | a | b | c |
---|---|---|---|---|---|---|---|---|---|---|---|---|---|
Network Name|Inc-v3|Inc-v4|Res-50|Res-101|Res-152|IncRes-v2|Inc-v3~ens3~|Inc-v3~ens4~|IncRes-v2~ens~|Vgg~16~|Vgg~19~|Inc-v3~adv~|IncRes-v2~adv~|

If you want to attack all $8$ normally-trained models,you can change the source models by modify attack_network by
```bash
python attack.py --attack_network = "0123459a"
```
and the number of models used in each stage is set by config.parallel_num in config.py.


We set the step size $\alpha$  to $0.8$. Furthermore, the penalties $\beta$ and $\gamma$ used in our model scheduling algorithm are set to $5.0$ and $0.1$, you can check config.py for more options.
Using the function attack in attack.py  scheduling models and craft adversarial examples. You can change the number of models used in each stage by modify config.parallel_num in config.py.For example, you can use two models in each stage by
```bash
python attack.py --parallel_num=2
```
For sample selection, you can using the function pick in pick.py, you can change the number of sample to craft by modify pick_num.For example, we select 1000 samples by
```bash
python pick.py --pick_num=1000
```
## Contributing
To our knowledge, we are the first to study model scheduling for multi-stage ensemble attacks towards DNNs. We propose an effective model scheduling strategy by jointly considering two complementary criteria: decision boundary similarity and model diversity.  
The proposed ensemble attack method may drive the research on adversarial example defenses. In addition, it can also be used to evaluate the robustness of DNN models before they are deployed. 
 
