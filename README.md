# genAiRP
## Landing Page

Generative Image Steganography is a novel system designed to merge the capabilities of generative AI with cryptographic steganography for secure information exchange. Traditional steganography hides data inside existing media, but this approach introduces new vulnerabilities such as detection through compression or forensic analysis. To address this, our method leverages image generation itself as the medium for embedding, ensuring the hidden information remains indistinguishable from naturally generated samples.

The use case centers around sending sensitive information such as passwords or cryptographic keys securely within generated images. By blending steganography with image synthesis, the system offers a fresh pathway for secure communication where the cover image is created dynamically, making it resistant to conventional steganalysis techniques.

## Model and Dataset

At its core, the system fine-tunes Stable Diffusion XL using feature-map–based generation to embed information within images while retaining visual realism. The pipeline is supported by:

- ImageNet-based Classifier: Trained on 21K+ cleaned and augmented samples (animals, locations) to identify the generated images and assist in extraction of the hidden keys. Achieved 85% classification accuracy.

-Steganography + Cryptography Algorithm: Designed to embed encrypted data directly into the generated image’s feature map, enabling secure retrieval of information.

## Algorithm
Sender Side (Embedding Process):

1. Message Division – Split the input secret message into fixed-size blocks.

2. Image Selection – Use the first 4 bits of each block to determine which image is generated via Stable Diffusion’s feature map.
- First Unencrypted Image
![FirstUnencrypted](/content/ChickenBuildings1.png)

3. Round Key Generation –

- Combine Current Feature Map (4 bits), Previous Feature Map (4 bits), and 8 selected bits from Previous Round Key.

- Apply XOR to generate a new round key (Kn).

4. Encryption – Encrypt the message block using the round key.

Encoded (String format): gAAAAABnN1vDWG0PcL45LmEMNnfXwJfH339pMpDqp8jzehfmzuJJfiOU55yid9pHRrCkag_OKOuYplm8VLY34iRI9PtFfTkuXU2iFTEAxGLP9Hd6ZgB4H7M=
Decoded: This is a secret message

5. Steganography Embedding – Embed the encrypted block into the generated image.
First Image encrypted with Key
![FirstEncrypted](/content/my_new_image.png)

6. Transmission – Send the stego-image as the carrier.

Receiver Side (Extraction Process):

1. Image Classification – Perform classification on the received stego-image to retrieve 4 bits from the feature map.
![ImageClassification](/content/ImageClass.png)

2. Round Key Reconstruction – Rebuild the round key using the same feature map-based F-box function as sender.

3. Data Extraction – Extract the encrypted data from the stego-image.

4. Decryption – Decrypt the block using the reconstructed round key.

5. Message Reconstruction – Append all decrypted blocks sequentially to recover the original secret message.

![F-Box](/content/F-Box.jpg)
![GenSteg](/content/GenSteg.jpg)


## Results and Evaluation

The system successfully demonstrated the feasibility of combining image generation with steganography:

- Achieved 85% classification accuracy using the ImageNet-trained model to reliably identify generated samples for retrieval.

- The steganographic pipeline achieved 80% retrieval accuracy in secure key extraction tests.

## Future Outlook

Future improvements will focus on enhancing the feature-map embedding technique to increase both retrieval accuracy and data capacity. By adding more structural detail to generated images, the system can securely transmit larger payloads without compromising realism. Longer-term directions include refining the generative model to resist adversarial detection and extending the method to multimodal steganography, enabling secure embedding across images, video, and potentially text-to-image workflows.

## Some Extra Images
Second Unencrpyted Image
![SecondUnencrypted](/content/ChickenSea19.png)

Second Image Encrypted with Message using previously communicated Key
![SecondEncrypted](/content/my_new_image2.png)


Implemented with PyTorch, OpenCV, Python, and cryptography libraries for end-to-end encryption, embedding, and extraction.
