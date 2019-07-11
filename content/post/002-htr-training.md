---
date: "2019-07-03"
tags: ["services", "training", "htr"]
title: "Prepwork for Handwritten Text Recognition"
author: "Alice Tarrant"
---

<p align="center"><img src="../../images/201907-001-spivakdoc.jpg"/></p>
<figcaption>Sample of a handwritten document written by Dr. Charles Spivak. Image credit: Ira M. and Peryle Hayutin Beck Memorial Archives, University of Denver</figcaption>

My name is Alice Tarrant and I am the Digitization Coordinator at the University of Denver. I manage the digitization and treatment of library materials at the Anderson Academic Commons. Recently my employees and I have been working on the HTR (Handwritten Text Recognition) of the JCRS collection. In this post I will describe the technical side of HTR using Transkribus, as well as some of the challenges we have encountered thus far.

HTR is more difficult than recognizing typed text for obvious reasons: the characters aren’t uniform, there are no preexisting fonts to check against, different people can have vastly different styles of handwriting, etc. To overcome these obstacles, Transkribus uses machine learning, specifically a recursive deep neural network. A neural network is a structure formed of a large number of unweighted nodes called “neurons”. At first, these neurons will output gibberish due to the random weights. However, we can “train” the network by showing it a set of finished transcriptions. Over time, the network will become more and more accurate in transcribing the texts. The “deep” label of a “recursive deep neural network” simply means the neurons are organized into consecutive layers; the recursive label means the network feeds the results from the previous characters into the next one, letting it learn a sort of context on what characters follow another.

Deep neural networks can be very powerful, but they have a major limitation: we need a set of finished transcriptions for it to learn from. Transkribus recommends having at least 10,000 words per hand to get good results from the training. So before we can expect any kind of usable result, we have to do a large amount of transcriptions by hand first. For our first training set, we are transcribing the hand of Dr. Charles Spivak, the Secretary of the JCRS. We chose him since he has the most handwritten documents in the collection, and could make the largest training set.

First an overview of how manual transcription works in Transkribus. The first step when transcribing a page is layout analysis. Transkribus groups lines of texts into “text boxes”. Each line of text within that text box is then marked separately. Transkribus can do layout analysis automatically, or the user can do it manually.

<p align="center"><img src="../../images/201907-002-layoutanalysis.png"/></p>
<figcaption>Layout analysis in Transkribus. Transkribus automatically detects baselines and text areas, we do some manual correction. Image credit: Ira M. and Peryle Hayutin Beck Memorial Archives, University of Denver</figcaption>

Once layout analysis is complete we can begin transcribing. This is fairly simple: we go line by line and type the text corresponding to that line. Transkribus supports bold, italics, underlines, subscripts, superscripts, and strikethroughs, so we can teach the model to recognize these things in the text. Once we have finished transcribing all lines, the page is marked as done, saved, and we move on to the next one.

While the process of transcribing is fairly simple, the nature of the documents themselves present certain challenges. The most prominent issue is the quality of the handwriting. The handwriting Dr. Spivak ranges from hard to read to almost completely illegible. The model bases its transcriptions off the manual work we do, and if we as humans are unable to read it, the machine doesn’t have a chance. To overcome this problem, we have been posting hard to decipher words in a department Slack channel so that others may help. I have also written a script in which I can enter the parts of the word I can read and it will search a dictionary file for compatible words.

Another issue is deciding when to include misspellings and typos. Transkribus reads words on a per character basis, so at first blush it would seem that including typos would be the best course. However, Transkribus will also determine the identity of a character based on the previous characters on the line. Therefore it may be useful to use the intended spelling of a word so that Transkribus will learn the context of that word. We ultimately decided to leave the typos in if the word was mostly legible. In some cases, however, it is unclear whether a typo has occurred or not: the word is so illegible that it seems that they may have spelled it wrong or left out a character, but we are unsure. In these cases, we put the word as intended, hoping that context will make up for illegibility.

<p align="center"><img src="../../images/201907-003-transcription.png"/></p>
<figcaption>Manual transcription in Transkribus to build our training set. Image credit: Ira M. and Peryle Hayutin Beck Memorial Archives, University of Denver</figcaption>

The required size of the training set has also been a source of difficulty. For best results, a model should be trained on a single hand. However, the vast majority of documents in the JCRS collection are from the families and friends of patients. The documents that are written by the doctors are often short or typed. Thus we spend a good deal of time sitting through the documents looking for eligible pages. We have a long way to go: preliminary training attempts max out at 25% character error rate, bottoming out at around 20 epochs (full passes through the training set). This is a small number of epochs to stop learning at, and the only way to solve it is to expand the training set. However, the character error rate is very promising and makes me optimistic that a useable model is attainable.

These are the major problems at the moment for this project. I will be interested to see in the coming months which new challenges appear as I train more of my employees on Transkribus and approach a trainable data set size.