# GSoC 2022&2023 Project Blog

Blog link: https://faye-yufan.github.io/gsoc22-animint/

# Summary of my GSoC 2023
## Basic Info
- **Name**: Yufan Fei
- **Email**: yufanfei8@gmail.com
- **Github Username**: Faye-yufan
- **Mentors**: Toby Dylan Hocking, Faizan Uddin Fahad Khan
- **Project Title**: Enhance Selection, Viz & Automation
- **Project Link**: [animint2](https://github.com/animint/animint2)

## Work Summary
| Pull Requests   |      Issues      |
| :------ |:--- |
| [#104 Support for feature fill_off](https://github.com/animint/animint2/pull/104) | [#65 User configurable colour and fill for selected and deselected elements](https://github.com/animint/animint2/issues/65)  |
| [#82 Replace phantomjs with chrome as headless browser](https://github.com/animint/animint2/pull/82) | |
| [#92 Fix scrolling issue in Firefox](https://github.com/animint/animint2/pull/92) | [#90 Observations on a Scrolling Bug](https://github.com/animint/animint2/issues/90) |
| [#101 created animint2pages()](https://github.com/animint/animint2/pull/101) | [#84 move gallery from blocks to gh pages](https://github.com/animint/animint2/issues/84) |
| [#107 Refactor draw_geom() and turn into a Geom class for scalability](https://github.com/animint/animint2/pull/107) | [#48 Move scale range calculation from compiler to the renderer.](https://github.com/animint/animint2/issues/48) |

## **Overview**

Being part of Google Summer of Code again this year has been a fantastic trip. A big thank you goes to my mentor for giving me the chance to be on this project again and for trusting me with it. I have noticed a tangible improvement in my comfort level and confidence when it comes to coding, which I credit largely to the experiences and learnings of the past year.

Balancing a full-time job during the daylight hours while dedicating my evenings and weekends to developing for animint has indeed posed its challenges. The increased responsibilities and the reduced time for development have made this year's journey a bit more tough compared to last year. Nevertheless, the support and understanding from my mentor have been important in navigating these challenges. His willingness to provide a flexible coding schedule has helped me a lot in managing my commitments effectively.

This GSoC project has really opened my eyes to so much more than just R programming. I got to learn about SSH tokens, which help keep online connections safe and private. I also dived into object-oriented programming, which is a smart way to organize code.

I even got to have some research and learned headless testing, which is a great way to check that software works right without having to click around manually, and learning the latest testing suite development is really fun.

Overall, this year’s Google Summer of Code has been a lot more than just writing code. It's been about learning new things, figuring out how to adapt, and always chasing after more knowledge. I'm really excited to use what I've learned to make the project better and to help out more in the open source community.

# Summary of my GSoC 2022
## Basic Info
- **Contributor**: Yufan Fei [@Faye-yufan](https://github.com/Faye-yufan)
- **Email**: yufanfei8@gmail.com
- **Mentors**: Toby Dylan Hocking [@tdhock](https://github.com/tdhock), Carson Sievert [@cpsievert](https://github.com/cpsievert)
- **Project Title**: Improving Animated Interactive GGPLOT
- **Project Link**: https://github.com/tdhock/animint2

## Overview
During the first three of work, I started this blog and fixing some tests warning as my first 'introduction' PR. This period of time is more about getting familar with the whole structure of the project and getting used to using R to test.  

As the middle of June, I started my first 'officially' working PR, which is optimize the speed of compiler. In this part of time, I started getting more familiar with the compiler side of code.  
However, it did not go as well as I expected, and the speed seems did not improve much after 1-2 weeks work, so my mentor suggested me to switch to a more easier task.  Then I switched to aesthetic aspect of the project, implementing visual feature.  

In July, when I was coding for the new features, thanks to my mentor still patiently guided me on the compiler issue, finally I got some improvements on compiler speed. Not until working on the PR for text size, I gradually realized how the JS side code is functioning. I was able to keep up with the project.  

When it comes to August, I could feel my coding process becoming more and more smooth. And I implemented another new feature `color_off` and fix some bugs as well.  

I learned a lot throughout this summer thanks to GSoC. This experience leads to my deep interest in coding for open source project.

## Work Summary
| Pull Requests   |      Issues      |
|----------------|----------------|
| [#58 Fix test warnings](https://github.com/tdhock/animint2/pull/58) | [#56 fix test warnings](https://github.com/tdhock/animint2/issues/56)  |
| [#61 Fix warning in 'make_text' func, renderer3 and compiler](https://github.com/tdhock/animint2/pull/61) | [#56 fix test warnings](https://github.com/tdhock/animint2/issues/56) |
| [#63 Improve getCommonChunk() speed in compiler](https://github.com/tdhock/animint2/pull/63) | [#57 Compiler speed improvements with many selection subsets](https://github.com/tdhock/animint2/issues/57) |
| [#66 Add support for legend/axis text size ](https://github.com/tdhock/animint2/pull/66) | [#64 Support for specifying legend/axis font size via theme()](https://github.com/tdhock/animint2/issues/64) |
| [#69 Change ggplot legend theme back to default](https://github.com/tdhock/animint2/pull/69) |  |
| [#72 Support for feature colour_off and use it with alpha_off ](https://github.com/tdhock/animint2/pull/72) | [#59 User configurable alpha for selected and deselected elements](https://github.com/tdhock/animint2/issues/59) |
| [#73 Fix issue geom_contour error ](https://github.com/tdhock/animint2/pull/73) | [#28 geom_contour error after loading ggplot2](https://github.com/tdhock/animint2/issues/28) |
| [#75 Fix data.table bug](https://github.com/tdhock/animint2/pull/75) | [#74 data table bug after compiler speed improvements](https://github.com/tdhock/animint2/issues/74) |
| [#78 fix legend entry text size with theme grey ](https://github.com/tdhock/animint2/pull/78) |  |

## Future Work
- Configurable axis titles/plot title/facet titles text size.
- Now `color_off` is availiable to use, and in the vain of this, we can also do the same thing to `fill`, `size`, these aesthetic parameters.
- Aesthetic feature of animint2 still needs more test cases, for example, in [issue #77](https://github.com/tdhock/animint2/issues/77), when using color_off in geom_point, stroke-width is too small to see stroke change. Maybe we should find a 'magic' default value for stroke-width.  This is also unavoidable when implementing other aesthetic features.
- Is there any way to optimize the selection and de-selection logic in `animint.js`? For example, when `get_color_off` function is called, it can also update the selection color.
- Currently the default axis style in animint2 is not aligned with ggplot2, which probably be better to remove the black axis line on JS.

## My expierence
The past five months is so fast that I cannot believe it is over now. The first time I came across the Google Summer of Code was actually on a Chinese social platform where a previous GSoC student shared his experience and he highly recommanded others to participate into. But I never thought about I can actually be selected as him at that time, because his background is way much better than me.  

After starting my master's program, I often played with data and dazzling charts and graphs, and of course, R and ggplot. So when I first discovered this project I was so excited, it is so cool to see the interactive animation in R after so many static red and green plots. And I started playing with it, which is really interesting from a user's perspective, at the same time, I drafted my purposal and did the tests.  

I can still remember 11AM, May 20, I was at the gym and could not help but checking the email inbox. 11:01, 11:05, nothing. I thought that's it, the proposal just didn't get selected, probably the broken English is why. And I never forget the feeling receiving the acceptance email around 11:34, that unreal thrill.  

The first time commiting to the animint2 repo, I was always nervous about all the CI tests would fail and checked several times on my local docker environment. Luckily, things got better day after day, as I got more familar with the code structure and more comfortable to commit without hesitation.  

It is true that sometimes when you facing a unsolved error in your code and you would feel frustrated. But I found those moments are somewhere l learned the most and can get a great sense of accomplishment after resolving the problem. To be honest, one of the most important things I learned during this exprience, which I am still not good at, is to ask questions or for help, whether to your mentors or your teammates. Since you never know what kind of excllent ideas they have that is out of your box.  

## Acknowledgments
First I'd like to thank my mentor, Dr. Toby Dylan Hocking, as he teaches and helps me a lot in this summer. He always patiently guided me through the coding process, taught me how to write well-structured code and always very quick to respond to my questions and point out my mistakes immediately. Also, he shed light on many issues, and I'm always amazed that he could come up with these incredible ideas and solutions so quickly. And appriciate for putting up with my broken English in every one-on-one meeting.

Charles Saluski also helped me a lot on getting my hand on the project. I really appriciate those brain storm meetings we had, and he always generously shared great informations and ideas.

Also, my R-GSoC mates, Fabrizio and Daniel, we always caught up with each other, and thanks for those kind words and encouragement.
