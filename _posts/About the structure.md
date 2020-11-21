# About the structure

## Why isn’t this obvious?

This structure is clear, and it works, but there is a reason why it’s not so obvious, and that is the way the characteristics of each quadrant of the documentation overlap with those of its neighbours in the scheme.

!['overview of the documentation system'](https://documentation.divio.com/_images/overview.png)

Each of the quadrants is similar to its two neighbours:

- *tutorials and how-to guides* are both concerned with **describing practical steps**
- *how-to guides and technical reference* are both **what we need when we are at work, coding**
- *reference guides and explanation* are both concerned with **theoretical knowledge**
- *tutorials and explanation* are both **most useful when we are studying**, rather than actually working

### The tendency to collapse

Given these overlaps, it’s not surprising that the different kinds of documentation become confused and mixed in with each other. In fact, there is a natural gravitational pull of these distinct types of documentation to each other, and it is hard to resist. Its effect is to collapse the structure, and that is why so much documentation looks like this:

!['collapse of the documentation structure'](https://documentation.divio.com/_images/collapse.png)

## Adoption of the system

Though it’s rare to find it clear examples of it used fully, a great deal of documentation recognises, in different ways, each of these four functions.

Good examples of the scheme in substantial projects include:

- the [Divio Developer Handbook](https://docs.divio.com/)
- [Django’s documentation](https://docs.djangoproject.com/en/3.0/#how-the-documentation-is-organized)
- [django CMS’s documentation](http://docs.django-cms.org/)

It’s possible to use the system even in very minimal documentation, for example [CoReport (an open-source COVID-19 reporting project)](https://docs.coreport.ch/). Here, applying the system creates a framework for future documentation, helping ensure that new material will conform.

Sometimes the documentation is so minimal that not all quadrants are ready to be represented, as in the case of [Getting started with Java and Spring-boot](https://github.com/flavours/getting-started-with-spring-boot/blob/master/README.md), which includes only a tutorial, how-to and reference material.

[But I never wanted to do DevOps!](https://workshop.no-devops.work/en/latest/explanation/index.html) is the written material that accompanies a popular workshop. The documentation strictly separate the [tutorial](https://workshop.no-devops.work/en/latest/the-workshop/index.html), the steps learners are to follow, from the *explanation* ([Further reading](https://workshop.no-devops.work/en/latest/explanation/index.html)). Both belong to the *most useful when we are studying* side of the system, so it’s natural to include them in a workshop.

In each case though, however minimal or even incomplete, the system is respected and the clear distinction between sections and their purposes will benefit the author and user right away, and help guide the expansion of the material as it develops in the future.

## About the analysis and its application

The analysis of documentation in this article is based on several years of experience writing and maintaining documentation, and much time spent considering how to improve it.

It’s also based on sound principles that come from a variety of disciplines. For example, its conception of tutorials has a pedagogical basis; it posits a tutor and a learner, and considers using software to be a craft in which abstract understanding of general principles follows from concrete steps that deal with particulars.

The system is presented regularly at talks and interactive workshops. The analysis has been applied to numerous projects, including large internal documentation sets, and has repeatedly procured benefits of usability and maintainability, across a very wide range of technical subject matter.



3353/5000

구조에 대해
왜 이것이 분명하지 않습니까?
이 구조는 명확하고 작동하지만 그렇게 명확하지 않은 이유가 있습니다. 이것이 문서의 각 사분면의 특성이 계획에서 이웃의 특성과 겹치는 방식입니다.

'문서 시스템 개요'
각 사분면은 두 이웃과 유사합니다.

튜토리얼과 방법 가이드는 모두 실제 단계를 설명하는 것과 관련이 있습니다.
방법 가이드와 기술 참조는 우리가 일하고 코딩 할 때 필요한 것입니다.
참조 가이드와 설명은 모두 이론적 지식과 관련이 있습니다.
튜토리얼과 설명은 실제로 작업하는 것보다 공부할 때 가장 유용합니다.
무너지는 경향
이러한 겹침을 감안할 때 서로 다른 종류의 문서가 서로 혼동되고 혼합되는 것은 놀라운 일이 아닙니다. 사실, 이러한 고유 한 유형의 문서가 서로에게 자연스럽게 중력을 가하고 있으며 저항하기가 어렵습니다. 그 효과는 구조를 무너 뜨리는 것이며, 그래서 많은 문서가 다음과 같이 보입니다.

'문서 구조의 붕괴'
시스템 채택
완전히 사용 된 명확한 예를 찾기는 어렵지만 많은 문서에서 이러한 네 가지 기능을 각각 다른 방식으로 인식합니다.

실질적인 프로젝트에서 계획의 좋은 예는 다음과 같습니다.

Divio 개발자 핸드북
Django의 문서
장고 CMS 문서
CoReport (오픈 소스 COVID-19보고 프로젝트)와 같이 아주 최소한의 문서에서도 시스템을 사용할 수 있습니다. 여기에서 시스템을 적용하면 향후 문서화를위한 프레임 워크가 생성되어 새로운 자료가 준수되는지 확인할 수 있습니다.

때로는 설명서, 방법 및 참조 자료 만 포함 된 Java 및 Spring-boot 시작하기의 경우처럼 문서가 너무 작아서 모든 사분면을 표현할 준비가되지 않았습니다.

하지만 DevOps를하고 싶지 않았습니다! 인기있는 워크샵과 함께 제공되는 글입니다. 설명서는 자습서, 학습자가 따라야 할 단계와 설명 (추가 읽기)을 엄격하게 구분합니다. 둘 다 시스템 측면을 연구 할 때 가장 유용하므로 워크숍에 포함하는 것이 당연합니다.

비록 미미하거나 불완전하더라도 각각의 경우 시스템이 존중되고 섹션과 그 목적 사이의 명확한 구분은 작성자와 사용자에게 즉시 도움이되며 향후 개발되는 자료의 확장을 안내하는 데 도움이됩니다.

분석 및 적용 정보
이 기사의 문서 분석은 문서를 작성하고 유지 관리 한 수년 간의 경험과 문서를 개선하는 방법을 고려하는 데 많은 시간을 투자 한 것입니다.

또한 다양한 분야에서 나온 건전한 원칙을 기반으로합니다. 예를 들어, 튜토리얼의 개념은 교육적 기반을 가지고 있습니다. 튜터와 학습자를 배치하고 소프트웨어를 사용하여 특정 사항을 다루는 구체적인 단계에서 일반 원칙에 대한 추상적 이해를 따르는 기술이라고 생각합니다.

이 시스템은 대화 및 대화 형 워크숍에서 정기적으로 제공됩니다. 분석은 대규모 내부 문서 세트를 포함하여 수많은 프로젝트에 적용되었으며 매우 광범위한 기술 주제에 걸쳐 사용 성과 유지 보수성의 이점을 반복적으로 확보했습니다.