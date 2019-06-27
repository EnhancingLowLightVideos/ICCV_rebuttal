# Rebuttal Material:Enhancing Low Light Videos by Exploring High Sensitivity Camera Noise

[Flickering artifacts in the video(#R1)](#F1)

[Results with large motion are required(#R1)](#R1)

[Compare with Learning to See in the Dark (#R1)](#C1)

[Compare with the model including only read noise, photon noise and dark noise (#R2)](#C2)

[Compare with the paper doing noise-to-noise (#R2)](#C22)

[Include more results on different type of camera models (#R3)](#I3)

## <span id="F1">Flickering artifacts in the video</span>

We thank the reviewer for pointing out this. Temporal flickering is a common problem in various video processing tasks due to the temporal instability of frame-based optimization or highly non-linearity of deep networks. Not only our methods, but many other video denoising algorithms, like VBM4D and TOFlow suffer from flicking artifacts. To solve this problem, we apply the post-processing [Learning Blind Video Temporal Consistency](<https://github.com/phoenix104104/fast_blind_video_consistency>) on both our algorithm and other methods (VBM4D and TOFlow).  

|                     VBM4D                     |                     TOFlow                     |                     Our                     |
| :-------------------------------------------: | :--------------------------------------------: | :-----------------------------------------: |
|    ![](pic/flickering/orgin/sys/VBM4D.gif)    |    ![](pic/flickering/orgin/sys/TOFlow.gif)    |    ![](pic/flickering/orgin/sys/our.gif)    |
| ![](pic/flickering/ECCV_c/sys/VBM4D_eccv.gif) | ![](pic/flickering/ECCV_c/sys/TOFlow_eccv.gif) | ![](pic/flickering/ECCV_c/sys/our_eccv.gif) |

| ![VBM4D](pic/flickering/orgin/cannon/VBM4D.gif)  | ![](pic/flickering/orgin/cannon/TOFlow.gif)       | ![](pic/flickering/orgin/cannon/our.gif)       |
| ------------------------------------------------ | ------------------------------------------------- | ---------------------------------------------- |
| ![](pic/flickering/ECCV_c/cannon/VBM4D_eccv.gif) | ![](pic/flickering/ECCV_c/cannon/TOFlow_eccv.gif) | ![](pic/flickering/ECCV_c/cannon/our_eccv.gif) |

| ![](pic/flickering/orgin/instr/VBM4D.gif)       | ![](pic/flickering/orgin/instr/TOFlow.gif)       | ![](pic/flickering/orgin/instr/our.gif)       |
| ----------------------------------------------- | ------------------------------------------------ | --------------------------------------------- |
| ![](pic/flickering/ECCV_c/instr/VBM4D_eccv.gif) | ![](pic/flickering/ECCV_c/instr/TOFlow_eccv.gif) | ![](pic/flickering/ECCV_c/instr/our_eccv.gif) |

> Fig.1 The odd-numbered rows are the results of the original algorithms(VBM4D, TOFlow, Our model). The even-numbered rows are the results of the post-processing based on the odd-numbered rows.

From Fig. 1, slight temporal flickering in  our enhancement videos can be remove via  [Learning Blind Video Temporal Consistency](<https://github.com/phoenix104104/fast_blind_video_consistency>) as post-processing. But severe temporal flickering in VBM4D and TOFlow is not obvious to be removed.

In the future work, we will explore to introduce temporal consistency loss in our network and develop an end-to-end network which could simultaneously enhance the low-light videos and reduce the flickering artifacts.

## <span id="R1">Results with large motion are required</span>

Thanks for the suggestion. Our network could deal with the large motion videos well. In fact, we did not deliberately limit the size of the movement on either the training dataset or the test one. Our datasets include about 900 sequences (800 sequences for training and 100 sequences for test). Both the training and test datasets are abundant in moving scenes including small, middle and big motions. The datasets are released at: [Datasets](<https://drive.google.com/open?id=1eC18IbfVGAebJBMDJy2sTdTPjdwdZ4jd>). In Fig. 2, we show more results of our method with big motions.

|                Input                 |            Our results             |
| :----------------------------------: | :--------------------------------: |
|  ![](pic/motion/real/noise/13.gif)   |  ![](pic/motion/real/our/13.gif)   |
|  ![](pic/motion/sys/noise/ball.gif)  |  ![](pic/motion/sys/our/ball.gif)  |
|  ![](pic/motion/sys/noise/cat.gif)   |  ![](pic/motion/sys/our/cat.gif)   |
|  ![](pic/motion/real/noise/19.gif)   |  ![](pic/motion/real/our/19.gif)   |
| ![](pic/motion/sys/noise/motor1.gif) | ![](pic/motion/sys/our/motor1.gif) |
|  ![](pic/motion/real/noise/24.gif)   |  ![](pic/motion/real/our/24.gif)   |

> Fig. 2 More results of our proposed model which including abundant motion. To facilitate visualization of the original input videos, we show in the first column the brightness-scaled input videos.

## <span id="C1">Compare with Learning to See in the Dark</span>

Thank \#R1 for your suggestion.  In Learning to See in the Dark, due to the requirement of long exposure for capturing clean images, it is difficult to extend this method to dynamic video denoising. Besides, two specific camera models (Sony $\alpha$7S 2 and Fujifilm X-T2) are provided with only the raw image as input, while those cameras are unable to obtain raw data when taking videos. 

However, we are still interested in comparing with this method at the same camera settings in the same low light scenario. In this rebuttal,  we calibrate the noise model parameters and train the network for Sony $\alpha$7S 2. Then we make comparisons with the test-sony model  [See-in-dark](<https://github.com/cchen156/Learning-to-See-in-the-Dark>).  A thorough comparison as show in Fig. 3. Through comparing with the image captured during the day, our method is demonstrated to recover higher fidelity both in color and spatial details.

![](pic/see-in-dark.bmp)

> Fig. 3 Comparisons with see-in-dark at the same camera settings(ISO:128000,Exposure time:1/30) in the same low light scenario.

## <span id="C2">Compare with the model including only read noise, photon noise and dark noise</span>

![](pic/modelvalue_im.bmp)

> Fig. 4 Comparisons on the model including only read noise, photon noise, and dark noise (RPD).

Thank \#R2 for your suggestion. To verify the effectiveness of the proposed noise model, we train the network of [TOFlow](https://arxiv.org/abs/1711.09078) and our network model upon basic practical noise model (Eq.~1) and the proposed noise model. As show in Fig. 4, with the same network (i.e. TOFlow or our network), the results of proposed noise model are of the best quality in terms of much less chrominance artifacts, more structural details and higher contrast.

## <span id="C22">Compare with the paper doing noise-to-noise</span>

Thank the reviewer for this suggestion.  The noise-to-noise paper proposed to learn to turn bad images into good images by only looking at bad images, which is quite innovative and inspiring.  From the [official code](<https://github.com/NVlabs/noise2noise>) , the pre-trained networks for Poisson and Gaussian noise are used to process our low light videos(show in Fig. 5). Since the noise levels in low light videos are too high for pre-trained networks(the mean of Poisson is 50 and the standard deviation of Gaussian is 0 to 50) and only use single frame information, the results are not very good.

![noise2noise](pic/noise2noise.bmp)

> Fig. 5 Comparisons with the noise-to-noise with the pre-trained network.

Then we train the noise-to-noise network with different noise models: Poisson, Gaussian and our noise model following the official guide in [Github](<https://github.com/NVlabs/noise2noise>). The noise level are chosen in comparable for fair comparison and the results are shown in Fig. 6. As compared, both method could remove a large extent of noise and recover the spatial details, while the proposed method could further remove the streak noise which could not be handled with Noise2noise. 

![](pic/noise2noise2.bmp)

> Fig. 6 Comparisons with the noise-to-noise that network trained with comparable noise level and network trained with our noise model.

## <span id="I3">Include more results on different type of camera models</span>

Here, the camera model of the Sony $\alpha$7S 2 camera is added, and the results are shown in Fig. 3.  We will include these results in the final paper, and build other types of camera models in the future work.

