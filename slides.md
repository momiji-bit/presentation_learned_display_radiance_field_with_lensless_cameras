---
theme: default
title: Learned Display Radiance Fields with Lensless Cameras
transition: slide-left
background: linear-gradient(135deg, #0f172a, #1e293b)
class: text-white
colorSchema: dark
---

<!-- Title -->
<h1 class="font-semibold tracking-tight leading-tight -mt-46">
  Learned Display Radiance Fields with Lensless Cameras
</h1>

<!-- Subtitle -->
<p class="italic text-xl text-gray-300">
  Accessible, angle-aware display calibration
</p>

<!-- Authors -->
<div class="text-lg text-gray-200 mt-4">
  <a href="https://ziyang.space"><span class="text-white font-bold">Ziyang Chen</span></a>, <a href="https://augvislab.github.io/people/yuta-itoh">Yuta Itoh</a>, and <a href="https://www.kaanaksit.com/">Kaan Akşit</a>
</div>

<!-- Lab logo in corner -->
<div class="absolute bottom-10 right-14">
  <img src="/logo.png" alt="lab logo" class="h-[200px] opacity-100">
</div>
<div class="absolute bottom-20 left-14">
  <img src="/sigasia.png" alt="lab logo" class="h-[100px] opacity-100">
</div>

<!--
Welcome everyone, and thank you for taking the time to join my presentation.
Today, I’m delighted to share our work, “Learned Display Radiance Fields with Lensless Cameras.”
This project is a collaborative effort with Professor Yuta Itoh from the **Institute of Science Tokyo** and my supervisor, Professor Kaan Akşit.👆

-->

---

# Motivation

<br>

<div class="absolute left-20 relative">
  <div class="bg-white p-2 rounded-lg shadow-lg relative inline-block">
    <video controls style="width: 700px; border-radius: 0.5rem;">
      <source src="/teaser.mp4" type="video/mp4">
    </video>
    <div class="absolute top-2 right-2 bg-black bg-opacity-60 text-white text-s px-2 py-1 rounded-md">
      Visual created using Adobe Firefly (AI-generated content)
    </div>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

---
layout: cover
---

#  Motivation

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
let us dive deep into our motivation, to understand the underlying research problems that led to our development.👆
-->

---

# Displays are ubiquitous in our life


<div class="absolute grid grid-cols-2 gap-2 mt-2 left-40">
  <div v-click="1" v-motion
    :initial="{ opacity: 0, x: -30 }"
    :enter="{ opacity: 1, x: 0 }">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/Laptop_computer_monitor_(Unsplash).jpg" 
          alt="sensor" 
          class="w-80 h-35 object-cover block">
    </div>
    <div class="text-center mt-1">
      Computer
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:Laptop_computer_monitor_(Unsplash).jpg">Taduuda taduuda</a>, CC0, via Wikimedia Commons
      </p>
    </div>
  </div>

  <div v-click="2" v-motion
    :initial="{ opacity: 0, x: -30 }"
    :enter="{ opacity: 1, x: 0 }">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/OpenStreetMap-on-Iphone15Plus.jpg" 
          alt="sensor" 
          class="w-80 h-35 object-cover block">
    </div>
    <div class="text-center mt-1">
      Smart phone
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:OpenStreetMap-on-Iphone15Plus.jpg">Infinite Bed</a>, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons
      </p>
    </div>
  </div>

  <div v-click="3" v-motion
    :initial="{ opacity: 0, x: -30 }"
    :enter="{ opacity: 1, x: 0 }">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/Apple_Vision_Pro_on_display_in_Toronto,_Canada_-_Aug_2024.jpg" 
          alt="sensor" 
          class="w-80 h-35 object-cover block">
    </div>
    <div class="text-center mt-1">
      Video-see-through AR headset
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:Apple_Vision_Pro_on_display_in_Toronto,_Canada_-_Aug_2024.jpg">LR.127</a>, <a href="https://creativecommons.org/licenses/by/4.0">CC BY 4.0</a>, via Wikimedia Commons
      </p>
    </div>
  </div>

  <div v-click="4" v-motion
    :initial="{ opacity: 0, x: -30 }"
    :enter="{ opacity: 1, x: 0 }">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/2CR_Soldiers_use_virtual_reality_for_Counter-Unmanned_Aerial_Systems_Training_(8867750).jpg" 
          alt="sensor" 
          class="w-80 h-35 object-cover block">
    </div>
    <div class="text-center mt-1">
      VR headset
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:2CR_Soldiers_use_virtual_reality_for_Counter-Unmanned_Aerial_Systems_Training_(8867750).jpg">U.S. Army photo by Pfc. Brent Lee</a>, Public domain, via Wikimedia Commons
      </p>
    </div>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
👆Displays are everywhere in our daily life, from 👆 computers to 👆 smart phones. In recent years, we are also exposed to the virtual imagery with rise of 👆 both AR and 👆 VR headsets

We want a perfect image shown on these displays 👆
-->

---

<v-click>
<span class="text-[2.5em] text-red-500">But, </span>
</v-click>

<v-click>
<span class="text-2xl">displays often suffer from defects such as:</span>
</v-click>

<br>
<br>

<div v-click="[3, 5]"
  v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 250 }"
  :click-4="{ x: 25 }"
  class="absolute ">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/backlight_bleeding.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Backlight bleeding
      </div>
  </div>
</div>

<div v-click=5
  v-motion
  :initial="{ x: 25}"
  class="absolute">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/backlight_bleeding_boxes.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Backlight bleeding
      </div>
  </div>
</div>

<div v-click=6 v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }" 
  class="absolute right-30 bottom-2">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/backlight_bleeding_boxes_zoom_in.png" 
            alt="sensor" 
            class="h-100 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Zoom in views
      </div>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
but 👆 not every pixel is equal👆 for example 👆 when I display a full black image with the liquid crystal display in our lab, you can see it does not produce an uniform black pattern 👆 if we zoom-in the image 👆👆 we can see that the light leaks from the edges. This is usually referred as backlight bleeding
-->

---

<v-click>
<p class="text-2xl">Displays also suffer from color inconsistency
</p>
</v-click>

<br>
<br>

<div v-click="[2, 4]"
  v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 250 }"
  :click-3="{ x: -1 }"
  class="absolute ">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/discoloration.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Discoloration
      </div>
  </div>
</div>

<div v-click=4
  v-motion
  :initial="{ x: -1}"
  class="absolute">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/discoloration_boxes.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Discoloration
      </div>
  </div>
</div>

<div v-click=4 class="absolute right-30 bottom-2" v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/discoloration_boxes_zoom_in.png" 
            alt="sensor" 
            class="h-100 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Zoom in views
      </div>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
👆 Despite the non uniform intensities,👆 displays also suffer from color inconsistency👆 Now, I turn on two displays in our lab👆 showing the identical color pattern on both displays 👆 As you can see the color varies dramatically from display to display.👆
-->

---

<div class="relative">

  <!-- FIRST TEXT (step 1) -->
  <span v-click="[1]" class="absolute top-0 left-0 text-2xl">
    These defects are visible at a 
    <span class="text-red-500">fixed viewing angle ❌</span>
  </span>

  <!-- SECOND TEXT (step 2) — SAME POSITION -->
  <span v-click="2" class="absolute top-0 left-0 text-2xl">
    These defects could be mitigated by the colorimeter:
  </span>

</div>

<div v-click="[1, 2]" class="absolute grid grid-cols-2 gap-2 mt-2 left-10 top-30">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/backlight_bleeding_boxes.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Backlight bleeding
      </div>
  </div>

  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/discoloration_boxes.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Discoloration
      </div>
  </div>
</div>

<div v-click=2 class="absolute right-5 top-0">
  <span class="text-red-500 " style="font-size: 25px;">❌ Fixed viewing position only</span>
</div>

<div v-click="2" class="flex justify-center items-center mt-2 h-full">
  <div v-click="1" v-motion
  :initial="{ x: 50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }">
  <br>
  <br>
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/Monitor_Calibration_2010-by-RaBoe-20.jpg" 
          alt="sensor" 
          class="w-auto h-80 object-cover block">
    </div>
    <div class="text-center mt-2">
      Colorimeter
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:Monitor_Calibration_2010-by-RaBoe-20.jpg">Ra Boe / Wikipedia</a>, <a href="https://creativecommons.org/licenses/by-sa/3.0/de/deed.en">CC BY-SA 3.0 DE</a>, via Wikimedia Commons
      </p>
    </div>
  </div>
</div>


<!-- 
Despite the display intrinsic deficiencies, 
 -->

---

<span class="relative top-0 left-0 text-2xl">
  The geometric viewing positions affects the percieved quality as well
</span>

<br>
<br>

<div class="absolute left-40" v-click="[1,2]" >
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <video controls autoplay muted loop playsinline style="width: 650px;"  >
      <source src="/rotational_display.mp4" type="video/mp4">
    </video>
  </div>
</div>

<div class="absolute left-40" v-click=2 >
  <div class="bg-white p-2 rounded-lg shadow-lg">
  <video controls autoplay muted loop playsinline style="width: 650px;"  >
    <source src="/depth_display.mp4" type="video/mp4">
  </video>
  </div>
</div>


<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
Another factor is the geometric viewing positions 👆 This could either due to rotations or 👆 changing position in depth 👆
-->

---

<v-click>
  <span class="relative top-0 left-0 text-2xl">
  These defects happen to near-eye display as well
  </span> 
</v-click>

<br>
<br>

<div class="absolute left-60" v-click>
  <div class="bg-white p-2 rounded-lg shadow-lg">
  <video controls autoplay muted loop playsinline style="width: 500px;"  >
    <source src="/ar_display.mp4" type="video/mp4">
  </video>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
These view-dependent distortions happen to near-eye displays as well. 👆Here is a footage captured with my Xreal glasses👆
-->

---


<v-click>
  <span class="relative top-0 left-0 text-2xl">
    The capability to capture defects with different geometric viewing position is important for:
  </span>
</v-click>

<br>
<br>

<div class="absolute flex justify-center gap-8 mt-2 left-35" >
  <div v-click="1" v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img src="/Logo_design_sketching.jpg" 
          alt="sensor" 
          class="w-80 h-60 object-cover block">
    </div>
    <div class="text-center mt-2">
      Graphic designer
      <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
        <a href="https://commons.wikimedia.org/wiki/File:Logo_design_sketching.jpg">Claireneon</a>, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons
      </p>
    </div>
  </div>

  <div v-click="2" v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }">
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/1280px-CCCamp_2019_by_CountCrapula_034.jpg" 
            alt="sensor" 
            class="w-80 h-60 object-cover block">
      </div>
      <div class="text-center mt-2">
        Video editor
        <p class="text-[0.55em] text-gray-500 mt-2" style="margin: 0;">
          <a href="https://commons.wikimedia.org/wiki/File:CCCamp_2019_by_CountCrapula_034.jpg">CountCrapula</a>, <a href="https://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>, via Wikimedia Commons
        </p>
      </div>
  </div>
</div>



<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!-- 
👆The capability to capture viewing-angle dependent distortion is critical for content creators such as graphic designers 👆 and video editors 👆, having a properly calibrated display with accurate color and intensity is essential, as they need to ensure a consistent and reliable visual experience across different devices and viewing environments.
-->



---

<v-clicks>

<div> 
  <span class="relative top-0 left-0 text-2xl">
    ISO standard defines procedures to assess the uniformity distribution of Flat Panel Displays (FPD)
  </span>
</div>

Which requires:

</v-clicks>

<v-clicks depth=2>

- A dark room
- Many captures of the displays with many viewing angles
- Complex setup & alignment

</v-clicks>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
👆 The industrial standard for evaluating the uniformity of flat panel displays requires 👆 measuring their brightness from 👆 many different viewing angles in a 👆 dark room — and that usually involves a 👆 complicated setup and precise alignment.
-->

---
transition: none
---

<br>

<div class="absolute left-40" >
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <video controls autoplay muted loop playsinline style="width: 650px;"  >
      <source src="/Conventional.mp4" type="video/mp4">
    </video>
  </div>
  <div class="text-center mt-2">
    Capturing angle-dependent intensity changes using the <a href="https://www.iso.org/standard/40100.html">ISO standard 9241-305:200</a>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
Here’s a time-lapse of me capturing the display’s angular intensity distribution using the ISO standard in our lab, which took about 10 minutes. As you can see I had to constantly move the camera ensuring it is pointing at the center of the display, and I have to account for the camera position using a checkerboard pattern for every capture.
-->

---

<br>

<div class="absolute left-40" >
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <video controls autoplay muted loop playsinline style="width: 650px;"  >
      <source src="/Heatmap.mp4" type="video/mp4">
    </video>
  </div>
  <div class="text-center mt-2">
    Captured angle-dependent intensity heatmap using the <a href="https://www.iso.org/standard/40100.html">ISO standard 9241-305:200</a>
  </div>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
And here is the captured display intensity heatmap with varying viewing angles.
-->

---

# Motivation


<br>
<br>
<br>
<br>
<br>
<span v-click class="text-3xl">
  <i>
  An <span class="text-green-500 font-semibold">accessible</span> method with a reasonable amount of captures is needed to measure the image quality of the displays from <span class="text-green-500 font-semibold">arbitrary viewpoints</span>.
  </i>

</span>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
This is a time-consuming and user-unfriendly task hinders daily calibration for average users which means: 👆
 an accessible method with reasonable amount of captures is needed to measure the image quality of the displays from arbitrary viewpoints
-->


---
layout: cover
---

# Implementation

<!--
Now let's talk about the implementation of our prototype
-->

---

# Idea

<br>
<v-clicks>

- **Lensless camera** captures angular information
- **Implicit Neural Representation** learns pixel light fields


<div  style="position: absolute; bottom: 30px" v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 140 }"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/lenless_lightfield_overview.png"
      class="w-150 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Pixel light fields acquisition with our lensless camera
  </div>
</div>

</v-clicks>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
We propose to use a 👆 lensless camera together with an 👆 implicit neural representation to measure angle-dependent display intensities,👆 with a simpler setup 👆. Before we dive into the setup, I would love to first explain what is lensless camera
-->

---
transition: fade
class: text-white
layout: default
---

# Imaging system

<div v-click=[1,2] style="position: absolute; left: 50%; bottom: 80px; transform: translateX(-50%);" >
  <div class="bg-black border-8 border-white p-2 rounded-lg shadow-lg">
    <img src="/conventional.png" alt="sensor" class="w-130 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    <span class="text-orange-500">One-to-One</span> mapping
  </div>
</div>

<div v-click=[2,3] style="position: absolute; left: 50%; bottom: 80px; transform: translateX(-50%);">
  <div class="bg-black border-8 border-white p-2 rounded-lg shadow-lg">
    <img src="/no_lens.png" alt="sensor" class="w-130 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    <span class="text-orange-500">One-to-Many</span> mapping
  </div>

</div>

<div v-click=3 style="position: absolute; left: 50%; bottom: 80px; transform: translateX(-50%);">
  <div class="bg-black border-8 border-white p-2 rounded-lg shadow-lg">
    <img src="/lensless_system.png" alt="sensor" class="w-130 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    <span class="text-orange-500">One-to-Selected</span> mapping
  </div>

</div>

<p v-click=1 style="position: absolute; bottom: 0px; right: 20%; font-size: 0.6em; color: #888;">
<a href="https://commons.wikimedia.org/wiki/File:Ccd-sensor.jpg">C-M</a>, <a href="http://creativecommons.org/licenses/by-sa/3.0/">CC BY-SA 3.0</a>, via Wikimedia Commons
</p>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
Let's first think of the conventional imaging setup👆, where we have the object on the left, lens at the middle, and sensor on the right. In this case, each point on the object has a one-to-one mapping on the imaging sensor. This one-to-one mapping does not provide any angular diversity for the captured points👆

Without a lens, light rays from every point on the object spread out and hit multiple points on the sensor simultaneously, creating an unfocused image that captures information from wide varity of angles.👆

By adding a specially designed mask, we can control or modulate the light reaching to the sensor. This creates a typical lensless imaging system where the target images are reconstructed with post-processing pipelines
You can see from this plot that only secleted light are able to pass and captured by the sensor.
In our proposal we will be utilizing lensless camera's ability to capture angular information with a single shot. whereas the conventional cameras would require multiple captures
-->

---

# Forward model

<div v-click>

We model the lensless camera as a linear convolution process. 

</div>


<div v-click="[1,2]" >
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_0.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>


<!-- lensless image -->
<div v-click="[2,3]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_1.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution_1.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- covolution -->
<div v-click="[3,4]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_2.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution_2.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- display pixel -->
<div v-click="[4,5]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_5.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution_3.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- PSF -->
<div v-click="[5,6]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_6.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution_4.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- zero-pad -->
<div v-click="[6,7]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_3.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- cropping -->
<div v-click="[7,8]">
  <div class="absolute left-1/2 pt-15px transform -translate-x-1/2 ">
      <img 
        src="/linear_system_4.png"
        class="w-auto h-8 object-cover"
      >
  </div>
  
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<!-- lienar -->
<div v-click="[8,10]">
  <div class="absolute left-1/2 transform -translate-x-1/2 ">
      <img 
        src="/linear_system_10.png"
        class="w-auto h-18 object-cover"
      >
  </div>
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<div v-click="[9,10]">
  <div class="absolute left-1/2 transform -translate-x-1/2 ">
      <img 
        src="/linear_system_7.png"
        class="w-auto h-18 object-cover"
      >
  </div>
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<div v-click=10>
  <div class="absolute left-1/2 transform -translate-x-1/2 ">
      <img 
        src="/linear_system_8.png"
        class="w-auto h-18 object-cover"
      >
  </div>
  <div  style="position: absolute; bottom: 65px; left: 100px;">
    <div class="bg-white p-2 rounded-lg shadow-lg">
      <img 
        src="/lensless_convolution.png"
        class="w-190 h-auto object-cover"
      >
    </div>
    <div class="text-center mt-2">
      Lensless convolution
    </div>
  </div>
</div>

<div v-click=8 class="absolute right-15 top-10">

<span class="text-green-500 " style="font-size: 30px;">✅ Linear operation</span>

</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
We model our lensless camera as a convolutional approximation, 👆 where the 👆sensor measurement 👆is represented as a linear convolution 👆 between the display pixel and the👆  precaptured point spread function (PSF) . Note that we first zero-pad 👆 and then 👆 crop them after convolution to avoid circular convolution.
This formulation assumes spatial invariance within the local region of interest, allowing us to describe the image formation process as a 👆linear operation that can be done efficiently with the 👆 discrete fourier transform and the 👆inverse fourier transform.
-->

---

# Hardware

<div v-click>
We need to keep the aperture small to maintain spatial invariance 
</div>

<br>

<div v-click="[1,2]" v-motion
  :initial="{ x: -40 }"
  :enter="{ x: 250 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/incident_angle.png"
      class="w-80 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
</div>

<div v-click="[2,3]" v-motion
  :enter="{ x: 250 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/incident_angle_1.png"
      class="w-80 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
</div>

<div v-click="[3,5]" v-motion
  :enter="{ x: 250 }"
  :leave="{ x: 40 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/incident_angle_2.png"
      class="w-80 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
</div>

<div v-click="[4,5]" class="absolute right-15 top-10">

<span class="text-red-500 " style="font-size: 30px;"> ❌ Limited FoV</span>

</div>

<div v-click="[5,6]" v-motion
  :initial="{ x: -40 }"
  :enter="{ x: 200 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/fov_expansion_3.png"
      class="w-130 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
  
</div> 

<div v-click="[6,7]" v-motion
  :enter="{ x: 200 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/fov_expansion_1.png"
      class="w-130 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
  
</div> 

<div v-click="7" v-motion
  :enter="{ x: 200 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/fov_expansion_0.png"
      class="w-130 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Incident angle illustration
  </div>
  
</div> 

<div v-click="5" class="absolute right-15 top-10">

<span class="text-green-500 " style="font-size: 30px;"> ✅ Expanded FoV</span>

</div>
<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
In order to maintain spatial invariance, 👆 we need to keep the aperture small 👆, however the field of view is largely dependent on the aperture size👆 and this samll aperture limits the practical FoV 👆. Therefore, we use an aperture array 👆 to expand the FoV, under the assumption that the adjecent pixels have similar behavior. so as we keep the sensor fixed, we turn on the pixels sequencially 👆👆.
-->

---

# Hardware

<div>
We sample nine positions on the display with our lensless camera
</div>

<br>

<div v-click="[1,2]" v-motion
  :initial="{ x: 200 }"
  :enter="{ x: 300 }"
  :leave="{ x: 30 }"
  class="absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/lensless_camera.png"
      class="w-70 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Top view of our lensless camera
  </div>
</div>

<div v-click="2" class="absolute left-40" >
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <video controls autoplay muted loop playsinline style="width: 625px;"  >
      <source src="/Ours.mp4" type="video/mp4">
    </video>
  </div>
  <div class="text-center mt-2">
    Capturing angle-dependent intensities using our prototype
  </div>
</div>

<div v-click="3" class="absolute right-15 top-10">

<span class="text-green-500 " style="font-size: 30px;"> ✅ No darkroom needed!</span>

</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
We sample nine positions of the display with our prototype 👆 and here 👆 is a time-lapse of our proposed workflow, which takes about 2 minutes and 👆 no dark room is needed
-->

---

# Hardware

<v-click>
We still need to capture the Point Spread Function (PSF) for each aperture
</v-click>

<br>
<br>

<div v-click="[2, 4]"
  v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 250 }"
  :click-3="{ x: -1 }"
  class="absolute ">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/psf_capture_setup.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Capture setup
      </div>
  </div>
</div>

<div v-click=4
  v-motion
  :initial="{ x: -1}"
  class="absolute">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/psf_capture_setup.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Capture setup
      </div>
  </div>
</div>

<div v-click=4 class="absolute right-10 bottom-8" v-motion
  :initial="{ x: -50 }"
  :enter="{ x: 0 }"
  :leave="{ x: 50 }">
  <div>
      <div class="bg-white p-2 rounded-lg shadow-lg">
        <img src="/psf_set.png" 
            alt="sensor" 
            class="h-80 w-auto object-cover block">
      </div>
      <div class="text-center mt-2">
        Captured PSF set
      </div>
  </div>
</div>




<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
👆In order to complete the forward model, we still need to capture point spread funciton for each aperture, 👆 this is our setup to capture the PSF which includes a 4f system, a pinhole and a lensless camera.👆👆. On right is the cpatured PSFs
-->


---

# Implicit Neural Representation

<br>

<v-clicks>


<!-- THIS ONE WORKS SO WELL -->
<div v-motion
  :initial="{ x: 10 }"
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>

<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_1.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>

<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_2.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>


<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_3.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>

<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_4.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>
<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_5.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>
<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_6.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>
<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_7.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>

<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_8.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>

<div v-motion
  :enter="{ x: 100 }"
  class="absolute"
  >
  <div class="bg-white p-2 rounded-lg shadow-lg inline-block">
    <img src="/model_pipeline_9.png" alt="sensor" class="w-175 h-auto object-cover">
  </div>
  <div class="text-center mt-2">
    Model overview
  </div>
</div>




</v-clicks>


<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
After we collect the lensless captures we applied the 👆 implicit neural representation to encode the angular information.
We model the inputs 👆 as three groups of coordinates. 👆 The x,y is the display pixel coordinates indicating where do we capture the image, 👆 the u,v is the light field anular coordinates which is calculated based on the geometry we showed earlier, 👆 the s,t is the image coordinates just like the other implicit neural representation methods for image compression. For each of the coordinate group, we apply positional encoding 👆 at different frequency levels.
These encoded features are then concatenated 👆 and passed through 👆 fully connected layers with 32 neurons and a sinusoidal activation.
We 👆 convolve the output reconstruction with the pre-captured point spread functions to predict the lensless image.
Finally, 👆 we compare the predicted image with the captured one to compute the loss.
-->

---

# Results




<div v-click="[1,2]" v-motion
  :initial="{ x: 10 }"
  :enter="{ x: 200 }"class = "absolute bottom-100px">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/capture_illustration.png"
      class="w-125 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    The average intensity comparison
  </div>
</div>

<div v-click="2" v-motion
  :initial="{ x: 200 }"
  :leave="{ x: 20 }"class = "absolute bottom-50px">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/capture_illustration_1.png"
      class="w-125 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    The average intensity comparison
  </div>
</div>

<div v-click="3" class="absolute right-15 top-10">

<span class="text-yellow-500 " style="font-size: 30px"> Average intensity per view</span>

</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
👆 To benchmark our method, we use the ISO standard from −10◦ to 16◦ vertical incident angles and we simulate the display intensity with our model. 👆 this plot shows the average intensity 👆 for each view and our method reproduces the ISO intensity trend, validating its physical plausibility.
-->

---

# Results

<div>
We compare the simulated display digital twin with the real-world display capture that is captured with a conventional camera
</div>

<br>

<v-clicks>

<div  v-motion
  :initial="{ x: 20 }"
  :enter="{ x: 150 }"class = "absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/real_world_simulation.png"
      class="w-150 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Comparison between real-world and simulation display
  </div>
</div>
</v-clicks>



<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
Despite of simulating the average intensity of the display,👆 our model is capable of generate the full display
-->

---

# Results

<div>

We can also simulate the pixels at close viewpoints
</div>

<br>

<v-clicks>
<div  v-motion
  :initial="{ x: 20 }"
  :enter="{ x: 150 }"class = "absolute">
  <div class="bg-white p-2 rounded-lg shadow-lg">
    <img 
      src="/supplementary_zoomin.png"
      class="w-150 h-auto object-cover"
    >
  </div>
  <div class="text-center mt-2">
    Comparison between real-world and simulation display
  </div>
</div>
</v-clicks>


<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>
<!-- 
👆 We can also simulate a close up view of the display.
-->

---

<div class="text-left text-white text-6xl font-bold mt-20">
  Thank you for listening!
</div>
<br>
<br>
<br>

<!-- Authors -->
<div class="flex justify-left items-end text-center text-gray-200 gap-16">
  <!-- Ziyang Chen -->
  <div class="flex flex-col items-center">
    <a href="https://ziyang.space" target="_blank" class="flex flex-col items-center">
      <img src="/ziyang_chen.jpg" alt="Ziyang Chen"
           class="w-24 h-24 rounded-full mb-2 border border-white-500 object-cover object-center transform translate-y-1">
      <span class="text-xl leading-6 text-white font-bold">Ziyang Chen</span>
    </a>
  </div>

  <!-- Yuta Itoh -->
  <div class="flex flex-col items-center">
    <a href="https://augvislab.github.io/people/yuta-itoh" target="_blank" class="flex flex-col items-center">
      <img src="/yuta-itoh.jpg" alt="Yuta Itoh"
           class="w-20 h-20 rounded-full mb-2 border border-gray-500 object-cover object-center opacity-60 hover:opacity-100 transition duration-300">
      <span class="text-lg leading-6 text-gray-200">Yuta Itoh</span>
    </a>
  </div>

  <!-- Kaan Akşit -->
  <div class="flex flex-col items-center">
    <a href="https://www.kaanaksit.com/" target="_blank" class="flex flex-col items-center">
      <img src="/kaan_aksit.jpg" alt="Kaan Akşit"
           class="w-20 h-20 rounded-full mb-2 border border-gray-500 object-cover object-center opacity-60 hover:opacity-100 transition duration-300">
      <span class="text-lg leading-6 text-gray-200">Kaan Akşit</span>
    </a>
  </div>
</div>
<br>
<p class="flex flex-row items-center text-xl font-bold space-x-2">
  <span>Email:</span>
  <a href="mailto:ziyang.chen.22@ucl.ac.uk" class="text-blue-400 hover:underline">
    ziyang.chen.22@ucl.ac.uk
  </a>
</p>
<!-- Lab logo in corner -->
<div class="absolute bottom-20 right-60">
  <img src="/logo.png" alt="lab logo" class="h-[200px] opacity-100">
</div>

<div class="absolute bottom-20 right-14">
  <a href="https://complightlab.com/publications/lensless_display_radiance_field/" target="_blank">
  <img src="/QR.png" alt="lab logo" class="h-[200px] opacity-100" >
  </a>
</div>

<div class="absolute bottom-4 right-6 text-sm text-gray-400">
  <SlideCurrentNo /> / <SlidesTotal />
</div>

<!--
thank you for listening i am happy to answer any questions in the Q&A section.
-->
