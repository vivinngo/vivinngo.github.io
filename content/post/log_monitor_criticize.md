---
title: "Log, Explain, Criticize: A Practical Loop for Trustworthy Models in Production"
date: 2026-06-08T10:00:00+01:00
description: "You train models and achieve good results with high accuracy, precision, RMSE, etc, but it quietly makes bad decisions in production, which raises questions in the team, including you."
tags: ["AI_model", "production"]
categories: ["AI topic"]
---
![Cover image](/img/post4_log_monitor_criticize/data_drift.jpeg)

You train models and achieve good results with high accuracy, precision, RMSE, etc, but it quietly makes bad decisions in production, which raises questions in the team, including you. Isn’t it disappointed? For the things you spend weeks/months on, and now you feel useless. If you have ever suffered that uncomfortable feeling, well, you’re not alone. This is a common challenge when developing ML/AI models.

Unlike projects at university or on Kaggle, in real projects, when you finish training models and evaluating with common metrics such as RMSE, MSE, accuracy, precision, etc, the game just starts. You have to monitor performance, explain the model’s decisions and present to your engineering team and stakeholders.

One of the most challenging problems is “drift”. It can be data drift or performance drift. In simple words, drift is the changes between training and production. Data drift means data inputs in production are different compared to training data. The reasons can be that your training data doesn’t cover the full picture (different months/seasons, etc) or because of external factors (behavior changes, new trends, etc). Performance drift is when the model performs badly while it was really good during training. This is complex and can be from many factors, such as historical bias, data leakage, overfitting, etc.
![Alt text](/img/post4_log_monitor_criticize/distribution.png)
Source: Evidently AI’s blog

As an ML/AI engineer, besides maintaining the model’s performance, another equally crucial task is to detect why data/performance drift occurs and be able to report to the team.

You should be able to answer these questions: “Why did the model make this decision?”, “What feature was most influential here?”, “Can we trust the output for this edge case?” to gain trust from the team and stakeholders.

This is when you need model observability and explainability. They are two different things that are easily confused. People usually lump them into one umbrella with terms like “ML transparency” or “ML trust”. Model explainability answers why the model made this prediction. Meanwhile, model observability answers whether the model behaves as expected in production.

There are 3 things you should do after shipping models.
![Alt text](/img/post4_log_monitor_criticize/three_components.png)
Source: Netflix's blog

## Check 1: Logging and Monitoring — Could you even see the problem?
Logging is the foundational layer of ML observability. In simple words, it records everything needed to reconstruct and reason about a model’s decision, including inputs, features, model version, output score, decision thresholds, and timestamps.

The hardest part is deciding what to log and at what granularity (level of detail). Without good logs, the upstream layers, like monitoring and explaining, have nothing reliable to work with. If you logged just input and output, you’d see accuracy shift with no idea. But if you also logged feature distribution over time, you’d see the shift when a feature silently becomes dominant in production. By choosing the correct things to log, you will be able to navigate your diagnosis from “Something is wrong” to “This feature is wrong”.
![Alt text](/img/post4_log_monitor_criticize/log_schema.jpeg)
Source: Netflix’s blog — Initial data schema for ML observability

In production, logs are stored in multiple destinations, depending on the types of logs. For example, high-volume request/prediction logs can be stored in object storage like S3 or Azure Blob, while structured metrics and feature distributions are stored in a time-series database like Prometheus or a monitoring platform like Datadog.

## Check 2: Explaining the names of the culprits
Monitoring tells you what’s wrong, but it cannot tell you what to fix. That’s when we need model explainability. This component aims to answer why models do what they do, which helps you quickly diagnose the issues. We aim to answer these questions: Which feature was the most influential? Why this prediction but not those predictions?

Local explanation is for a single prediction, for example, “Why did the model make this specific prediction?”. Global explanation is for overall patterns, for example, “Across all the predictions the model makes, which features matter most overall?”. Local explanation matters more to stakeholders because they want to know, in particular, the edge cases and why in that situation. This is because their work is based on model prediction; they need to work with customers to explain what happened if there is any issue.

There are some common techniques to explain models, such as SHAP, LIME, Integrated Gradients (IG), or DeepLIFT. SHAP is widely used in most teams and used as part of model explanation in the Netflix Payment Routing.
![Alt text](/img/post4_log_monitor_criticize/shap.png)
Source: Geeksforgeeks’s post

Uber uses Intergated Gradients (IG) to explain individual predictions from deep learning models on its Michelangelo platform. However, IG is computationally expensive, so the team uses RAY to distribute computations across many workers in parallel, which cuts total runtime by 80%.

## Check 3: Criticize the explanation

After the previous step, we are quite confident about our understanding of models, but wait, can you rely 100% on explanation results? Well, explainability can build partial trust, but it’s not everything.

To be clear, explainability tells you which features the model is leaning on to make this decision, not the features actually causing the real-world outcome. A feature can be perfectly valid at design time and become a proxy in production.

At design time, you need to answer: Is this feature true right now, given today’s data? (gut-check, leakage audit, ablation)

At production/monitoring, you need to answer: Is it still true? (distribution monitoring, stability across regimes, importance drift).

The three checks above are a loop, integrated into a workflow that you do on a schedule. You may need to monitor model performance daily, weekly, or monthly because things change very quickly. It’s part of our job to identify the issues and fix them quickly.

## References
Case study from Netflix payment routing https://netflixtechblog.com/ml-observability-bring-transparency-to-payments-and-beyond-33073e260a38

Case study from Uber https://www.uber.com/us/en/blog/enabling-deep-model-explainability-with-integrated-gradients/

---
