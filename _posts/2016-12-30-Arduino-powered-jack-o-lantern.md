---
published: true
title: 'Arduino-Powered Jack'o'lantern'
permalink: /2016/arduino-jack-o-lantern
author: bradford
layout: post
categories:
  - Technology
  - Tutorials
tags:
  - Arduino
comments: true
---
_This Halloween I decided to have a pumpkin-carving contest with my wife. I wanted to leverage my talents, so I took this as an opportunity to buy and play with an Arduino. I know, I'm about a decade late, but it was pretty fun_

I picked up the [Kuman Project Starter kit](https://www.amazon.com/gp/product/B016D5KUHS/ref=oh_aui_search_detailpage?ie=UTF8&psc=1) as well as an [infrared PIR Motion Sensor](https://www.amazon.com/gp/product/B00UMKZ4AE/ref=oh_aui_search_detailpage?ie=UTF8&psc=1), [SD Card Shield](https://www.amazon.com/gp/product/B00KAE24PA/ref=oh_aui_search_detailpage?ie=UTF8&psc=1), and [amplifier](https://www.amazon.com/gp/product/B008BGLMNY/ref=oh_aui_search_detailpage?ie=UTF8&psc=1) from Amazon. Along with some old speakers and 5v power bank I already had, these were all the components I needed to get my pumpkin to scream.

My daughter has a pet slug (just a little toy), so I decided to turn my pumpkin into a horrible monster slug. My goal, ultimately, was to have an eerie 'flickering' candle (LED), Green LED eyes on the end of stalks, and to have it respond to motion by playing one of several scary sounds.

Plugging it all together wasn't difficult, though it was a bit cluttered. Perhaps a case is in order for next time, as long as it can house all the components I need.

Unfortunately I don't have a photo of it assembled, besides the video below, however [here is the code that put it all together](https://github.com/elBradford/snippets/blob/master/HalloweenSlugOLantern.ino):

```arduino
#include <RBD_Timer.h>
#include <RBD_Light.h>
#include "SD.h"
#include "TMRpcm.h"
#include "SPI.h"

#define SD_ChipSelectPin 4


TMRpcm tmrpcm;
int redpin=3;      //Pin 10
int greenpin=5;    //Pin 11
int bluepin=6;      //Pin 12
int colorr = 0;
int colorg = 0;
int colorb = 0;
long flickerTime = 0;
int played = 0;
RBD::Light eye(10);
int pirPin = 7;

char *sounds[6] = {"slimer.wav", "growl.wav", "growl2.wav", "scream.wav", "scream2.wav", "scream3.wav"};

void setup(){
  // SOUND
  tmrpcm.speakerPin = 9;
  Serial.begin(9600);
  if (!SD.begin(SD_ChipSelectPin)) {
    Serial.println("SD fail");
  }

  // MOTION SENSOR
  pinMode(pirPin, INPUT);

  // COLOR LED
  randomSeed(analogRead(0));
  tmrpcm.setVolume(6);

  eye.fade(1000,6000,1000,2000);
}

void loop(){

  // DETECT MOTION
  if (digitalRead(pirPin)){
    // PLAY SOUND IF SOUND ISN'T PLAYING AND HASN'T ALREADY PLAYED
    if (!played && !tmrpcm.isPlaying()){
      tmrpcm.play(sounds[random(6)%6]);
      played = 1;
    }

    // TURN LIGHT RED
    analogWrite(redpin, 255);
    analogWrite(greenpin, 0);
    analogWrite(bluepin, 0);

  } else {
    played = 0;

    // CANDLE FLICKER
    if (millis() >= flickerTime){
      flickerTime = millis() + random(600);
      colorr = random(128);
      colorg = random(255-colorr);
      colorb = random(20);

      analogWrite(redpin, 128+colorr);
      analogWrite(greenpin, colorg);
      analogWrite(bluepin, colorb);
    }
  }

  eye.update();

}
```

[![Arduino Pumpkin Slug](https://i.ytimg.com/vi_webp/lGfjO_fyixk/maxresdefault.webp)](https://youtu.be/lGfjO_fyixk)
