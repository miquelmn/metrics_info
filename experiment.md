# XAI experiment

We developed a two parts experiment aiming to measure the performance of XAI methods in the medical image context. To do so, we assesed the performance dividing it into two different elements: goodness and trust. The experiment is divided also into two parts each one of them aimed to measure these two elements separatly. Nonetheless we also had two elements shared between both of the parts: the dataset and the artificial intelligence model.

## Shared elements

### Dataset

For the experiment, we used the [MURA](https://stanfordmlgroup.github.io/competitions/mura/) dataset. This dataset contains x-ray images from seven parts of the upper body. The original goal for the dataset was the detection of bone fractures. Nonetheless, we simplified the problem trying to discover the part of the body found on the image. To simplify even further the problem we converted the classifaction problem into a binary one between hands and elbows.


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

The A.I model is a crucial part in the process of obtaining explanations. Nonetheless, we considered an issue outside the scope of the research, and we were only interested in obtaining a model with an acceptable performance that allowed us to apply successfully XAI techniques. To be able to accomplish this, we took into consideration the access to an already existing implementation and the performance in the benchmark competition [ImageNet](https://paperswithcode.com/sota/image-classification-on-imagenet?tag_filter=18%2C17%2C5%2C3). We selected the CNN [EfficientNet](https://arxiv.org/pdf/1905.11946.pdf), due to the availability of already implemented version of it and the high score in the ImageNet benchmark competition. We increased the performance of this model with the usage of *transfer learning* based on the weights obtained to train with the ImageNet dataset.  


## Goodness

The first part of the experiment is the assessment of the goodness of the XAI algorithms used. Goodness is defined by Hoffman *et al.* as

> \[...\] we find assertions about what makes for a good explanation, from the standpoint of statements as explanations. There is a general consensus on this; factors such as clarity and precision. Thus, one can look at a given explanation and make an a priori (or decontextualized) judgment as to whether or not it is "good."

In other words, the goal of this part of the experiment was to assure, with the measure of the goodness, the quality of the methods to obtain saliency map and if the results of these methods were consequent. 

To measure the goodness, multiple metrics had been proposed. Following the guidelines of T. Gomez *et al.* we used one metric for goodness (**AOPC**, proposed by Samek *et al.*) and two auxiliary ones (**Sparsity**, proposed by T. Gomez *et al.* and **Faithfulness**, proposed by D. Alvarez *et al.*). We studied also the option to aggregate them into an easier to intepret metric. 

The results of the goodness were used to select the best fitting XAI method from the ones indicated in the next subsection.

### XAI Methods

In this experiment, we were only take into consideration the XAI algorithms that generates saliency maps. As we already explained, the Goodness will be used to select which saliency algorithm to use for the follow-up experimentation. The selection of the algorithms to calculate the goodness is obtained from two sources: Miro *et al.* (SLR) and from Hooker *et al.*

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

Similarly that we did with the AI model we used already implemented version of these different XAI methods.

The results of this part of the experiment was a set of acceptable XAI methods for the selected dataset, MURA, and the selected black-box model, EfficientNet. The assertion of this acceptability will be the goodness defined in the previous section. 