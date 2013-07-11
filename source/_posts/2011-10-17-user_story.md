---
author: Kent Chiu
published: true
layout: post
title: "User Story簡介"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - pm
---




這邊採用[Behavior\_Driven\_Development](http://en.wikipedia.org/wiki/Behavior_Driven_Development "http://en.wikipedia.org/wiki/Behavior_Driven_Development")所建議的User
Story格式進行撰寫，簡介可以看[這裡](http://dannorth.net/whats-in-a-story "http://dannorth.net/whats-in-a-story")或者是[這裡](http://c2.com/cgi/wiki?UserStoryTemplate "http://c2.com/cgi/wiki?UserStoryTemplate")


```
    Title (one line describing the story)
     
    Narrative:
    As a [role]
    I want [feature]
    So that [benefit]
     
    Acceptance Criteria: (presented as Scenarios)
     
    Scenario 1: Title
    Given [context] And [some more context]...
    When [event]
    Then [outcome] And [another outcome]...
     
    Scenario 2: ...
```


```
    Story: Account Holder withdraws cash
     
    As an Account Holder
    I want to withdraw cash from an ATM
    So that I can get money when the bank is closed
     
    Scenario 1: Account has sufficient funds
    Given the account balance is \$100 And the card is valid And the machine contains enough money
    When the Account Holder requests \$20
    Then the ATM should dispense \$20 And the account balance should be \$80 And the card should be returned
     
    Scenario 2: Account has insufficient funds
    Given the account balance is \$10 And the card is valid And the machine contains enough money
    When the Account Holder requests \$20
    Then the ATM should not dispense any money And the ATM should say there are insufficient funds And the account balance should be \$20 And the card should be returned
     
    Scenario 3: Card has been disabled
    Given the card is disabled
    When the Account Holder requests \$20
    Then the ATM should retain the card
    And the ATM should say the card has been retained
     
    Scenario 4: The ATM has insufficient funds
    ...
```

