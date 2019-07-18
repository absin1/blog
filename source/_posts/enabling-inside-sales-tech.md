---
title: The Tech
date: 2019-07-18 17:53:57
tags:
---
How are we technically achieving [this](/blog/2019/07/17/enabling-inside-sales) vision?

A contextual streaming speech recognition engine with transcribing millions of conversations concurrently with a latency of milliseconds. We experiment with various architectures including TDNN+LSTM, RNN-T, CTC and employ the ones that makes most sense based on the use case. We also employ language model and trie enriched with custom vocabulary to ensure high accuracy of our ASR system.

We use CNNs with attention mechanism to perform emotion recognition and while doing this we use both the text and speech signals to gain an accurate insight into the emotional state of the conversation.

We remember all the conversations that have ever happened successful or otherwise in graph-structured bidirectional LSTMs. Often there is information lurking in the unstructured texts that reside in all the training and product spec documents. We encash all that with KGCNs (Knowledge Graph Convolutional Network) and try to factor this into equation too.

We feed all this information into a Reinforcement Learning conversation engine which is DNDTs (Deep Neural Decision Trees).

With this abstraction we have ample control over how the conversation flows and how we can tweak it up for specific use cases while maintaining inherent learnability in the system from just conversational data.

Of course to do all this we  integrate with almost all the dialers that a small business to an enterprise would use. A robust architecture ensures that we are able to seamlessly integrate into most of the CRMs that we find in the wild.
