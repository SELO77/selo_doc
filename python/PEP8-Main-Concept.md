Tags: python, pep, pythonic, coding convention

# PEP8 Main Concept

## Introduction
This document gives coding conventions for the Python code comprising the standard library in the main Python distribution.

이 문서는 파이썬 배포판 표준 라이브러리를 구성하는 파이썬 코드의 코딩 컨벤션을 제공합니다.

This style guide evolves over time as additional conventions are identified and past conventions are rendered obsolete by changes in the language itself.

이 스타일 가이드는 시간이 지남에 따라 발전합니다. 추가적인 컨벤션이 명시되거나 언어의 변화로 과거 컨벤션이 필요없을 때 갱신됩니다.

Many projects have their own coding style guidelines. In the event of any conflicts, such project-specific guides take precedence for that project.

## A Foolish Consistency is the Hobgoblin of Little Minds
One of Guido's key insights is that ***code is read much more often than it is written.*** The guidelines provided here are intended to improve the readability of code and make it consistent across the wide spectrum of Python code. As [PEP 20(Zen of Python)](https://www.python.org/dev/peps/pep-0020/) says, ***"Readability counts."***

> One of Guido's key insights is that ***code is read much more often than it is written.***
"우리는 코드를 작성하기보다는 훨씬 더 많이 읽는다." by Guido

A style guide is about **consistency**. **Consistency** with this style guide is important. **Consistency** within a project is more important. **Consistency** within one module or function is the most important.

However, know when to be inconsistent -- sometimes style guide recommendations just aren't applicable. When in doubt, use your best judgment. Look at other examples and decide what looks best. And don't hesitate to ask!