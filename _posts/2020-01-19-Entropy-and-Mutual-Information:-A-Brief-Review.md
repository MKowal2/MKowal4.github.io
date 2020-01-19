---
layout: post
title: "Entropy and Mutual Information: A Brief Overview"
author: "Matthew Kowal"
meta: "Springfield"
---

Hi there! This is the innagural post for the Low Loss Blog, a place where I document my attempts to become a better researcher and try to understand the reasons why the neural networks I am training aren't (or are!) learning what they are supposed to. I am hoping that people reading this can learn from the the gaps in my own knowledge that I have (fingers crossed) filled in. To read this blog, I assume an undergraduate level of understanding in probability, calculus, and linear algebra. However if you don't have this background, I hope that the intuition I give along with the mathematical formalisms can also aid you in your understanding of these concepts as all concepts I will cover in this blog will have tangible implecations outside of the realm of theoretical mathematics. 

## Understanding Mutual Information

For this post I hope to accomplish a few different things. I will review a brilliant [document](https://people.cs.umass.edu/~elm/Teaching/370_F09/mutInf.pdf) put together by Erik G. Learned-Miller. I found this document when attempting to better understand the concept of 'Mutual Information', and it has been by far the most influential document in my understanding of entropy and mutual information. It is a short document, only 3 pages, with the purpose of being an introduction to entropy and mutual information for discrete random variables. Erik does something that I think so many teachers miss when introducing students to new concepts, which is using real-world, easy to understand examples to aid with the formulas. I hope I can add some intuition behind what entropy, joint entropy, and mutual information actually represent, as well as review some simple and more complex examples that I am currently working on in my research. 

### Entropy

When I started to dig into what mutual information really represents, the word I kept running into was: Entropy. Now I heard about entropy way back in highschool chemistry and physics but I'll be perfectly honest; I had no real understanding of the term, especially since I couldn't see how it could seemingly be so related to every field of science. So, what does *entropy* mean?

Entropy refers to the amount of uncertainty that an event has. If an event A has multiple outcomes, and you have no idea which outcome will occur, the event A has *high* entropy. For example, if you roll a fair 6-sided die, this event has a high entropy because each number is equally likely to appear during the roll. If event B has multiple outcomes, and you are very certain about the outcome, the event has *low* entropy. For example, if event is whether or not the sun will come up tomorrow, this event (hopefully) has a low entropy, since we are confident that the sun will come up tomorrow morning. 


### Joint Entropy


### Mutual Information

