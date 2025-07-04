# Tasks.

## Stories
### Architecture
-[x] Break down app into smaller sub-webapps that would be easier to generate with bolt. Backend/frontend always separately.
-[x] Create main DDLs for:
  -[] User
  -[] Course Configuration
    - Topic
    - Goal
    - Depth
    <!-- - Resources (main resourses to use for consistent content generation) -->
  -[] Course
    - Module
    - Topic
      - topic description
      - questions
    - Topic content
  -[] Course Progress
    - User
    - Course
    - Module
      - Topic
        - Exam Passed?

### Contract with AI        
#### Request:
Course Configuration
    - Topic
    - Goal
    - Depth

#### Response:
Syllabus:    
  Modules[]:
    Topics[]:
      Summary: String
      Keywords: String[]
  Keywords: String[]     

### Landing
-[] Create landing page with:
  -[] Course examples showcase
  -[] Depth level explanation with examples
  -[] System limitations section
    -[] Morality guidelines
    -[] Private data restrictions
  -[] Test types explanation
  -[] Certificate system overview

### Authentication
-[] Implement Google OAuth integration / some other auth
-[x] Create user profile storage
 
### Subscription
-[] Integrate RevenueCat
-[] Persist (or fetch?) subscription info
-[] Create subscription tiers:
  -[] Basic (Levels 1-3, max 5 courses)
  -[] Medium (Levels 1-4, limited video)
  -[] Large (Levels 1-5, full features)
Extra:
-[] Create subscription upgrade/downgrade flow

### Course Builder
-[x] Create course input form:
-[x] Topic input field
-[x] Goal input field
-[x] Depth selector (1-5)
-[x] Implement AI course generation:
-[x] Course structure generation with strict schema
-[] Size/depth compatibility warning



Extra:
-[] Add course validation:
  -[] Size/depth compatibility checks
  -[] Accuracy warning system
-[] Save course structure to the database
-[] Size/depth compatibility warning

### Learning Platform
-[] Create course dashboard:
  -[] Course list view
-[] Implement course page container:
  -[] Modules list on the left
  -[] Topics sub-list
-[] Create topic viewer:
  -[] Content generation:
    - Generate content with ai based on the topic description, prev and next topic for context. Markdown?
    - Save content to the DB. 
  -[] Content display
    - Request from BE-content for given topic.
    - On BE either generate, or return the content.
  -[] Resource links
  -[] Progress tracking
-[] Module locking system
  -[] Progress-based unlocking

Extra: 
  -[] Progress tracking

### Reinforcement Engine
-[] Implement assessment types:
  -[] Multiple choice quiz generator
  -[] GPT-4 evaluation system
  -[] Emotion analysis integration
-[] Create exam system:
  -[] 20-minute timer
  -[] Anti-cheating measures:
    <!-- -[] Eye tracking -->
    -[] Tab switching detection
  <!-- -[] 3-strike system -->
-[] Build progress tracking:
  -[] Module completion status
  -[] Test results storage
  -[] Overall progress calculation

### Video Tutor
-[] Set up video recording system
-[] Implement AI tutor interface
-[] Create feedback system
-[] Add progress tracking

### Video Exam
-[] Create video exam interface
-[] Implement emotion analysis
-[] Set up recording system
-[] Add evaluation criteria

### Certification
-[] Implement Algorand smart contracts:
  -[] Course data storage
  -[] Score recording
  -[] Tutor feedback storage
-[] Create NFT certificate generation
-[] Build verification portal:
  -[] Completion date display
  -[] Module skill breakdown
  -[] Certificate sharing system
