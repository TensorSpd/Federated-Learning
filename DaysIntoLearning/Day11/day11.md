# Day 11 of #30DaysOfFL - Federated Learning for Text Generation ✨

Building on the experience from Day 10, which centered on federated learning concepts for image classification, Day 11 shifted the focus to a novel domain: [**Federated Learning for Text Generation**](https://www.tensorflow.org/federated/tutorials/federated_learning_for_text_generation). The objective was to understand how federated methods can be applied to refine a pre-trained language model using decentralized, character-level text data. This scenario is quite realistic in natural language processing tasks, where data often originates from multiple devices or sources and must remain private.

---

## Overview

**Key Insight**: Instead of training a language model from scratch, it is more efficient to start with a pre-trained model and then adapt it with federated learning techniques to new, domain-specific data.

- 📜 **Starting with a Pre-Trained Model**:  
  A character-level RNN model previously trained on Charles Dickens’ works was used as the starting point. This approach mimics a common real-world scenario: leveraging pre-trained language models (potentially large and expensive to train) and then fine-tuning them on decentralized data that may differ in style, topic, or vocabulary.

- 🤝 **Federated Data from Shakespeare**:  
  The tutorial tapped into the TFF `shakespeare` dataset, wherein each client corresponds to lines from a specific character in a Shakespearean play. This simulates the complex, heterogeneous data distributions encountered in real federated settings. Each client’s data can differ greatly, reflecting unique vocabulary usage, punctuation, and stylistic patterns.

- 🔄 **Refining the Model Federatedly**:  
  The Federated Averaging (FedAvg) algorithm was employed to incorporate client updates into the global model. Instead of sharing raw text (as would happen in a centralized approach), each client computed updates locally and only shared model gradients or weights, preserving privacy.

- ⚙️ **Integration with Keras**:  
  By carefully bridging TFF and standard Keras operations, it was straightforward to:  
  - Load the model into TFF for federated training.  
  - Push the updated global model weights back into a standard Keras model.  
  - Use Keras’ familiar `.evaluate()` and `.predict()` methods for performance checks and text generation.

---

## Detailed Steps and Concepts

- **Model Loading**:  
  The pre-trained model on Dickens’ texts was retrieved and loaded into Keras. This included a defined vocabulary of ASCII characters and a mapping from characters to IDs. This step showcased the ease of integrating pre-trained weights into a new environment.

- **Data Preparation**:  
  The Shakespeare dataset was preprocessed to match the model’s input requirements:  
  - Text lines were split into character sequences.  
  - ASCII characters were converted to integer IDs.  
  - Sequences were batched and shuffled, similar to standard machine learning pipelines, but now at the client level.

  A notable challenge was handling different lengths of text across clients. The tutorial used simple truncation (drop_remainder) to avoid complicated padding. In a production-level scenario, strategies for padding or dynamic batching would be needed.

- **Federated Averaging Setup**:  
  Using `tff.learning.algorithms.build_weighted_fed_avg`, a federated training loop was configured. This involved:  
  - Initializing global model parameters.  
  - Choosing client and server optimizers.  
  - Iteratively performing local training on each client’s subset of data and aggregating updates on the server.

  While the example only ran for a few rounds on a small subset of clients, the conceptual takeaway was clear: the global model gradually adapts to the patterns in the Shakespearean text.

- **Evaluation and Text Generation**:  
  After federated fine-tuning, updated weights were loaded back into a standard Keras model. This seamless round-trip allowed:  
  - Standard evaluation metrics (like char-level accuracy) to confirm whether the model improved over time.  
  - Text generation using the newly updated model weights, demonstrating how the language style and character distributions shifted toward Shakespearean prose.

---

## Example Snippet

The following snippet illustrates connecting the pre-trained model to TFF’s federated learning process:

```python
def create_tff_model():
    # Input spec taken from a preprocessed example dataset
    input_spec = example_dataset.element_spec
    
    # Clone the existing Keras model for TFF
    keras_model_clone = tf.keras.models.clone_model(keras_model)
    
    return tff.learning.models.from_keras_model(
        keras_model_clone,
        input_spec=input_spec,
        loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
        metrics=[FlattenedCategoricalAccuracy()]
)
```

This snippet shows:  
- How a Keras model is converted into a TFF model suitable for federated training.  
- How to define custom metrics (like `FlattenedCategoricalAccuracy`) to measure character-level prediction performance.  
- How to initialize and manage the global federated learning state.

---

## Key Takeaways

- 🚀 **Leverage Pre-Trained Models**: Jump-starting federated training with a pre-trained model can drastically reduce training time and improve initial performance.

- 🌐 **Heterogeneous and Non-IID Data**: The Shakespeare dataset exemplifies real-world challenges in federated data. Adapting to these conditions is essential for robust model performance.

- 🏗️ **Seamless Integration**: The ability to push updated weights back into a Keras model simplifies validation and inference, making it easier to measure progress and quality improvements.

This demonstration provides a tangible example of how federated learning is flexible, not only for image tasks but also for language models. It sets the stage for future experiments with more complex architectures, larger datasets, or different modalities, as well as experimentation with hyperparameters and optimization strategies in a federated setting.

---

**End of Day 11**: Moving from image classification (Day 10) to text generation (Day 11) broadened the understanding of federated learning’s applications.