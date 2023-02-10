---
title: Cognitive Speech Service formats file sizes and response times
author: Martin Kearn
description: The Cognitive Speech Service provide the ability to convert text to speech with various voices and other attributes. One attribute that can be conigured is the output format which has a big impact on the overall response size and time, which can make a different to the latency of the request. In this article, I'll discuss some tests I ran
image: https://dummyimage.com/800x600/000/fff&text=placeholder
thumbnail: https://dummyimage.com/200x200/000/fff&text=placeholder
status: draft
published: 2018/12/18 09:00:00
type: article
categories: 
  - Cognitive Services
  - Speech
---

# Cognitive Speech Service formats, file sizes and response times

The [Cognitive Speech Service](https://azure.microsoft.com/en-us/services/cognitive-services/speech-services/) provides the ability to convert text to speech with various voices and other attributes.

One attribute that can be configured is the output format which has a big impact on the overall response size and time. The size and time of the response can make a difference to the latency of the request.

In this article, I'll discuss some tests I ran.

## SSML used to test with
With the text-to-speech API, the text that needs to be converted to speech should be passed as an [Speech Synthesis Markup Language (SSML)](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-synthesis-markup) document in the body of the request.

SSML let you set parameters such as pitch, speed, voice, pronunciation, pauses and lot of other things which control how the audio is pronounced.

In order to have a representative sample, I used this SSML which includes most of the popular settings:

```
<speak version='1.0' xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)'>
Good morning Jess, how are you today? It's a lovely day here in London isn't it?
</voice>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody rate="default" pitch="high">
Good morning Guy <break time="500ms" /> Yes I <emphasis level="strong">love</emphasis> coming to London. I can't get used to the whole 
<phoneme alphabet="ipa" ph="t&#x259;ma&#x325;&#x27E;o&#x325;"> tomato </phoneme>
<phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
thing though!
</prosody>
</voice>
</speak>
```

You will notice that I've used two different voices above. One ends in 'Neural' and one ends with 'RUS'.

The 'Neural' voice is a new range of voice fonts that use neural-network algorithms to create a more natural, engaging and realistic voice compared to the previous 'RUS' voices. You can read all about it on [Microsoft previews neural network text-to-speech](https://azure.microsoft.com/en-gb/blog/microsoft-previews-neural-network-text-to-speech/). 'RUS' stands for "rich unit of speech" which is a more traditional way to construct artificial voice fonts by effectively combining individual words.

At the time of writing the `prosody` markup is not supported by the new neural voices, only RUS.
