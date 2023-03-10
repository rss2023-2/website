**Team #2 Ritika Jeloka, Ian Perez, Jason Salmon, Esha Ranade, Sydney Chun**

**6.4200 Robotics: Science and Systems**

**March 10th, 2023**

# Lab 3 Report: Wall-Following on the Racecar
Edited by: Ritika Jeloka

## Requirements

### Audience
RSS faculty and staff, hypothetical managers, and professionals in the field (including your potential employers).

### Purpose
Write a persuasive argument demonstrating to faculty that you understand the Lab content and how it fits into the context of the class, and that the algorithmic solution you designed is sound and works well in experiments or simulations.  Make claims for your work, supported by detailed technical explanation, justification, and experimental analysis that would be persuasive to a hypothetical manager unfamiliar with the Lab.

### Rubric
See “Rubric for reports” on Canvas (Modules section).

### Visuals
All visual support (graphs, charts, images, clips, tables, etc.) must be numbered, titled, captioned, and cross-referenced in-text. See the "Formatting examples for figures, pseudocode, etc" section below for examples on how to do this using markdown.

### Other
Label each section with the author’s name.

Technical grades will be team-based; CI grades will be individual.

You may peer review each other’s sections for the purpose of learning from each other.

The report as a whole should be edited for consistency and clarity; the editor’s name should appear at the top, and editing tasks should be shared over all the Reports.

## 1. Introduction (Esha Ranade)
In this lab, we apply the wall-following functionality achieved in our previous lab to a physical car. The shift from simulation to robot introduces several new challenges which we navigated through during this week. The functioning wall-following and safety overriding mechanisms bring our team one step closer to a fully autonomous vehicle by ensuring that it does not crash into unintended 

Our team was expecting to

Motivates and contextualizes this lab’s goals (i.e., identifies **what** you have designed in this lab and **how** that fits among the other RSS labs or how it contributes to developing an autonomous system). Presents an overview of the **purpose** and **specifications** of this lab. Provides a short and informal summary of the **technical problem** and introduces a bird’s-eye view of your technical solution.

(no more than 500 words)

## 2. Technical Approach
Formally presents the **technical problem** you have to solve in the lab.

Describes your team’s **initial set-up**, **technical approach**, and **ROS implementation**. Discusses the different building blocks of your technical approach.

- Understanding the output from the Velodyne LIDAR sensor

Introduces required mathematical symbols and reports key mathematical relations to present the approach.

In addition to reporting on the **technical solution** you devised in response to the technical problem posted in this lab, this section explains the **how** of your approach, and should **justify** your team’s design choices and the rationale behind any tradeoffs. (Why these and not other choices?)

Any subsection must be numbered and start with a high-level overview that orients the reader.

Finally, remember to use figures to help the reader understand your approach.

(no more than 2250 words)

[Esha]

During this process, our team encountered several issues that we needed to overcome in order to get the robot running successfully. 

Our unfamiliarity with the Velodyne LIDAR sensor and the additional preprocessing required to render its outputs useful led us to spend much of our time trying to understand how to parse the sensor data. 

The provided information suggested that the Velodyne sensors were placed on the robot with a 60 degree rotation, something we were able to verify by observing the direction of the logo on the robot's sensor. Figure ___(TODO: HI FILL ME IN PLZ)___ below depicts the angles measured by the Velodyne sensor and their correspondances to the robot's forward direction, which is what we had used to control steering in our simulation from lab 2. We originally considered using simple algebraic operations to apply an offset to the measured angles in order to map them to their bearings in relation to the front of the robot. However, we ultimately used a different strategy which is detailed further below.

Our team noticed an uhnexpected fluctuation in the LIDAR sensor data: alternating high and low values in succession. Through discussion with the other teams in Pod 1, we realized that the Velodyne appeared to be publishing two halves of the scan to the same topic, resulting in alternating data values. This split is shown in Figure _____(TODO: HI FILL ME IN PLZ)____. To address this, we created a custom intermediary node where the ranges of the two LaserScan messages data provided by the sensor were first concatenated into a single array, with all of the values and angles in order. This array was then sent to the `/scan` topic, allowing us to parse continuous data.

We also noticed a phenomenon where several "inf" or infinite values were being measured by the sensor. We speculated that these occured when the closest object being sensed was either too close or too far away from the sensor. Rather than filtering these out entirely, which would have decreased the total number of data points and changed our index calculations for a desired direction, we initially replaced the 'inf' values with the maximum number detected in the range. Using the maximum distance would prevent the robot from actually using it in a PID calculation for heading. However, we later realized that each of the two different ranges of scan data included the full 360 degrees worth of distances, though only half of each was relevant. The 'inf' values represented the points not included in a certain half's intended range. By combining the two scans in such a way that only the non-'inf' value corresponding to a desired direction was included in the distance calculations, we were able to get the correct values required for our PID control.



## 3. Experimental Evaluation
The purpose of this section is to **provide evidence of the functionality** of your design, and to **document your experimental evaluation**. The section should explain both:
- **what** was tested and **why**, and **how** those tests were performed (Technical Procedures, including a clear definition of the performance metrics used in the analysis),
- and **discuss the result** of those tests to arrive at an assessment of the functionalities you implemented in this lab (Results).

**You can find ideas and suggestions in the “Good Experimental Evaluation” Recitation on Canvas (Modules section).**

(no more than 1250 words)

## 4. Conclusion
Summarizes what you have achieved in this design phase, and notes any work that has yet to be done to complete this phase successfully, before moving on to the next. May make a nod to the next design phase.

(no more than 500 words)

## 5. Lessons Learned
Presents individually authored self-reflections on technical, communication, and collaboration lessons you have learned in the course of this lab.

## Formatting examples for figures, pseudocode, etc

### Tables   

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

### Images/Videos   

Left Straight Wall
<video src= https://user-images.githubusercontent.com/76889241/224224818-70a1ebbe-f8d3-4849-82d8-0da88c31f2f5.mp4 />

Left Corner Turn
<video src= https://user-images.githubusercontent.com/76889241/224224854-7cac1b5a-1b80-46d8-a305-45eed0054f7f.mp4 />

Right Corner Turn
<video src=https://user-images.githubusercontent.com/76889241/224225169-ee903f28-2873-453a-84bd-65ee11758a39.mp4 />

Right Straight Wall
<video src= https://user-images.githubusercontent.com/76889241/224224962-f124bf3b-9123-4b24-a1c2-92d7ef4fa92f.mp4 />

Make sure you place images in the right folders for them to actually be rendered in html  
![alt text](https://cdn-ssl.s7.disneystore.com/is/image/DisneyShopping/3061105551624-1  "Iron Man")   
Why is the image so large? Because you have to manual resize if you use markdown... try using html to include the image like this one

| <img src="http://www.storywarren.com/wp-content/uploads/2016/09/space-1.jpg" width="50%"> |
|:--:|
| This is a caption for our image! Ooo, Ahhh **SPACE** |  

Look at this https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet  

### LaTeX

$$
\begin{bmatrix}
  \lambda&0\\ 0&\lambda
\end{bmatrix}=\lambda I
$$

This is inline latex $6=\sqrt{42}$

### Code Blocks

```json
{
  "6.141": "normal",
  "16.405": "woke",
  "no_sleep": "spoke"
}
```

```python
{
def do_something_productive():
  if not_productive:
    do_work()
  else:
    cry()
}
```