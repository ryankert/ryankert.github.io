---
title: Machine Learning Notes Ch1 Regression
tags: [ML, DL]
categories: [Machine Learning]
date: 2023/1/17 10:46:25
mathjax: true
---

## Machine Learning

is looking for function

## Different types of function

- Regression
  - output is a scalar
  - eg. next day PM2.5 rate
- Classification
  - Given options(classes), the function outputs the correct one
  - eg. AlphaGo, chose the right position to play
- Structured Learning
  - create structured things (image, texts)

## Steps

example : predict teacher's youtube channels views per day

1. Funciton with Unknown Parameters
   $$y=f(DATA)$$
   Data : previous data in this case
   **Model**
   $$y=b+w*x_1$$
   y: numbers of views on 2/26
   $x_1$: numbers of views on 2/25 (one day earlier)
   w (weight), b (bias) are unknown parameters that we learned from data.

2. Define Loss from Training Data
   - Loss is a funciton of paramenters, $L(b,W)$
   - represent how good a set of values is.
