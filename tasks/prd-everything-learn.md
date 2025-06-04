# Product Requirements Document: "Orion Path" or "Eurus monopati" - AI-Powered, Blockchain-Certified Learning Platform

## Introduction/Overview

Everything Learn is an adaptive learning platform combining AI-powered content generation with blockchain-certified credentials. The system addresses the need for personalized, verifiable education paths in technical domains by offering dynamic course creation, intelligent assessment systems, and tamper-proof certification.

## Primary Goal: 
Create a scalable learning ecosystem where users can:
* Generate personalized courses within 2 minutes
* Receive AI-proctored assessments
* Earn immutable blockchain certificates


## User Stories
As a Learner:
I want to input "Stoics Basics" and get Level 3 content so I can maintain conversation about this topic in 3 hours.
I need a simulation of a test conversation with a "friend" to verify my understanding through conversation

As an Employer:
I want to verify blockchain certificates to ensure candidate skills match claimed expertise


## Functional Requirements MVP

### Authentication
Yes. we def need to make authentication :(
Can we pretty please make google auth?
 
### Subscriptions
Logged in user should be able to subsccribe with RevenueCat to one of 3 levels of subscriptions:
1. Free: recommended to test out the platform. Best suites for very small topics
2. Basic: only 1-3 depth level courses. Max 5 courses.  
3. Medium: 1-4 level courses. Limited video functions.  
4. Large: 1-5 levels. More video content. Video exam. Certificate. 

### Dynamic Course Builder
#### Info:
Main page should provide an introduction into the project: 
* Give some course examples
* Explain correlation between depth and seriosness/size of the topic through examples 
* Explain the limitations:
  * Morality. No "how do I laundry money" courses.
  * No private data courses. We can't build "Internal oracle policies" course. 
* Explain the possible tests
* Explain the certificate


#### Prompt:
* System must accept free-form topic input, ex. Python Basics
* System must accept free-form "goal" input. ex. To be able to write a small command line app
* Based on the topic and the goal, the system must suggest minimum course size and depth. 
* User can change course depth and length to customize it. 
* If the course can't fit into requested size system must suggest a smaller scope course or o bigger course length, or accept the terms with a "Accuracy warning" disclamer. 
* Depth selector must offer 5 discrete levels (1=basic → 5=expert)

#### Output: 
AI must generate course outlines with:
* The list of open licensed curated resources from ≥3 verified sources per concept
* 3-5 modules per depth level
* 1-10 topics per module. 
  * Usually, keep it under 5 topics. 
  * Topic must be under 15 minutes

Must save course structure into the database.
Must save link between user and the course into the database.


### Learning platform:
Once the course structure is generated, user should be able to see their courses. 
When choosing a course, shoud be able to journey module by module. 
All modules but the first one should be "locked" until user completes the previous module and passes tests.

#### Course content
User must see: 
* all course structure: modules and topics names.
* the content of topics of unlocked modules.

#### Content Availability:
* When pressing on the topic, the user should be able to see content of the topic.
* Passing tests in the end of the module unlocks the next module. 


### Reinforcement Engine
In the end of the topic, 
Assessment system must support:
* Multiple choice quizzes (auto-generated per section)
* Open-ended questions with GPT-4 evaluation
* Video-based oral exams with emotion analysis

Final exams must include:
* 20-minute time limit
* 3-strike cheating detection (eye tracking, tab switching)



## Functional Requirements Extra:
#### AI Video Tutor

#### Blockchain Certification
Algorand smart contracts must:
Store: Course depth, final score, AI tutor feedback
Generate shareable NFT certificates
Certificate verification portal must show:
Completion date
Skill breakdown by module


# Non-Goals
Real-time collaboration features
Mobile app development (web-only initially)
Offline course access
User-generated content marketplace
