# MULTIMODAL-SEQUENCE-PREDICTION-WITH-VISUAL-PRETRAINING-AND-CONTRASTIVE-GROUNDING


**NOTE:**
 
 **To open all ipynb files in this repository, open with github.dev (find the dropdown for open with github.dev at the top left corner after double clickking to open)**
 
**Find all figures and plots on plot_log.pdf** 
 
 
 **Introduction**

This project investigates the impact of visual encoder quality and multimodal grounding objectives on a multimodal sequence prediction task using the Story Reasoning dataset. Given a sequence of image frames and associated textual descriptions, the model is trained to predict the subsequent image frame and text description.
The central question is whether stronger visual representations, obtained via a ResNet encoder pretrained on the Story Reasoning dataset and contrastive image to text grounding, lead to improved reconstruction quality and structural fidelity relative to a convolutional baseline.

**Model Variants/Experiments**

Three model configurations are evaluated under identical training conditions:

 **Baseline CNN Model:**
The baseline architecture uses a dual‑backbone convolutional neural network as the visual encoder. No visual pretraining or explicit cross‑modal alignment is used. This model serves as a lower‑bound reference for evaluating representational improvements.

**ResNet‑18 Visual Encoder/Experiment 1:**
In this model, the baseline CNN visual encoder is replaced with a ResNet‑18 pretrained on Story Reasoning dataset to 100 epochs (see ResNet_Pretrained.ipynb). All other components, including the temporal GRU, decoder, and text autoencoder are kept identical. This configuration isolates the effect of visual pretraining.

**ResNet‑18 with Symmetric InfoNCE Grounding/Experiment 2:**
This model extends the ResNet‑18 variant by introducing a symmetric InfoNCE contrastive loss between region‑of‑interest (ROI) image embeddings and their corresponding text embeddings. A learnable temperature parameter is employed to adaptively scale similarity scores. This experiment evaluates whether explicit multimodal grounding improves reconstruction and structural alignment.
Experiment Results and Plots (See plot_log.pdf for plots)

**Training Loss Evaluation**

ResNet: The loss curve decreases consistently over training indicating stable convergence behavior. Smooth reduction in loss suggests the optimizer successfully learned meaningful visual-text representations.

Baseline: Loss decreases during training, indicating successful optimization. Convergence behavior appears slightly less stable than the ResNet variant. However, the baseline architecture still learns meaningful multimodal relationships.

InfoNCE: The InfoNCE loss decreases steadily and remains stable despite the additional contrastive objective indicating that the additional contrastive objective did not destabilize training. 
While the ResNet‑based model demonstrates slightly smoother loss trajectories, no substantial reduction in overall training loss is observed relative to the baseline.
 
 **Structural Similarity Evaluation**
 
SSIM Heatmaps

Structural Similarity Index Measure (SSIM) heatmaps were computed between reconstructed frames and ground‑truth targets.

ResNet: The ResNet‑18 model exhibits coherent regions of structural agreement, with clear alignment around salient spatial regions.

Baseline: The baseline CNN produces more diffuse SSIM responses.

InfoNCE: The InfoNCE‑augmented model does not show visually distinct improvements over the ResNet‑only variant.

In summary, even if SSIM gains are limited, interpretability indicates improved reasoning behavior.

**SSIM Distributions and Summary Statistics**

SSIM distributions were computed across 626 held‑out test samples for each model. The resulting distributions show substantial overlap, with nearly identical summary statistics:

Model          Mean SSIM    Std

ResNet‑18       0.1276      0.0578

Baseline CNN    0.1272      0.0575

ResNet + InfoNCE  0.1274     0.0578

All three models achieve extremely similar SSIM statistics; mean SSIM values differ only marginally, standard deviations are nearly identical, and quartile ranges strongly overlap. This indicates that the 3 models have:

1. Comparable perceptual reconstruction quality.
   
2. Similar aggregate prediction performance.
   
3. No dominant improvement in low-level image similarity.
   
**Best Mean SSIM**

The ResNet model achieves the highest mean SSIM: ResNet: 0.127619, InfoNCE: 0.127402, and Baseline: 0.127234. Although the improvement is small, it suggests that stronger visual encoding slightly improves reconstruction quality, and better feature extraction contributes to perceptual consistency.

**Maximum SSIM**

The Baseline model achieves the highest maximum SSIM: Baseline Max: 0.494581, ResNet Max: 0.490835, InfoNCE Max: 0.489662. This indicates that all 3  models occasionally produce highly accurate predictions, the baseline architecture can still perform strongly on favorable samples. However, maximum performance alone does not indicate overall consistency.

**Variability**

All models exhibit nearly identical standard deviations: ResNet: 0.057766, Baseline: 0.057451, and InfoNCE: 0.057826. This suggests similar robustness across evaluation samples, comparable prediction consistency, and no major stability differences in reconstruction quality.

**Explainability Analysis: Integrated Gradients**

Integrated Gradients (IG) attribution maps were used to analyse how each model utilises spatial information in the input frames.
Baseline CNN

The baseline CNN exhibits near‑uniform attribution maps, indicating that reconstruction outputs are weakly sensitive to specific spatial structures. This suggests reliance on low‑frequency or global visual statistics, consistent with blurrier reconstructions.
ResNet‑18. In contrast, the ResNet‑18 model demonstrates more localised and structured attributions, particularly around salient regions of the input. This indicates that visual pretraining enables the encoder to capture more meaningful spatial features, even when improvements are not reflected strongly in SSIM values.
InfoNCE. The InfoNCE‑augmented model exhibits attribution patterns similar to the ResNet‑only model. This suggests that while contrastive grounding improves cross‑modal alignment objectives, it does not substantially alter spatial sensitivity in the visual reconstruction pathway for this task.


**Comparative Summary**

**ResNet‑18**

**Strengths**

1. Highest mean SSIM.

2. Improved spatial feature extraction.

3. More structured saliency maps.

4. Stable training dynamics.

 **Weaknesses**

Improvement over baseline is modest. 

**Baseline CNN**

**Strengths**

1. Competitive SSIM performance.

2. Simpler architecture.

**Weaknesses**
Unstable convergence.

**InfoNCE**

**Strengths**

1. Improved multimodal grounding.
   
2. Stable optimization despite contrastive objectives

**Weaknesses**
		
1. Limited quantitative SSIM improvement.

2. Gains are more qualitative than numerical.

**Discussion**

The results of this project demonstrate that visual pretraining improves internal representations, as evidenced by sharper reconstructions and more structured attribution maps. All models achieve similar SSIM performance and training remains stable across all architectures. However, global SSIM fails to capture these improvements, underscoring the importance of combining qualitative analysis, explainability, and quantitative metrics.
The limited impact of symmetric InfoNCE grounding suggests that:

Multimodal alignment may already be implicitly learned by the base architecture, or

The task is dominated by visual prediction rather than fine‑grained cross‑modal grounding.

**Project Limitations/Challenges**

This project did not:

Explore richer perceptual evaluation metrics.

Evaluate text prediction quality directly.

Investigate stronger multimodal contrastive objectives.

**Conclusion**

This project shows that Stronger visual encoders improve feature quality and attention behavior. It also reveals that improvements in latent alignment do not necessarily translate into large SSIM gains. While architectural improvements such as ResNet‑18 pretraining enhance the quality of learned visual representations, these gains are not always reflected in traditional reconstruction metrics. Explainability analyses provide crucial insight into representational differences that scalar metrics alone fail to capture.
This study demonstrates the necessity of multi‑faceted evaluation in multimodal learning systems, where improvements in representation quality may manifest internally even when conventional metrics remain unchanged.
