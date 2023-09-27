
# VideoDirectorGPT: Consistent Multi-scene Video Generation via LLM-Guided Planning

Official implementatino of **VideoDirectorGPT**, a novel framework for consistent multi-scene video generation that uses the knowledge of LLMs for video content planning and grounded video generation.


[![arXiv](https://img.shields.io/badge/Arxiv-VideoDirectorGPT-orange)](https://arxiv.org/abs/2309.15091) [![Pytorch](https://img.shields.io/badge/ProjectPage-VideoDirectorGPT-green)](https://videodirectorgpt.github.io/)






[Han Lin](https://hl-hanlin.github.io/),
[Jaemin Cho](https://j-min.io),
[Abhay Zala](https://aszala.com/),
[Mohit Bansal](https://www.cs.unc.edu/~mbansal/)


#### Code comming soon!


<br>
<img width="800" src="assets/video_plan.png"/>
<br>

Illustration of our two-stage framework for long, multi-scene video generation from text:
- In the first stage, we employ the LLM as a video planner to craft a video plan, which provides an overarching plot for videos with multiple scenes, guiding the downstream video generation process. The video plan consists of scene-level text descriptions, a list of the entities and background involved in each scene, frame-by-frame entity layouts (bounding boxes), and consistency groupings for entities and backgrounds. 
- In the second stage, we utilize Layout2Vid, a grounded video generation module, to render videos based on the video plan generated in the first stage. This module uses the same image and text embeddings to represent identical entities and backgrounds from video plan, and allows for spatial control over entity layouts through the Guided 2D Attention in the spatial attention block.

<br>



# Generated Examples


## Single-Scene Videos

Our proposed VideoDirectorGPT framework substantially improves layout and movement control

<table class="center">
  <td style="text-align:center;" width="170"></td>
  <td style="text-align:center;" width="170">"pushing stuffed animal from <strong>left to right</strong>"</td>
  <td style="text-align:center;" width="170">"pushing pear from <strong>right to left</strong>"</td>
  <td style="text-align:center;" width="170">"a pizza is to the <strong>left</strong> of an elephant"</td>
  <td style="text-align:center;" width="170">"<strong>four</strong> frisbees"</td>
  <tr>
  <td style="text-align:center;" width="170">ModelScopeT2V</td>
  <td><img src=assets/action_modelscope_1.gif width="170"></td>
  <td><img src=assets/action_modelscope_2.gif width="170"></td>
  <td><img src=assets/vpeval_modelscope_1.gif width="170"></td>
  <td><img src=assets/vpeval_modelscope_2.gif width="170"></td>
  <tr>
  <td style="text-align:center;" width="170">VideoDirectorGPT (Ours)</td>
  <td><img src=assets/action_ours_1.gif width="170"></td>
  <td><img src=assets/action_ours_2.gif width="170"></td>
  <td><img src=assets/vpeval_ours_1.gif width="170"></td>
  <td><img src=assets/vpeval_ours_2.gif width="170"></td>
</table >



## Multi-Scene Videos



### Single Text Prompt ➡ Multi-Scene Video 


<table class="center">
  <td style="text-align:center;" width="170"></td>
  <td style="text-align:center;" width="170">"make caraway cakes"</td>
  <td style="text-align:center;" width="170">"make peach melba"</td>
  <tr>
  <td style="text-align:center;" width="170">ModelScopeT2V</td>
  <td><img src=assets/hirest_modelscope_1.gif width="170"></td>
  <td><img src=assets/hirest_modelscope_2.gif width="170"></td>
  <tr>
  <td style="text-align:center;" width="170">VideoDirectorGPT (Ours)</td>
  <td><img src=assets/hirest_ours_1.gif width="170"></td>
  <td><img src=assets/hirest_ours_2.gif width="170"></td>
</table >


Our model is able to generate a detailed video plan that properly expands the original text prompt to show the process, has accurate object bounding box locations (overlaid), and maintains the consistency of the person across the scenes. ModelScopeT2V only generates the final food (caraway cake/peach melba) and that food is not consistent between scenes.






### List of Scene Descriptions ➡ Multi-Scene Video 

**Scene 1**: <strong><span style="color:red">mouse</span></strong> is holding a book and makes a happy face.
**Scene 2**: <strong><span style="color:red">he</span></strong> looks happy and talks.
**Scene 3**: <strong><span style="color:red">he</span></strong> is pulling petals off the flower.
**Scene 4**: <strong><span style="color:red">he</span></strong> is ripping a petal from the flower.
**Scene 5**: <strong><span style="color:red">he</span></strong> is holding a flower by <strong><span style="color:red">his</span></strong> right paw.
**Scene 6**: one paw pulls the last petal off the flower.
**Scene 7**: <strong><span style="color:red">he</span></strong> is smiling and talking while holding a flower on <strong><span style="color:red">his</span></strong> right paw.


<table class="center">
  <td style="text-align:center;" width="170">ModelScopeT2V</td>
  <td style="text-align:center;" width="170">VideoDirectorGPT (Ours)</td>
  <tr>
  <td><img src=assets/coref_modelscope_1.gif width="170"></td>
  <td><img src=assets/coref_ours_1.gif width="170"></td>
</table >



Our video plan's object layouts (overlaid) can guide the Layout2Vid module to generate the same mouse across scenes consistently, whereas ModelScopeT2V loses track of the mouse right after the first scene.








### User-Provided Input Image ➡ Video

Users can flexibly provide either text-only or image+text descriptions to place custom entities when generating videos with VideoDirectorGPT. For both text and image+text based entity grounding examples, the identities of the provided entities are well preserved across multiple scenes


<table class="center">
  <td style="text-align:center;" width="170">Input Type</td>
  <td style="text-align:center;" width="170">Input Example</td>
  <td style="text-align:center;" width="170">Scene 1: a <span style="color:red"> < S ></span> then gets up from a plush beige bed.</td>
  <td style="text-align:center;" width="170">Scene 2: a <span style="color:red"> < S ></span> goes to the cream-colored kitchen and eats a can of gourmet cat snack.</td>
  <td style="text-align:center;" width="170">Scene 3: a <span style="color:red"> < S ></span> sits next to a large floor-to-ceiling window.</td>
  <tr>
  <td style="text-align:center;" width="150">Text-Only Input</td>
  <td style="text-align:center;" width="170"><span style="color:red">< S > </span>= "white cat"</td>
  <td><img src=assets/white_cat_1.gif width="170"></td>
  <td><img src=assets/white_cat_2.gif width="170"></td>
  <td><img src=assets/white_cat_3.gif width="170"></td>
  <tr>
  <td style="text-align:center;" width="150">Image+Text Input</td>
  <td><img src=assets/cats.png width="170"></td>
  <td><img src=assets/cat_1.gif width="170"></td>
  <td><img src=assets/cat_2.gif width="170"></td>
  <td><img src=assets/cat_3.gif width="170"></td>
  <tr>
  <td style="text-align:center;" width="150">Image+Text Input</td>
  <td><img src=assets/siamese_cats.png width="170"></td>
  <td><img src=assets/siamese_cat_1.gif width="170"></td>
  <td><img src=assets/siamese_cat_2.gif width="170"></td>
  <td><img src=assets/siamese_cat_3.gif width="170"></td>
  <tr>
  <td style="text-align:center;" width="150">Image+Text Input</td>
  <td><img src=assets/teddy_bears.png width="170"></td>
  <td><img src=assets/teddy_bear_1.gif width="170"></td>
  <td><img src=assets/teddy_bear_2.gif width="170"></td>
  <td><img src=assets/teddy_bear_3.gif width="170"></td>
</table >



<!---
<br>
<img width="800" src="assets/pororo1.png"/>

Video generation examples on a Coref-SV prompt. Our video plan's object layouts (overlayed) can guide the Layout2Vid module to generate the same mouse and flower across scenes consistently, whereas ModelScopeT2V loses track of the mouse right after the first scene, generating a human hand and a dog instead of a mouse, and the flower changes color.


<br>
<img width="800" src="assets/hirest1.png"/>

Comparison of generated videos on a HiREST prompt. Our model is able to generate a detailed video plan that properly expands the original text prompt to show the process, has accurate object bounding box locations (overlayed), and maintains the consistency of the person across the scenes. ModelScopeT2V only generates the final caraway cake and that cake is not consistent between scenes.

-->




# Citation

If you find our project useful in your research, please cite the following paper:

```bibtex
@article{Lin2023VideoDirectorGPT,
        author = {Han Lin and Abhay Zala and Jaemin Cho and Mohit Bansal},
        title = {VideoDirectorGPT: Consistent Multi-Scene Video Generation via LLM-Guided Planning},
        year = {2023},
}
```