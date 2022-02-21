# XAI experiment

We develop a two parts experiment aiming to measure the performance of a XAI method. To do so, we're going to divide the performance into two different elements: goodness and trust. Each of the parts of the experiments aim to handle one of the two elements. Despite the fact that we divide the experiment into two parts, two elements are shared between both of them: the dataset and the artificial intelligence model.

## Shared elements

### Dataset

For the experiment, we propose to use the [MURA](https://stanfordmlgroup.github.io/competitions/mura/) dataset. This dataset contains x-ray images of multiple parts of seven parts of the upper body. The original goal for the dataset was the detection of bone fractures. Nonetheless, we simplify the problem trying to discover the part of the body found on the image.

> We assembled a dataset of musculoskeletal radiographs consisting of 14,863 studies from 12,173 patients, with a total of 40,561 multi-view radiographic images. Each belongs to one of seven standard upper extremity radiographic study types: elbow, finger, forearm, hand, humerus, shoulder, and wrist

To simplify even further the problem we are going to only select two of the classes: hands and elbows. This selection 


<center>
	<div align="center" style="display: flex; justify-content: space-evenly;">
		<img style="width: 40%;" src="figs/hand.png" />
		<img style="width: 40%;" src="figs/elbow.png" />
	</div>
	<br />
	<em>Examples of hands and elbows from the MURA dataset</em>
</center>

The dataset contains multiples view of each body part. We are going to use all of these views indistinctly. 

##### Specifications

<center>

|  Information		| Values            	|
| :---------------	|:--------------------:	|
| Dataset size		| 11077					|
| Train size	 	| 10188					|
| Test size	 		| 889					|
| Number of classes	| 7						|
| Selected classes	| Hands and elbows		|
| Type of image 	| X-ray					|
| Size of image 	| Variable				|
| Representation	| UINT8					|

</center>

### A.I model

The A.I model is a crucial part in the process of obtaining explanations. Nonetheless, we considered an issue outside the scope of the research, and we are only interested in obtaining a model with an acceptable performance that allow us to apply successfully XAI techniques. To be able to accomplish this we take into consideration the simplicity in the usage of the model, if it is or not implemented in the default libraries, and the performance in the benchmark competition [ImageNet](https://paperswithcode.com/sota/image-classification-on-imagenet?tag_filter=18%2C17%2C5%2C3). We have selected [EfficientNet](https://arxiv.org/pdf/1905.11946.pdf) model, due to the availability in the Tensorflow library and the high score in the ImageNet benchmark.

To increase the performance of this model on our problem, we are going to use *transfer learning* approaches with the weights obtained to train with the ImageNet dataset.  


## Goodness

The first part of the experiment is the assessment of the goodness of the XAI algorithms we are going to use. Goodness is defined by Hoffman *et al.* as

> \[...\] we find assertions about what makes for a good explanation, from the standpoint of statements as explanations. There is a general consensus on this; factors such as clarity and precision. Thus, one can look at a given explanation and make an a priori (or decontextualized) judgment as to whether or not it is "good."

In other words, what we will try to assure with the measure of the goodness is the quality of the saliency map and if the results of these methods is real. 

To measure the goodness, multiple metrics has been proposed. Following the guidelines of T. Gomez *et al.* we are going to use one metric for goodness (**AOPC**, proposed by Samek *et al.*) and two auxiliary ones (**Sparsity**, proposed by T. Gomez *et al.* and **Faithfulness**, proposed by D. Alvarez *et al.*). We are going to study also the option to aggregate them into an easier to study metric. 

The results of the goodness will be used for the selection of the best fitting XAI method. We are going to select one from the ones indicated in the next subsection

### XAI Methods

In this experiment, we are gonna only take into consideration the XAI algorithms that generates saliency maps. As we already explained, the Goodness will be used to select which saliency algorithm to use for the follow-up experimentation. The selection of the algorithms to calculate the goodness is obtained from two sources: Miro *et al.* (SLR) and from Hooker *et al.*

<center>
	
|  Methods		   		|	Authors															|
| :-------------------  | :------------------------------------------------------------ 	|
| GradCAM	   			| [S. Ramprasaath *et al.*](https://arxiv.org/abs/1610.02391)		|
| GradCAM++	   			| [A. Chattopadhyay *et al.*](https://arxiv.org/abs/1710.11063)		|
| LIME					| [M. Ribeiro *et al.*](https://arxiv.org/abs/1602.04938)			|
| SmoothGrad			| [D. Smilkov *et al.*](https://arxiv.org/abs/1706.03825)			|
| NormGrad				| [R. Sylvestre-Alvise *et al.*](https://arxiv.org/abs/1910.08823)	|
| K. Simonyan *et al.*	| [K. Simonyan *et al.*](https://arxiv.org/abs/1312.6034)			| 	
| Guided Backprop 		| [J. Springenberg *et al.*](https://arxiv.org/abs/1412.6806)		|
| Integrated Gradients	| [M. Sundararajan *et al.*](https://arxiv.org/abs/1703.01365)		|

</center>

We are gonna use implemented version of these different XAI methods.

The results of this part of the experiment gonna be a list of acceptable XAI methods for the selected dataset, MURA, and the selected black-box model, EfficientNet. The assertion of this acceptability will be the goodness defined in the previous section. 