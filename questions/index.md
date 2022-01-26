---
layout: default
title: Questions-Answers engine
nav_order: 99
has_children: true
---

THIS IMAGE IS ON DEVELOPMENT.
THIS IS JUST ANNOUNCEMENT.

Please, contact us to [support@perfumerlabs.com](mailto:support@perfumerlabs.com) if you want to get this container.
This will encourage us to speed up its development.
We will try to prioritize this task and pack it as soon as possible.

What is it
==========

This is container providing special self-made engine to construct questionnaires.
You upload YAML files containing questions-answers tree-like structure,
then you can request API with answers your user chosen.
All logics about how to order questions and conditions this image takes by itself.

Note, that this is only backend API logic, all client side design you have to do on your side.

Usage
-----

Consider we have next sample yaml:

```yml
- q: Your favourite food?
  many:
    - a: Burger
      t:
        - q: How old are you?
          one:
            - a: 1-10
            - a: 11-20
            - a: 21-30
    - a: Pizza
      t:
        - q: Which size is your cloth?
          one:
            - a: S
            - a: M
            - a: L
            - a:
              t:
                - q*: What is your surname?
    - a: Chips
- q: Where are you from?
```

Lets take closer look, what is happening here.

Small letter "q" defines question text.
So, the first question customer have to answer is "Your favourite food?".

Keyword "many" means that it should be a question with multiple possible answers, with multiple choice (in html it can be made by checkbox or multiple select probably).
Small letter "a" defines text of possible answer.
In this case 3 answer variants are suggested: Burger, Pizza and Chips.

If customer choose first variant Burger, then user must be prompted to answer the question, which is situated under the letter "t" right below "a".
So, if Burger is chosen, next question must be "How old are you?".

Key word "one" means that it should be a question with multiple possible answers, but with only one possible choice (in html it can be made by radio-button or regular select probably).
Small letter "a" defines text of possible answer.
In this case 3 answer variants are suggested: 1-10, 11-20 and 21-30.

If customer choose Pizza on the first question, then question "Which size is your cloth?" will be next respectively.
It is again "radiobutton" question because of "one" keyword.
There are 3 variants of answer, but also there is "a" with empty string - this means that this is custom answer.
So, on design it must be 3 radio-buttons and 1 text input for own answer text.

If user choose own answer then next answer should be "What is your surname?".
Notice, there is no keywords under the text.
This means that this is just text input (i.e. <input-text> html tag).
Also there is a star near the "q" letter.
This means that this question is required.

If user choose in first question both Burger and Pizza, then engine follows, firstly, "Burger"-branch, and then it will follow "Pizza"-branch.

And after all questions the last question will be "Where are you from?", because it is situated on the same alignment level as first question "Your favourite food?".
Questions on same levels are supposed to have same priority level, and go one-by-one.

Size of yaml file is not limited and you can write as many questions as you want.
Thus, you can construct very complicated questionnaires.

API
---

API is not stable yet.