# AI Safety Homework 1 - Group 5

Repository for the first homework of the AI Safety: Concepts and Techniques course at the University of Oldenburg (winter semester 2026/27).

Group members: SHALHAWI Sedra, HOROYAN Koren, BENAMMAR Wissem, Awais

## What is in here

- opendrivevla_hw1_groupe5_final.ipynb: the notebook for Part 1, with all code and saved outputs
- opendrivevla_hw1_groupe5_final.html: the same notebook exported to HTML, so it can be read without Jupyter
- hw1_groupe5/: the raw results of our runs (model predictions, noise experiments, planning metrics, ODD annotations and figures)

The nuScenes data and the model checkpoint are not included because of their size and the nuScenes licence.

## Part 1: OpenDriveVLA

We ran the 0.5B version of OpenDriveVLA on the nuScenes validation set. We did not have a Linux machine with a suitable GPU, and the full nuScenes dataset is too large for Google Colab, so we rented a server on RunPod with an RTX A5000. We used CUDA 11.8 because the repository needs PyTorch 2.1.2 and an mmcv version that has to be compiled, and both work best with that version. We kept memory usage under 12 GB and ran all trials one after the other, as asked in the assignment.

Some choices we made:

- Sample 400 (80 x 5) is counted in the order used by the dataset loader, which sorts samples by timestamp. The 30 extra samples come from random.seed(5) and sorted(random.sample(range(6019), 30)).
- The model keeps an internal state between frames. We reset it before every run so that all runs start from the same conditions. While checking this, we found that our first clean reference had been computed without a reset, so we recomputed it and corrected the results. Both versions are kept in the notebook.
- Noise is added on images scaled to [0, 1] and clipped back to that range. For sample 400 we tested four levels (0.02, 0.05, 0.10, 0.20) with three seeds each. For the 30 samples we used a smaller setup (0.05 and 0.20, two seeds) because each run takes time on the server.
- We call a result a failure when the planned trajectory moves on average more than 0.5 m away from the clean one. This is roughly the lateral margin a car has in its lane. A larger deviation does not prove the car would crash, it only shows that the plan changed noticeably.
- For the ODD we used factors we could actually read from the data: time of day, weather, city, road type, driving goal, speed, traffic density and pedestrians nearby.

## Part 2: STPA of an AI anti-phishing system

The assignment allowed us to replace the voice assistant with another system where losses matter. Most of us study cybersecurity, so we chose something closer to what we work on: an AI-based anti-phishing gateway that checks every incoming email before it reaches the user. Phishing is one of the most common ways companies lose money and accounts, and the AI takes real decisions (warn, flag, block), which makes it a good case for STPA. It also let us look at the link between safety and security, for example when an attacker writes text in an email to fool the classifier.

The full analysis (architecture, losses, hazards, unsafe control actions, requirements and verification plan) is in the report.

## Use of AI tools

Notebook cells marked with [GenAI-assisted: Claude] were written with the help of Claude. The complete statement is in the report.
