# UAV Traffic Event Image Generation

This repository contains the implementation of our novel generative approach for enhancing the photorealism of synthetic UAV traffic event images, as described in our paper *Synthesizing Realistic Traffic Events from UAV Perspectives: A Mask-guided Generative Approach Based on Style-modulated Transformer*.  

## Abstract

Unmanned aerial vehicles (UAVs) have emerged as valuable tools in intelligent transportation systems, offering potential for real-time traffic monitoring and emergency response. However, the limited flight endurance of UAVs restricts their ability to collect large amounts of traffic event data crucial for data-driven models. While simulation platforms provide an alternative data source, a significant visual gap persists between synthetic and real images. Given the unique characteristics of UAV perspectives, where objects typically appear smaller, and the challenges faced by existing generative approaches in maintaining structural and semantic consistency, this paper proposes a novel generative approach. Our method integrates semantic mask guidance with a style-modulated Transformer-based GAN architecture to address these issues. Our approach first utilizes the Segment-Anything Model (SAM) and Contrastive Language-Image Pre-training (CLIP) to extract semantic masks from GTA V simulated images. Subsequently, we introduce Spatially-Attentive Denormalization Modules (SADM) within the generator. These modules incorporate semantic mask statistics to enhance image quality and maintain consistency. Furthermore, we develop a perceptual discriminator incorporating a "memory bank" mechanism to more effectively evaluate image realism and stabilize the training process. To further enhance the quality of generated images, we develop a comprehensive training strategy. Our experimental results demonstrate that the proposed method outperforms existing state-of-the-art approaches in both quantitative metrics (i.e., FID and KID) and qualitative visual assessments, thus highlighting its effectiveness and superiority. Overall, our approach offers a robust solution for generating highly realistic traffic event images from UAV perspectives, effectively addressing the scarcity of real-world UAV-recorded traffic event data.  

![](images/1.png)

## Key Features

- Semantic mask guidance using SAM and CLIP  
- Style-modulated Transformer-based GAN architecture  
- Spatially-Attentive Denormalization Modules (SADM)  
- Perceptual discriminator with "memory bank" mechanism  
- Comprehensive training strategy  

## Dataset

Our study utilizes a comprehensive and diverse dataset to address the scarcity of real traffic event images from UAV perspectives. We use real UAV images (target domain) to transform traffic event images simulated by GTA V (source domain) into a realistic style.  

#### Source Domain (GTA V Simulated Images)

- Total images: 7,638  
  - Traffic events (primarily accidents): 3,348  
  - Normal traffic scenarios: 4,290  
- Simulation process:  
  - Used Rockstar video editing tool for angle setting and video recording  
  - Randomly extracted a single frame per video  
- Diversity factors:  
  1. Road types (e.g., urban main roads, highways, multi-level interchanges)  
  2. Vehicle types (e.g., motor vehicles, non-motor vehicles)  
  3. Weather conditions (e.g., sunny, rainy, snowy, foggy)  
  4. Lighting conditions (e.g., night, day, dusk)  
  5. Shooting angles (vertical and non-vertical perspectives)  

#### Target Domain (Real UAV Images)

Total images: 3,200  

1. Open-source UAV video datasets (2,600 images):  
   - UAVDT: 1,000 frames  
   - AU-AIR: 600 frames  
   - VDD: 400 frames  
   - UAVid: 600 frames  
2. Custom collection: 600 vertically shot UAV images from Nanjing city, China  

#### Dataset Split

- Training set (80%): 6,110 GTA V images and 2,560 real UAV images  
- Test set (20%): 1,528 GTA V images and 640 real UAV images  

This comprehensive dataset, combining synthetic and real images from various sources and conditions, ensures that our model can effectively learn to generate realistic UAV traffic event images across a wide range of scenarios. The diversity in road types, vehicles, weather conditions, lighting, and shooting angles enhances the dataset's representativeness.  


## Results

#### Results on Our Dataset

Our method demonstrates strong performance compared to existing state-of-the-art models. The following table presents a comprehensive comparison:  

| ID   | Category                  | Model               | Code                                                         | FID↓      | KID↓     |
| ---- | ------------------------- | ------------------- | ------------------------------------------------------------ | --------- | -------- |
| 1    | Traditional Methods       | CycleGAN            | [Link](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) | 143.36    | 6.61     |
| 2    | Traditional Methods       | AugGAN              | [Link](https://github.com/yuanmengzhixing/AugGAN-Cross-Domain-Adaptation-with-GAN-based-DataAugmentation) | 118.91    | 5.13     |
| 3    | Traditional Methods       | U-GAT-IT            | [Link](https://github.com/Lornatang/UGATIT-PyTorch)          | 163.20    | 8.56     |
| 4    | High-Resolution Methods   | LPTN                | [Link](https://github.com/csjliang/LPTN)                     | 96.32     | 4.13     |
| 5    | High-Resolution Methods   | EPE                 | [Link](https://github.com/isl-org/PhotorealismEnhancement)   | 94.98     | 4.10     |
| 6    | Diffusion Methods         | UNSB                | [Link](https://github.com/cyclomon/UNSB)                     | 81.56     | 3.96     |
| 7    | Transformer-based Methods | UVCGAN              | [Link](https://github.com/LS4GAN/uvcgan)                     | 100.74    | 4.32     |
| 8    | Transformer-based Methods | UVCGAN V2           | [Link](https://github.com/LS4GAN/uvcgan2)                    | 79.82     | 3.85     |
| 9    | Transformer-based Methods | **Proposed Method** | [Link](https://github.com/TRMetaGroup/UAV-Event-Photorealism) | **73.44** | **3.68** |

Note: Lower values for FID (Fréchet Inception Distance) and KID (Kernel Inception Distance) indicate better performance.  

As shown in the table, our proposed method achieves the best performance across all compared models, with the lowest FID (73.44) and KID (3.68) scores. This demonstrates the effectiveness of our approach in generating high-quality, realistic UAV traffic event images.  

![[Your original results section] ](images/2.png)



#### Results on CelebA Dataset

We conducted additional experiments on the CelebA dataset to demonstrate the broader applicability of our method. The following table shows the FID and KID scores for various image translation tasks. Lower scores indicate better performance.  

##### (1) Male to Female / Female to Male

| Method      | Male to Female   |                  | Female to Male   |                  |
| ----------- | ---------------- | ---------------- | ---------------- | ---------------- |
|             | FID              | KID (×100)       | FID              | KID (×100)       |
| ----------- | ---------------- | ---------------- | ---------------- | ---------------- |
| ACL         | 9.4              | 0.58±0.06        | 19.1             | 1.38±0.09        |
| Council     | 10.4             | 0.74±0.08        | 24.1             | 1.79±0.10        |
| CycleGAN    | 15.2             | 1.29±0.11        | 22.2             | 1.74±0.11        |
| U-GAT-IT    | 24.1             | 2.20±0.12        | 15.5             | 0.94±0.07        |
| VitGAN      | 9.6              | 0.68±0.07        | 13.9             | 0.91±0.08        |
| UVCGAN V2   | 4.7              | 0.14±0.02        | 7.6              | 0.24±0.02        |
| Ours        | **4.5**          | **0.13±0.02**    | **7.3**          | **0.23±0.03**    |

##### (2) Remove Glasses / Add Glasses

| Method      | Remove Glasses   |                  | Add Glasses      |                  |
| ----------- | ---------------- | ---------------- | ---------------- | ---------------- |
|             | FID              | KID (×100)       | FID              | KID (×100)       |
| ----------- | ---------------- | ---------------- | ---------------- | ---------------- |
| ACL         | 16.7             | 0.70±0.06        | 20.1             | 1.35±0.14        |
| Council     | 37.2             | 3.67±0.22        | 19.5             | 1.33±0.13        |
| CycleGAN    | 24.2             | 1.87±0.17        | 19.8             | 1.36±0.12        |
| U-GAT-IT    | 23.3             | 1.69±0.14        | 19.0             | 1.08±0.10        |
| VitGAN      | 14.4             | 0.68±0.10        | 13.6             | 0.60±0.08        |
| UVCGAN V2   | **10.6**         | **0.27±0.06**    | **11.3**         | **0.34±0.07**    |
| Ours        | 11.2             | 0.31±0.08        | 12.0             | 0.36±0.09        |

Note: Bold numbers indicate the best performance for each metric.  

These results demonstrate that our method achieves state-of-the-art performance on gender swap tasks and competitive performance on eyeglasses manipulation tasks, showcasing its effectiveness across different image translation scenarios.

## Usage & Citation

We will upload our code soon. If you find this work useful in your research, please consider citing our paper.