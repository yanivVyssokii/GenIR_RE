# Reverse Engineering the GenIR Trransformer

This project investigates how individual neurons in the MLP layers of a generative information retrieval (GenIR) transformer model respond to specific semantic tokens in user queries. Our primary goal is to identify and analyze neurons that are sensitive to categories such as sentiment, temporal expressions, country names, and question-related words. This contributes to a deeper understanding of how GenIR models encode and differentiate important concepts during retrieval.

---

## Environment Setup

To set up the project environment:

1. **Download the model**  
   Download the `DSI-large-7423` model and store it in a folder named exactly `DSI-large-7423` at the root of this repository.

2. **Create the Conda environment**  
   Run the following command in the project root:

   ```bash
   conda env create -f environment.yml
3. **Activate the environment**
   After the environment is created, activate it with:

   ```bash
   conda activate transformer-env

## 🧪 Experiment Description

This project explores how individual MLP neurons in the decoder of a Generative Information Retrieval (GenIR) model respond to semantically meaningful tokens in user queries. The key idea is to measure how much each neuron's activation changes when a specific token in a query is ablated (i.e., replaced with the UNK token).

### Objective

To identify neurons that are highly sensitive to specific categories of words, such as:

- Negative and positive sentiment words
- Temporal expressions
- Question words
- Country and named entities
- Informative concepts like “AI”, “GDP”, etc.

These neurons are expected to play important roles in how the GenIR model interprets and processes user intent.

---

### Methodology

For each word category:

1. **Query Set Generation**:  
   A set of 3-5 queries is generated for each target word (e.g., `terrible`, `today`, `India`), ensuring the word appears naturally in the sentence.

2. **Token Indexing**:  
   The index of the token containing the word is located using the model’s tokenizer. This allows precise ablation of the desired word.

3. **Forward Pass & Hooking**:  
   The query is passed through the model, and the activations of the MLP layer (typically layer 23) are captured using hooks.

4. **Ablation**:  
   The identified token is replaced with the UNK token, and the model is run again.

5. **Delta Computation**:  
   The neuron-wise difference in activation is computed between the original and ablated runs.

6. **Aggregation**:  
   The results are averaged across all queries for each category. For each neuron, we compute the **mean absolute delta**.

---

### Output Metrics

- **Top-k Neuron Analysis**:  
  For each category, the top 5 neurons with the highest average delta are reported.

- **Activation Histograms**:  
  Histograms show the distribution of activation changes across all neurons in the selected layer. These provide insights into how localized or distributed the impact of the token is.

---

### Insights Enabled

This ablation-based experiment design allows us to:

- Attribute meaning to individual neurons
- Identify shared or specialized neurons across categories
- Understand the relative importance of different types of words in the GenIR model’s decision-making

