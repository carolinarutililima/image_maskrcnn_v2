# Supplementary Information

Supplementary materials for this manuscript (datasets, code, and supporting files) are available at:

https://github.com/carolinarutililima/image_maskrcnn_v2

---

## Response to Reviewer #1 (Full Text)

### 1. Novelty and Contribution

**a) Mask R-CNN has been widely used in medical image segmentation and classification; the novelty is unclear.**  
**Response:** We thank the reviewer for this observation. We agree that Mask R-CNN has been widely applied in medical image analysis; however, our contribution does not rely on the use of the standard model. As clarified in the revised manuscript, the novelty lies in (i) the integration of two customized backbones—ResNeXt-143 and a tuned Wide ResNet depth 16, width 12, dropout 0.4)—within an ensemble Mask R-CNN framework (Section The Novel Implementation); (ii) the introduction and evaluation of targeted enhancements such as Soft-NMS and Huber loss for densely clustered cytology structures (Sections 3.4.3–3.4.4); (iii) the harmonized multi-dataset training protocol across three heterogeneous cytology datasets (Section Preprocessing and Dataset Integration); and (iv) the development of a practical IoMT-based diagnostic tool that generates color-coded overlays and automatic coordinate-based reports for malignant cells (Section Tool for Diagnostic – Mask Cervical Tool). These elements together define the novelty of the proposed end-to-end system beyond the conventional application of Mask R-CNN.

**b) “New Features” are not defined mathematically or architecturally.**  
**Response:** We appreciate the reviewer’s comment. In the revised manuscript, the “new features” are now explicitly defined both mathematically and architecturally. Section Loss Functions and Enhancements provides the formal definition of the Huber loss used in the bounding-box regression branch, including its piecewise formulation and comparison with Smooth L1. Section Soft-NMS mathematically describes the exponential Soft-NMS score-decay function and explains its architectural integration into the inference pipeline. Furthermore, the ensemble mechanism is detailed in Section Training Methodology, where we describe the soft-voting strategy and provide the averaging equation used to combine predictions from the ResNeXt-143 and WideResNet backbones. These sections collectively define all newly introduced components in precise mathematical and architectural terms, addressing the reviewer’s concern.

**c) Ensemble methodology lacks theoretical grounding.**  
**Response:** We thank the reviewer for this comment. In the revised manuscript, the ensemble methodology is explained in detail within the Training Methodology section, where we describe the soft-voting fusion strategy applied after Soft-NMS and provide the explicit averaging equation used to combine the confidence scores of the ResNeXt-143 and WideResNet backbones. The rationale for this approach is further discussed in Section The Novel Implementation, where independent backbone evaluations and comparative results demonstrate that each backbone captures complementary cytological features, motivating their combination. Additionally, the effect of Soft-NMS on the ensemble’s behavior is isolated in the Soft-NMS experiment table, which provides empirical grounding for the chosen inference strategy. Together, these sections provide both the methodological description and the empirical justification supporting the ensemble design.

**d) No clear methodological innovation beyond integration.**  
**Response:** We thank the reviewer for this comment. The manuscript indeed introduces methodological contributions that go beyond simple integration, and these are supported by quantitative evidence in the Results section. The innovations can be found in multiple parts of the manuscript:

- **Architectural exploration and optimization:** The proposed Wide ResNet backbone is not used as a plug-in component; rather, we conduct a structured architectural study that evaluates how depth and width influence segmentation performance in cervical cytology images. This analysis is presented in Table 7 (Wide ResNet depth/width experiments), demonstrating performance gains resulting from specific architectural adjustments.
- **Regularization and generalization improvements:** The manuscript evaluates dropout as a controlled architectural parameter, showing how different dropout rates affect segmentation quality in Table 8 (dropout experiments). This systematic investigation demonstrates that methodological decisions—not only integration—drive performance improvements.
- **Inference-level innovation (Soft-NMS adaptation):** The adoption of Soft-NMS is justified through a full threshold sensitivity analysis shown in Table 9 (Soft-NMS experiments), where we demonstrate that Soft-NMS (τ = 0.7) substantially improves recall in dense, overlapping cytology regions. This is a methodological refinement tailored to the cytology domain.
- **Loss-function enhancement:** In the subsection Loss Functions and Enhancements, we provide mathematical definitions of the added Huber loss and explain why it is beneficial for cell-level bounding-box regression, distinguishing our approach from standard Mask R-CNN implementations.
- **Defined ensemble strategy:** The proposed ensemble mechanism is mathematically specified (soft voting + mask confidence selection) and empirically validated in Table 10 (final backbone comparison). This fusion strategy consistently outperforms both individual models—demonstrating that it is not a trivial integration step, but a purposeful methodological contribution.

Together, these results demonstrate that the manuscript provides architectural refinement, loss-function enhancement, inference-level improvement, and ensemble-based methodological innovation, all validated through systematic experiments. These components extend beyond simple integration and contribute meaningfully to the performance and robustness of cytology cell segmentation.

### 2. Data Samples

**a) The manuscript does not adequately address inter-dataset variability.**  
**Response:** We thank the reviewer for raising this important point. The manuscript already addresses inter-dataset variability within the Preprocessing and Augmentation Procedures subsection of the Training Methodology section. Specifically:

- We explicitly describe how all three datasets—despite differences in resolution, acquisition, and staining—were standardized using Roboflow’s unified preprocessing pipeline, which includes color normalization, exposure correction, and contrast adjustment (Section 3.2, Preprocessing and Augmentation Procedures).
- Image dimensions were homogenized by resizing all samples to 640×640, and Dataset 1 was cropped beforehand to account for its higher-resolution sensor output (Section 3.2).
- Domain variability was mitigated through consistent augmentation applied uniformly across all datasets, including rotation, flips, brightness/contrast jitter, Gaussian noise, and random cropping. These augmentations were specifically chosen to reduce the effects of staining differences and illumination inconsistencies (Section 3.2).
- Although no explicit stain-normalization algorithm (e.g., Macenko, Vahadane) was imposed, the unified pipeline and augmentations serve as a practical domain-generalization strategy, which is reflected in the cross-dataset results reported in Table 13 (dataset comparison), where the model achieves stable performance across datasets with different acquisition characteristics.

Thus, the manuscript already incorporates a domain-shift mitigation strategy based on unified preprocessing, controlled augmentations, and cross-dataset evaluation, and these components are clearly documented in the text.

**b) Potential risk of data leakage if cropping and merging were performed prior to train–test splitting.**  
**Response:** We thank the reviewer for highlighting this concern. We clarify that no data leakage occurred, as the manuscript already specifies that all partitioning was performed after preprocessing and strictly at the image level, not the crop level. This ensures that no portion of any image used for training was ever included in validation or testing. As described in Section 3.4 (Training Methodology), the dataset was divided into 70% training, 20% validation, and 10% testing, after the preprocessing pipeline was applied. Importantly, the cropping operation for Dataset 1 was performed only to reduce extremely large resolutions, and each resulting crop was treated as a uniquely indexed sample. The split was then applied to this full set of unique samples, preventing any overlap between splits. Furthermore, the Results section explicitly notes that all evaluation metrics, confidence intervals, and statistical tests were computed solely on the held-out test set, reinforcing that the testing images were never used during model training or tuning. These clarifications confirm that the full protocol prevents training–testing overlap.

**c) The process of label harmonization across datasets is unclear.**  
**Response:** We thank the reviewer for this observation. The manuscript clarifies that all images from Dataset 1, Dataset 2, and Dataset 3 were manually labeled or re-labeled by the same medical specialist, ensuring consistent diagnostic criteria and uniform class definitions across datasets. Specifically, Dataset 1 and Dataset 3 were fully annotated by the specialist, and Dataset 2 (Skipmed) underwent the same labeling validation step before being incorporated. This harmonized all labels into the unified 5-class taxonomy (Dysk, Koil, Meta, Para, Supe).

**d) The manuscript does not report class distribution statistics.**  
**Response:** We appreciate the reviewer’s observation. The manuscript addresses class imbalance and presents per-class performance through multiple analyses in the Results and Discussion section. The confusion matrix (Figure 16) includes class-wise behavior, and the text describes strategies to mitigate imbalance, including dataset merging, targeted annotation of underrepresented classes, uniform augmentation, and ensemble/Soft-NMS effects. Cross-dataset evaluation in Table 15 shows stable performance across datasets with different class frequencies. While raw per-class counts are not listed, per-class behavior is explicitly analyzed and balancing strategies are documented.

### 3. Literature Review

**a–d)** The literature review lacks structured categorization, recent studies, critical limitations, and a comparative table.  
**Response:** We appreciate the reviewer’s detailed feedback. Due to the strict 10-page limit, the manuscript focuses the Related Works section on CNN-based approaches directly comparable to our Mask R-CNN pipeline; Transformer-based, hybrid, or XAI models were not included. Section 2 discusses recent deep-learning studies in cervical cytology, and we analyze limitations such as single-backbone dependence, cross-dataset generalization, and overlapping cell regions. These shortcomings align with the studies cited by the reviewer (10.1109/ACCESS.2023.3325883, 10.1007/978-3-031-77620-5_11). A comparative table was omitted for space constraints.

### 4. Proposed Methodology

**a) Insufficient architectural details.**  
**Response:** We expanded Section 3.4 (Training Methodology) to include full backbone descriptions, hyperparameters, anchor configurations, training procedures, and a link to the public code repository.

**b) “New features” are unclear.**  
**Response:** We added mathematical definitions for Huber loss and Soft-NMS in Section 3.4 and clarified their integration in the inference pipeline.

**c) Ensemble mechanism not adequately explained.**  
**Response:** We clarified that the ensemble uses soft voting after Soft-NMS, with confidence-score fusion and no stacking or hand-tuned weights.

**d) Preprocessing stage lacks stain normalization and augmentation details.**  
**Response:** We clarified that Roboflow’s preprocessing includes color normalization, exposure/contrast adjustment, resizing to 640×640, and consistent augmentations (rotations, flips, brightness/contrast jitter, Gaussian noise, random cropping).

**e) Data leakage concerns.**  
**Response:** We clarified that a strict 70/20/10 split was applied after preprocessing and that training used only the training subset, with validation/testing fully isolated.

### 5. Results and Discussion

**a) Lack of statistical validation.**  
**Response:** We added 95% confidence intervals via bootstrapping and paired Wilcoxon signed-rank tests on per-image IoU and Dice values.

**b) Missing per-class metrics.**  
**Response:** We added per-class precision, recall, F1, sensitivity, and specificity, derived from the confusion matrix evaluation pipeline.

**c) No comprehensive ablation study.**  
**Response:** We clarified that independent backbone comparisons, Soft-NMS threshold experiments, ensemble vs. single-backbone results, and augmentation robustness together provide component-wise ablation evidence.

**d) Missing comparison to EfficientNet/Vision Transformers.**  
**Response:** We clarified the scope and computational constraints, noting that the study targets Mask R-CNN backbones and multi-dataset robustness, and contextualized the results accordingly.

**e) Missing computational complexity analysis.**  
**Response:** We added training time, inference time, and memory usage, comparing the two backbones and the ensemble overhead.# Supplementary Information

Supplementary materials for this manuscript (datasets, code, and supporting files) are available at the project repository:

https://github.com/carolinarutililima/image_maskrcnn_v2
