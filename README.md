# cv-mini-project

---

## 1. Description (±1 page)
- **Background & Motivation**  
  With a linear probe, you can quickly test new datasets and get competitive performance. Along with DINOv2 model, we can achieve very robust embeddings for images without requiring full fine-tining, saving computational resources. So I've thought It may be useful and interesting to do this task.
- **Hypothesis / Research Question**  
  We hypothesize that the frozen DINOv2 backbone will produce embeddings that are separable enough for a linear classifier to achieve strong accuracy on CIFAR-10.
- **Plan**  
  Summarize your steps:
  1. Load the DINOv2 backbone (ViT-B/14).
  2. Freeze the backbone and train a linear layer on top for CIFAR-10.
  3. Evaluate classification accuracy and visualize embeddings with t-SNE.

---

## 2. Experiment Journal (3–8 pages)

### EXP01: Linear Probe with DINOv2
- **Setup & Implementation**  
  - We've used CIFAR-10 dataset, as was proposed at the very end of the example notebook, and I've used train and split parts of it preconfigured at the download stage. If we'd have required more thorough processing, I'd also advocate for having a validation dataset split from the train part of the downloaded one.
  - We've added a very basic transformations, and resizing to meet both the normalization requirements by the CIFAR dataset, and size requirements by the DINO model
  - Moreover we've used CUDA, since all the work was done in Google Colab with A100 GPU
  - We've loaded DINOv2 via `torch.hub`, froze it, and added a linear classifier.
- **Hyperparameters**  
  - Learning rate is default I'd say, because this task is not as demanding requiring very thorough search withing the hyperparameter space: 0.01 \
  - The optimizer is SGD, with momentum 0.9
  - Number of epochs is 4, since the embeddings from the base model are pretty robust, requiring little extra attention, after 4th epoch I've seen no improvement, but it was already superb (97%)
- **Training Logs & Metrics**  
  - Train loss: ![Screenshot 2025-02-23 at 22.33.03.png](images%2FScreenshot%202025-02-23%20at%2022.33.03.png)
  - Present final test accuracy (e.g., “Test Accuracy: 97.8%”). ![Screenshot 2025-02-23 at 22.39.24.png](images%2FScreenshot%202025-02-23%20at%2022.39.24.png)
- **Analysis of Results**
  - I'd say that the DINOv2 already has very good embeddings and that the CIFAR-10 dataset is pretty novel as a downstream task. So there's no solid reason to do this particular training, but It would be extremely interesting to use it in other more complicated end-to-end scenarios. Moreover, it was interesting for me to play with DINOv2 for linear probe experiment!
  - There are some missclassifications, but that isn't a big deal, compared to other methods of training and other models we've explored in other modules of this course (i.e. ones trained on top of CLIP, Self-Supervised, Semi-Supervised, etc.) but that is a whole other area. ![Screenshot 2025-02-23 at 22.33.11.png](images%2FScreenshot%202025-02-23%20at%2022.33.11.png)
### EXP02: Feature Embedding Visualization
- **Method**  
  - I've used T-SNE to visualize the embeddings in a 2d space, since it's a very common method for embedding visualization.
- **t-SNE Plot**  
  - ![Screenshot 2025-02-23 at 22.32.46.png](images%2FScreenshot%202025-02-23%20at%2022.32.46.png)
  - The classes 3, 5, 6, 0 and 2 overlap a bit, but that might be caused more by the limitations of 2D space rather than the poor classifier performance, and I'm sure that If we've visualized this in 3D we'd clearly see that the classes are well separated. But if we've had a low accuracy, we'd definetely go that way, along with some more measurable metrics, like centroid distance and others.

---

## 3. Summary (±1 page)
- **Key Findings**  
  - We found that even without fine-tuning, DINOv2 features yield a linear classifier accuracy of 97.8% on CIFAR-10, demonstrating strong representation quality.
- **Limitations & Future Work**  
  - We've only tested on a single dataset and used limited hyperparameter tuning.
  - Fine-tuning the entire model could be challenging, and interesting, however, the most interesting future work for me would be a like transfer-learning wheere the downstream task is loosely resembled in the training data (which I think is not the case for CIFAR-10, hence so splendid results here, without much fo a burden for fine-tuning)

