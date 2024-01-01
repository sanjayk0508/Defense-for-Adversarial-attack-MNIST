 # **Defense for Adversarial attack-MNIST Dataset**

## **Introduction**

This repository explores defenses against adversarial attacks in machine learning, implemented using the Adversarial Robustness Toolbox (ART). 

### **Defense Types**

**ART supports five defense types:**

1. **Detector:** The detector defense offers a means to identify adversarial samples effectively. Within ART, various detection techniques are available; one such method, Activation Defense, utilizes a victim model's activations to categorize samples into distinct clean and adversarial groups. Notably, ART, up until version 1.10.1, exclusively provided detectors tailored for evasion and poisoning attack scenarios.
    - **Activation Defense:** Clusters samples based on neural activations (e.g., `ActivationDefense`).

2. **Transformer:** Transformer defenses manipulate input data by introducing subtle perturbations aimed at exposing adversarial samples. Through intricate analysis, these defense mechanisms gauge the impact of these perturbations on the model's output, thereby identifying and flagging outliers as potentially poisoned samples.
    - **STRIP:** Detects poisoned data using random transformations (e.g., `STRIP`).

3. **Trainer:** The trainer defense strategy involves instructing the model through training on a blend of clean and adversarial samples. This method essentially guides the model to correctly predict labels for both types of inputs—clean and adversarial—imparting a more comprehensive understanding of diverse data scenarios.
    - **Adversarial Training:** Incorporates adversarial samples into training (e.g., `AdversarialTrainer`).

4. **Preprocessor:** The preprocessor defense technique employs unique preprocessing methods on adversarial inputs to mitigate perturbations, aiming to refine these samples to closely resemble clean data. By enhancing the resemblance between adversarial and clean samples, this strategy potentially enhances model performance against various attack types. The versatility of ART's preprocessing defenses allows them to potentially counteract a broad spectrum of adversarial attacks.

5. **Postprocessor:** The postprocessing defense involves applying specific modifications to the model's outputs, creating a veil over their inherent meaning, thus thwarting potential attackers seeking to steal the model. By obscuring the decision-making process within the postprocessed outputs, this defense acts as a deterrent against model theft.

**Code Examples**

**Detector Defense (Activation Defense):**

```python
from art.defences.detector.poison import ActivationDefense

defense = ActivationDefense(classifier=classifier_poisoned, x_train=train_images, y_train=train_labels)
report, is_clean_reported = defense.detect_poison(nb_clusters=2, reduce="PCA", nb_dims=10)
```

**Transformer Defense (STRIP):**

```python
from art.defences.transformer.poisoning import STRIP

strip = STRIP(classifier=classifier_poisoned)
defense = strip.__call__()
defense.mitigate(x_test_poison[:5000])
```

**Trainer Defense (Adversarial Training):**

```python
from art.defences.trainer import AdversarialTrainer

trainer = AdversarialTrainer(base_classifier=base_classifier, attacks=attack_fgm, ratio=0.5)
trainer.fit(x=x_train[:10000], y=y_train[:10000], nb_epochs=10)
```
